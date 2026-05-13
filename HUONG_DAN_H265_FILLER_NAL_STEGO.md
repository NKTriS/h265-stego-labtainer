# Hướng dẫn đầy đủ bài lab h265-filler-nal-stego

## 1. Giới thiệu chung

Trong bài lab này, sinh viên đóng vai điều tra viên an toàn thông tin. SOC phát hiện một file video camera H.265 được xuất ra ngoài hệ thống kho hàng. Video nhìn bình thường, vẫn có thể kiểm tra bằng `ffprobe` và có thể xuất frame bằng `ffmpeg`, nhưng bên trong có một thông điệp bị giấu.

Kỹ thuật của bài lab là giấu tin trong **Filler Data NAL** của chuẩn H.265/HEVC. Filler Data NAL là loại NAL hợp lệ, thường bị bộ giải mã bỏ qua khi phát video. Vì vậy video vẫn xem được, nhưng vẫn có thể chứa dữ liệu ẩn.

Quy tắc giấu tin trong bài:

```text
số byte 0xff trong Filler NAL chẵn -> bit 0
số byte 0xff trong Filler NAL lẻ   -> bit 1
```

Bài này không dùng SEI, không nối dữ liệu ở cuối file, không dùng AUD, không dùng motion vector, không dùng intra prediction và không dùng lượng tử/QP.

## 2. Mục tiêu bài lab

Sau khi hoàn thành bài lab, sinh viên cần làm được các việc sau:

1. Cài và chạy một bài Labtainer bằng `imodule`.
2. So sánh bản video sạch và bản video nghi vấn.
3. Xác nhận file nghi vấn vẫn là video H.265 thật.
4. Quét NAL unit để phát hiện Filler Data NAL bất thường.
5. Phân tích số byte `0xff` trong Filler NAL.
6. Sửa một đoạn code nhỏ để khôi phục thông điệp.
7. Đối chiếu log audit để tìm user đáng ngờ.
8. Viết kết luận điều tra và kiểm tra bằng `checkwork`.

## 3. Cài bài lab

Trên máy Labtainer của sinh viên, mở terminal và chạy:

```bash
cd ~/labtainer/labtainer-student
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-filler-nal-stego.tar.gz
labtainer h265-filler-nal-stego
```

Nếu Labtainer không tự tải được Docker image, chạy thêm:

```bash
docker pull nktris/h265-filler-nal-stego.workstation.student:latest
labtainer h265-filler-nal-stego
```

Khi lab chạy thành công, một terminal `workstation` sẽ mở ra. Toàn bộ thao tác làm bài được thực hiện trong terminal `ubuntu@workstation`.

## 4. Xóa bản cũ để chạy lại

Nếu sinh viên đã từng chạy lab và muốn làm lại từ đầu:

```bash
cd ~/labtainer/labtainer-student
stoplab h265-filler-nal-stego 2>/dev/null || true
docker rm -f h265-filler-nal-stego.workstation.student h265-filler-nal-stego-igrader 2>/dev/null || true
docker rmi -f nktris/h265-filler-nal-stego.workstation.student:latest 2>/dev/null || true
rm -rf ~/labtainer/trunk/labs/h265-filler-nal-stego
rm -rf ~/labtainer_xfer/h265-filler-nal-stego
```

Sau đó cài lại bằng lệnh `imodule` ở phần trên.

## 5. Thực hành

### Task 1 - So sánh hai file chứng cứ

Trong terminal `workstation`, xem thư mục chứng cứ:

```bash
ls evidence
```

Trong thư mục này có hai file:

```text
warehouse-clean.hevc
warehouse-suspect.hevc
```

Tính hash cho hai file:

```bash
sha256sum evidence/warehouse-clean.hevc evidence/warehouse-suspect.hevc > sha256sum.stdout
cat sha256sum.stdout
```

Nếu hai hash khác nhau, điều đó cho thấy bản nghi vấn đã bị thay đổi so với bản sạch.

### Task 2 - Kiểm tra file nghi vấn là video H.265 thật

Kiểm tra loại file:

```bash
file evidence/warehouse-clean.hevc
file evidence/warehouse-suspect.hevc
```

Kiểm tra thông tin video bằng `ffprobe`:

```bash
ffprobe -v error -select_streams v:0 -show_entries stream=codec_name,width,height,r_frame_rate -of default=noprint_wrappers=1 evidence/warehouse-suspect.hevc > ffprobe.stdout
cat ffprobe.stdout
```

Kết quả cần có:

```text
codec_name=hevc
```

Nếu muốn kiểm tra video có thể xem được, sinh viên có thể xuất một frame:

```bash
ffmpeg -v error -i evidence/warehouse-suspect.hevc -frames:v 1 suspect_frame.jpg
ls -lh suspect_frame.jpg
```

Nếu môi trường có hỗ trợ giao diện video, có thể xem bằng:

```bash
ffplay evidence/warehouse-suspect.hevc
```

### Task 3 - So sánh NAL unit giữa bản sạch và bản nghi vấn

Chạy tool so sánh NAL:

```bash
python3 tools/nal_compare.py evidence/warehouse-clean.hevc evidence/warehouse-suspect.hevc > nal_compare.py.stdout
cat nal_compare.py.stdout
```

Quan sát các dòng:

```text
CLEAN_FILLER=0
SUSPECT_FILLER=248
FILLER_DELTA=248
```

Ý nghĩa: bản sạch không có Filler NAL, còn bản nghi vấn có nhiều Filler NAL. Đây là dấu hiệu bất thường.

### Task 4 - Dump Filler Data NAL

Chạy tool dump Filler NAL:

```bash
python3 tools/filler_dump.py evidence/warehouse-suspect.hevc > filler_dump.py.stdout
cat filler_dump.py.stdout
```

Output có dạng:

```text
rec nal offset payload_len ff_count parity
000 ... ... ... 004 0
001 ... ... ... 005 1
```

Cột quan trọng là:

```text
ff_count
parity
```

Trong bài này, parity của `ff_count` chính là bit bị giấu.

### Task 5 - Sửa extractor để khôi phục thông điệp

Mở file extractor:

```bash
nano tools/filler_extract.py
```

Tìm hai dòng có `TODO`.

Sửa dòng lấy bit thành:

```python
bit = rec["ff_count"] % 2
```

Sửa dòng giải mã message thành:

```python
message = bits_to_text(bits)
```

Lưu file trong nano:

```text
Ctrl + O
Enter
Ctrl + X
```

Chạy lại extractor:

```bash
python3 tools/filler_extract.py evidence/warehouse-suspect.hevc > filler_extract.py.stdout
cat filler_extract.py.stdout
```

Nếu làm đúng, kết quả sẽ có:

```text
FILLER_MESSAGE=HEVC{filler_nal_length_channel}
```

### Task 6 - Đối chiếu log audit

Đọc log:

```bash
cat evidence/export_audit.log
```

Tìm dòng upload:

```bash
grep uploaded evidence/export_audit.log > grep.stdout
cat grep.stdout
```

User đáng ngờ trong log là:

```text
ops-video12
```

### Task 7 - Viết kết luận điều tra

Tạo file kết luận:

```bash
nano answer.txt
```

Ghi đúng một dòng:

```text
ops-video12 HEVC{filler_nal_length_channel} FILLER_NAL_LENGTH
```

Lưu file:

```text
Ctrl + O
Enter
Ctrl + X
```

Ý nghĩa kết luận:

```text
ops-video12                  user đáng ngờ
HEVC{filler_nal_length_channel}  thông điệp khôi phục
FILLER_NAL_LENGTH            kỹ thuật giấu tin
```

## 6. Kiểm tra kết quả

Quay lại terminal student hoặc mở terminal mới trên máy Labtainer, chạy:

```bash
cd ~/labtainer/labtainer-student
checkwork h265-filler-nal-stego
```

Nếu làm đúng, kết quả sẽ là:

```text
Y - cw1
Y - cw2
Y - cw3
Y - cw4
Y - cw5
Y - cw6
Y - cw7
```

## 7. Các file sinh viên cần tạo ra

Sau khi hoàn thành, trong home của container `workstation` cần có các file:

```text
sha256sum.stdout
ffprobe.stdout
nal_compare.py.stdout
filler_dump.py.stdout
filler_extract.py.stdout
grep.stdout
answer.txt
```

`checkwork` sẽ tự động kiểm tra các file này. Sinh viên không cần nhập checkword thủ công.

## 8. Tóm tắt lệnh làm bài

```bash
sha256sum evidence/warehouse-clean.hevc evidence/warehouse-suspect.hevc > sha256sum.stdout

ffprobe -v error -select_streams v:0 -show_entries stream=codec_name,width,height,r_frame_rate -of default=noprint_wrappers=1 evidence/warehouse-suspect.hevc > ffprobe.stdout

python3 tools/nal_compare.py evidence/warehouse-clean.hevc evidence/warehouse-suspect.hevc > nal_compare.py.stdout

python3 tools/filler_dump.py evidence/warehouse-suspect.hevc > filler_dump.py.stdout

nano tools/filler_extract.py

python3 tools/filler_extract.py evidence/warehouse-suspect.hevc > filler_extract.py.stdout

grep uploaded evidence/export_audit.log > grep.stdout

nano answer.txt
```

Nội dung `answer.txt`:

```text
ops-video12 HEVC{filler_nal_length_channel} FILLER_NAL_LENGTH
```

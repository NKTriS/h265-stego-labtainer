# H.265 Steganography Labtainer Labs

Repo này chứa các bài lab Labtainer về giấu tin trong video H.265/HEVC.

## Lab 1: h265-stego

Phân tích giấu tin trong SEI và VCL slice.

Hướng dẫn HTML: [HUONG_DAN_H265_STEGO.html](HUONG_DAN_H265_STEGO.html)

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-stego.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
docker pull nktris/h265-stego.workstation.student:latest
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

## Lab 2: h265-cctv-motion-leak

Điều tra rò rỉ CCTV H.265 qua kênh motion-vector LSB. Lab có 2 container: `analyst` và `evidence-server`.

Hướng dẫn HTML: [HUONG_DAN_H265_CCTV_MOTION_LEAK.html](HUONG_DAN_H265_CCTV_MOTION_LEAK.html)

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-cctv-motion-leak.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
docker pull nktris/h265-cctv-motion-leak.analyst.student:latest
docker pull nktris/h265-cctv-motion-leak.evidence-server.student:latest
cd ~/labtainer/labtainer-student
labtainer h265-cctv-motion-leak
```

## Lab 3: h265-filler-nal-stego

Điều tra giấu tin trong H.265 bằng Filler Data NAL. Lab có hai video HEVC thật: bản sạch và bản nghi vấn. Sinh viên có thể so sánh hash, dùng `ffprobe`, xuất frame bằng `ffmpeg`, quét NAL và sửa extractor nhỏ để khôi phục thông điệp.

Hướng dẫn đầy đủ:

- [HUONG_DAN_H265_FILLER_NAL_STEGO.md](HUONG_DAN_H265_FILLER_NAL_STEGO.md)
- [HUONG_DAN_H265_FILLER_NAL_STEGO.html](HUONG_DAN_H265_FILLER_NAL_STEGO.html)

```bash
cd ~/labtainer/labtainer-student
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-filler-nal-stego.tar.gz
labtainer h265-filler-nal-stego
```

Nếu máy không tự kéo image:

```bash
docker pull nktris/h265-filler-nal-stego.workstation.student:latest
```

Kiểm tra:

```bash
checkwork h265-filler-nal-stego
```

Các lab không yêu cầu nhập checkword thủ công; Labtainer tự chấm qua `checkwork`.

## Lab 4: h265-cabac-merge-stego

Thực hành giấu tin trong H.265 bằng parity của `merge_idx` trong CABAC/merge-mode trace.

Hướng dẫn HTML: [HUONG_DAN_H265_CABAC_MERGE_STEGO.html](HUONG_DAN_H265_CABAC_MERGE_STEGO.html)

```bash
cd ~/labtainer/labtainer-student
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-cabac-merge-stego.tar.gz
labtainer h265-cabac-merge-stego
```

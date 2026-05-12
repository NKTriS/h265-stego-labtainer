# H.265 Steganography Labtainer Labs

Repo này chứa các bài lab Labtainer về giấu tin trong video H.265/HEVC.

## Lab 1: h265-stego

Phân tích giấu tin trong SEI và VCL slice.

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-stego.tar.gz
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

## Lab 2: h265-cctv-motion-leak

Điều tra rò rỉ CCTV H.265 qua kênh motion-vector LSB. Lab có 2 container: `analyst` và `evidence-server`.

Bản mới dùng HTTP port `8080`:

```text
http://evidence-server:8080/case/cctv.hevc
http://evidence-server:8080/logs/cctv_export.log
```

Nếu đã từng chạy bản cũ, xóa sạch trước:

```bash
cd ~/labtainer/labtainer-student
stoplab h265-cctv-motion-leak 2>/dev/null || true
docker rm -f h265-cctv-motion-leak.analyst.student h265-cctv-motion-leak.evidence-server.student h265-cctv-motion-leak-igrader 2>/dev/null || true
docker rmi -f nktris/h265-cctv-motion-leak.analyst.student:latest nktris/h265-cctv-motion-leak.evidence-server.student:latest 2>/dev/null || true
rm -rf ~/labtainer/trunk/labs/h265-cctv-motion-leak
rm -rf ~/labtainer/trunk/scripts/labtainer-student/.tmp/h265-cctv-motion-leak
rm -rf ~/labtainer_xfer/h265-cctv-motion-leak
```

Cài lại:

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-cctv-motion-leak.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
docker pull nktris/h265-cctv-motion-leak.analyst.student:latest
docker pull nktris/h265-cctv-motion-leak.evidence-server.student:latest
labtainer h265-cctv-motion-leak
```

Kiểm tra:

```bash
checkwork h265-cctv-motion-leak
```

Các lab không yêu cầu nhập checkword thủ công; Labtainer tự chấm qua `checkwork`.

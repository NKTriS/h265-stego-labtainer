# H.265 Steganography Labtainer Labs

Repo này chứa các bài lab Labtainer về giấu tin trong video H.265/HEVC.

## Lab 1: h265-stego

Phân tích giấu tin trong SEI và VCL slice.

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-stego.tar.gz
```

Chạy:

```bash
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

## Lab 2: h265-cctv-motion-leak

Điều tra rò rỉ CCTV H.265 qua kênh motion-vector LSB. Lab có 2 container: `analyst` và `evidence-server`.

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-cctv-motion-leak.tar.gz
```

Nếu không có `imodule`, tải bằng `curl`:

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-cctv-motion-leak.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
```

Chạy:

```bash
cd ~/labtainer/labtainer-student
labtainer h265-cctv-motion-leak
```

Kiểm tra:

```bash
checkwork h265-cctv-motion-leak
```

Các lab không yêu cầu nhập checkword thủ công; Labtainer tự chấm qua `checkwork`.

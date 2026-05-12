# h265-stego Labtainer Lab

Bài lab Labtainer phân tích giấu tin trong video H.265/HEVC.

Docker image của lab đã được đẩy lên DockerHub:

```text
nktris/h265-stego.h265-stego.student:latest
```

Vì vậy trên máy có Internet/DockerHub, sinh viên chỉ cần tải gói lab rồi chạy, không cần build image thủ công.

## Cài đặt bằng imodule

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-stego.tar.gz
```

## Cài đặt bằng curl

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-stego.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
```

## Chạy lab

```bash
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

Nếu máy không truy cập được DockerHub, hãy build local:

```bash
cd ~/labtainer/labtainer-student
./bin/rebuild -L -b h265-stego
```

## Kiểm tra tiến độ

```bash
checkwork h265-stego
```

Bài lab không yêu cầu nhập checkword thủ công. Sinh viên làm theo hướng dẫn trong `HUONG_DAN_SINH_VIEN.md` được đóng gói bên trong file `h265-stego.tar.gz`.

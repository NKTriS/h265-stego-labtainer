# Cài đặt bài lab h265-cctv-motion-leak

Docker images đã có trên DockerHub:

```text
nktris/h265-cctv-motion-leak.analyst.student:latest
nktris/h265-cctv-motion-leak.evidence-server.student:latest
```

## 1. Tải bài lab

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-cctv-motion-leak.tar.gz
```

Nếu không có `imodule`:

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-cctv-motion-leak.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
```

## 2. Chạy lab

```bash
cd ~/labtainer/labtainer-student
labtainer h265-cctv-motion-leak
```

## 3. Kiểm tra

```bash
checkwork h265-cctv-motion-leak
```

Bài lab có 8 task và không yêu cầu nhập checkword thủ công.

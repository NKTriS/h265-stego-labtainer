# Cài đặt bài lab h265-stego từ GitHub

Docker image đã có trên DockerHub:

```text
nktris/h265-stego.workstation.student:latest
```

Vì vậy sinh viên chỉ cần tải gói lab rồi chạy `labtainer h265-stego`. Labtainer sẽ tự pull image nếu máy chưa có image.

## 1. Tải bài lab

Cách khuyến nghị:

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-stego.tar.gz
```

Nếu không có `imodule`, dùng `curl`:

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-stego.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
```

Kiểm tra sau khi tải:

```bash
ls ~/labtainer/trunk/labs/h265-stego
```

Cần thấy:

```text
config/
dockerfiles/
docs/
h265-stego/
instr_config/
HUONG_DAN_SINH_VIEN.md
```

## 2. Chạy lab

```bash
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

Nếu muốn làm lại từ đầu:

```bash
labtainer -r h265-stego
```

## 3. Nếu máy không vào được DockerHub

Nếu gặp lỗi:

```text
Unable to reach DockerHub
Could not find image info for h265-stego
```

thì build image local:

```bash
cd ~/labtainer/labtainer-student
./bin/rebuild -L -b h265-stego
```

## 4. Kiểm tra kết quả

```bash
checkwork h265-stego
```

Bài lab không yêu cầu nhập checkword thủ công.

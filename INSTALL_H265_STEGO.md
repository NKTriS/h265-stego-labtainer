# Cài đặt bài lab h265-stego từ GitHub

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

## 2. Build Docker image lần đầu

Trên máy mới, nếu chạy ngay:

```bash
labtainer h265-stego
```

mà gặp lỗi:

```text
Unable to reach DockerHub
Could not find image info for h265-stego
```

thì nguyên nhân là máy chưa có Docker image của lab. Hãy build image trước:

```bash
cd ~/labtainer/labtainer-student
./bin/rebuild -L -b h265-stego
```

Sau khi build xong, kiểm tra image:

```bash
docker images | grep h265-stego
```

Cần thấy image dạng:

```text
h265-stego.h265-stego.student
```

## 3. Chạy lab

```bash
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

Nếu muốn làm lại từ đầu:

```bash
labtainer -r h265-stego
```

## 4. Kiểm tra kết quả

```bash
checkwork h265-stego
```

Bài lab không yêu cầu nhập checkword thủ công.

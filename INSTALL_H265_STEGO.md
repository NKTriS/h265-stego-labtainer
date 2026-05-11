# Cài đặt bài lab h265-stego từ GitHub

## Cấu trúc repo đề xuất

Đưa file sau lên GitHub:

```text
h265-stego.tar.gz
```

Ví dụ repo:

```text
https://github.com/<ten-tai-khoan>/<ten-repo>
```

## Cách sinh viên tải bài lab bằng imodule

Sinh viên có thể tải trực tiếp giống các bài DSEC khác:

```bash
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-stego.tar.gz
```

## Cách sinh viên tải bài lab bằng curl

Nếu file được đặt ở nhánh `main`, sinh viên tải bằng lệnh:

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-stego.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
```

Sau đó khởi động bài lab:

```bash
cd ~/labtainer/labtainer-student
labtainer h265-stego
```

Nếu muốn làm lại từ đầu:

```bash
labtainer -r h265-stego
```

## Cách kiểm tra gói sau khi tải

Sau khi giải nén, kiểm tra thư mục:

```bash
ls ~/labtainer/trunk/labs/h265-stego
```

Cần thấy các thư mục/file chính:

```text
config/
dockerfiles/
docs/
h265-stego/
instr_config/
HUONG_DAN_SINH_VIEN.md
```

## Cách kiểm tra bài lab

Khởi động:

```bash
labtainer h265-stego
```

Trong quá trình làm bài, kiểm tra tiến độ:

```bash
checkwork h265-stego
```

Bài lab không yêu cầu nhập checkword thủ công.

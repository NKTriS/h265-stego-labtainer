# Cài lab h265-filler-nal-stego

Lab này điều tra giấu tin trong video H.265 bằng **Filler Data NAL**. File nghi vấn vẫn là video HEVC thật, có thể kiểm tra bằng `ffprobe` và xuất frame bằng `ffmpeg`.

## Xóa bản cũ nếu đã từng chạy

```bash
cd ~/labtainer/labtainer-student
stoplab h265-filler-nal-stego 2>/dev/null || true
docker rm -f h265-filler-nal-stego.workstation.student h265-filler-nal-stego-igrader 2>/dev/null || true
docker rmi -f nktris/h265-filler-nal-stego.workstation.student:latest 2>/dev/null || true
rm -rf ~/labtainer/trunk/labs/h265-filler-nal-stego
rm -rf ~/labtainer/trunk/scripts/labtainer-student/.tmp/h265-filler-nal-stego
rm -rf ~/labtainer_xfer/h265-filler-nal-stego
```

## Cài lab bằng imodule

```bash
cd ~/labtainer/labtainer-student
imodule https://github.com/NKTriS/h265-stego-labtainer/raw/refs/heads/main/h265-filler-nal-stego.tar.gz
labtainer h265-filler-nal-stego
```

Labtainer sẽ dùng image:

```text
nktris/h265-filler-nal-stego.workstation.student:latest
```

Nếu máy không tự kéo image từ DockerHub, chạy thêm:

```bash
docker pull nktris/h265-filler-nal-stego.workstation.student:latest
labtainer h265-filler-nal-stego
```

## Cài lab bằng curl nếu imodule lỗi DNS

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-filler-nal-stego.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
docker pull nktris/h265-filler-nal-stego.workstation.student:latest
cd ~/labtainer/labtainer-student
labtainer h265-filler-nal-stego
```

Nếu máy sinh viên bị lỗi DNS khi tải từ GitHub, có thể tải file `.tar.gz` trên máy khác rồi copy thủ công vào `~/labtainer/trunk/labs/` và giải nén.

## Kiểm tra

```bash
checkwork h265-filler-nal-stego
```

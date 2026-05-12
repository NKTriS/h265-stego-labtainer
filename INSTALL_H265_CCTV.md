# Cài đặt bài lab h265-cctv-motion-leak

Docker images trên DockerHub:

```text
nktris/h265-cctv-motion-leak.analyst.student:latest
nktris/h265-cctv-motion-leak.evidence-server.student:latest
```

Bản hiện tại dùng port `8080` trên `evidence-server`.

## Xóa bản cũ

```bash
cd ~/labtainer/labtainer-student
stoplab h265-cctv-motion-leak 2>/dev/null || true

docker rm -f h265-cctv-motion-leak.analyst.student \
h265-cctv-motion-leak.evidence-server.student \
h265-cctv-motion-leak-igrader 2>/dev/null || true

docker rmi -f nktris/h265-cctv-motion-leak.analyst.student:latest \
nktris/h265-cctv-motion-leak.evidence-server.student:latest 2>/dev/null || true

rm -rf ~/labtainer/trunk/labs/h265-cctv-motion-leak
rm -rf ~/labtainer/trunk/scripts/labtainer-student/.tmp/h265-cctv-motion-leak
rm -rf ~/labtainer_xfer/h265-cctv-motion-leak
```

## Tải lại lab

Nếu mạng GitHub ổn:

```bash
curl -L https://raw.githubusercontent.com/NKTriS/h265-stego-labtainer/main/h265-cctv-motion-leak.tar.gz | tar -xz -C ~/labtainer/trunk/labs/
```

Nếu GitHub bị lỗi DNS, tải file `h265-cctv-motion-leak.tar.gz` bằng máy khác rồi copy vào VM, sau đó chạy:

```bash
tar -xzf h265-cctv-motion-leak.tar.gz -C ~/labtainer/trunk/labs/
```

## Pull image mới

```bash
docker pull nktris/h265-cctv-motion-leak.analyst.student:latest
docker pull nktris/h265-cctv-motion-leak.evidence-server.student:latest
```

## Chạy lab

```bash
cd ~/labtainer/labtainer-student
labtainer h265-cctv-motion-leak
```

Trong terminal `analyst`, tải evidence bằng:

```bash
curl -v http://evidence-server:8080/case/cctv.hevc -o cctv.hevc > curl.stdout 2>&1
```

## Checkwork

```bash
checkwork h265-cctv-motion-leak
```

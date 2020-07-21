# General guidelines and recommendations

## Build from a remote build context, using a Dockerfile from stdin

ใช้ syntax เพื่อสร้าง image โดยใช้ไฟล์จากรีโมต `git` repository,
ใช้ `Dockerfile` จาก `stdin`. ใช้ syntax `-f` (หรือ `--file`) เพื่อเลือก `Dockerfile` ที่ใช้ โดยการใช้สัญลักษณ์ hyphen (`-`) เป็นตัวแทนของ filename เพื่อให้ Docker อ่าร `Dockerfile` จาก `stdin`:

```bash
docker build [OPTIONS] -f- PATH
```

ใช้ Syntax เมื่อต้องการสร้าง image จาก repository ที่ไม่ได้จัดเก็บ `Dockerfile`, หรือต้องการสร้าง `Dockerfile` โดยไม่ขึ้นอยู่กับ fork ของ repository.

ตัวอย่างข้างล่างการสร้าง `Dockerfile` จาก `stdin` และการเพิ่ม `hello.c` ไฟล์จากหห ["hello-world" Git repository on GitHub](https://github.com/docker-library/hello-world).

```bash
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c .
EOF
```

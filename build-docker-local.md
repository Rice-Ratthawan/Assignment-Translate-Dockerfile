# General guidelines and recommendations

## BUILD FROM A LOCAL BUILD CONTEXT, USING A DOCKERFILE FROM STDIN

ใช้ syntax เพื่อสร้าง image โดยใช้ไฟล์จากรีโมต `git` repository,
ใช้ `Dockerfile` จาก `stdin`. ใช้ syntax `-f` (หรือ `--file`) เพื่อระบุ `Dockerfile` ที่ใช้ โดยการใช้สัญลักษณ์ hyphen (`-`) เป็นตัวแทนของ filename เพื่อให้ Docker อ่าร `Dockerfile` จาก `stdin`:

```bash
docker build [OPTIONS] -f- PATH
```

ตัวอย่างข้างล่างใช้ current directory (`.`) ในการ build context และสร้าง image โดยการใช้ `Dockerfile` ที่ผ่าน `stdin` ตามเอกสารอ้างอิง [here
document](http://tldp.org/LDP/abs/html/here-docs.html).

```bash
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context, and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```

# General guidelines and recommendations

## BUILD AN IMAGE USING A DOCKERFILE FROM STDIN, WITHOUT SENDING BUILD CONTEXT

ใช้ syntax นี้เพื่อสร้าง image โดยใช้ `Dockerfile` จาก `stdin`โดยไม่มีการส่งไฟล์เพิ่มเติมเหมือน build context สัญลักษณ์ hyphen (`-`) คัดลอกตำแหน่งของ `PATH`และช่วย Docker อ่าน build context (ที่มีเฉพาะ `Dockerfile`) จาก `stdin` แทนที่จาก directory

```bash
docker build [OPTIONS] -
```

ทำตามตัวอย่างการสร้าง image โดยใช้ `Dockerfile` ผ่าน `stdin`ไม่มีไฟล์ถูกส่งเหมือนกับ build context ไปยัง daemon

```bash
docker build -t myimage:latest -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

การไม่สนใจ build context จะเป็นประโยชน์ในสถานการณ์ที่`Dockerfile` ไม่ร้องขอไฟล์เพื่อที่คัดลอกไปยัง image และปรับปรุงความเร็วในการสร้าง ราวกับไม่มีไฟล์ส่งไปยัง daemon

ถ้าต้องการเพิ่ม build-speed โดยการ excluding *บาง*ไฟล์ จาก build-
context อ้างถึงใน [exclude with .dockerignore](#exclude-with-dockerignore).

> **Note**: การสร้าง Dockerfile โดยใช้ `COPY` หรือ `ADD` จะล้มเหลว ถ้า syntax ที่ถูกใช้เป็นไปตามตัวอย่างที่แสดงข้างล่าง:
>
> ```bash
> # create a directory to work in
> mkdir example
> cd example
>
> # create an example file
> touch somefile.txt
>
> docker build -t myimage:latest -<<EOF
> FROM busybox
> COPY somefile.txt .
> RUN cat /somefile.txt
> EOF
>
> # observe that the build fails
> ...
> Step 2/3 : COPY somefile.txt .
> COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
> ```

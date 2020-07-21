# Best practices for writing Dockerfiles

เอกสารฉบับนี้ครอบคลุมถึงคำแนะนำเรื่องวิธีปฏิบัติที่ดีสุดและวิธีการสร้าง image ที่มีประสิทธิภาพ

Docker สร้าง images อัตโนมัติโดยการอ่านจาก `Dockerfile` -- text file ที่จัดเก็บคำสั่งทุกอย่าง เพื่อสร้าง image โดย `Dockerfile` สามารถอ่านคำแนะนำได้ที่ [Dockerfile reference](../../engine/reference/builder.md).

Docker image ประกอบด้วย read-only layers ซึ่งแสดงถึง Dockerfile instruction ที่ layer ถูกซ้อนกันและมีชิ้นส่วนของการเปลี่ยนแปลงของ layer ครั้งก่อน โดยดูจาก `Dockerfile`:

```dockerfile
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

แต่ละคำสั่งสร้าง 1 layer:

- `FROM` สร้าง layer จาก `ubuntu:18.04` Docker image.
- `COPY` เพิ่มไฟล์จาก Docker client's current directory.
- `RUN`สร้าง application ด้วย `make`.
- `CMD` ระบุคำสั่งที่เรียกภายใน container.

เมื่อเรียก image และสร้าง container เพิ่ม _writable layer_ ใหม่
(the "container layer") บนสุดของ underlying layers ทุกการเปลี่ยนแปลงสร้าง
การเรียก container เช่น การเขียนไฟล์ใหม่ การปรับแต่งไฟล์ และลบไฟล์ ที่ถูกเขียนใน writable container layer.

ข้อมูลเพิ่มเติมสำหรับ image layers (และวิธีการที่ docker สร้างและจัดเก็บ images) ดูที่
[About storage drivers](../../storage/storagedriver/index.md).

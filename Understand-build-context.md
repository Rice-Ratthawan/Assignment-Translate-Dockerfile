# General guidelines and recommendations
## Understand build context
 เมื่อประกาศคำสั่ง `docker build`เรียกการทำงานนี้ว่า _build context_ โดยปกติ Dockerfiles ถูกจัดเก็บไว้ที่นี้ แต่สามารถย้ายไปยังที่อื่นโดยใช้ file flag (`-f`) เมื่อไม่คำนึงถึงสถานที่จัดเก็บ `Dockerfile` recursive contents และ directories จะถูกส่งไปยัง Docker daemon เหมือนกับ build context

> ตัวอย่างของ Build context
>
> สร้าง directory เพื่อสร้าง build context และใช้ `cd` เพื่อเข้าไปยังไฟล์  เขียนคำว่า "hello" ลงใน text file ที่ชื่อว่า `hello` และสร้าง Dockerfile ที่รัน `cat` จากนั้นสร้าง image จากภายใน build context (`.`):
>
> ```shell
> mkdir myproject && cd myproject
> echo "hello" > hello
> echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
> docker build -t helloapp:v1 .
> ```
> ย้ายไฟล์ `Dockerfile` และ `hello` โดยแยก directories และสร้างเวอร์ชั่นที่สองของ image (โดยไม่ขึ้นอยู่กับ cache จากการสร้างครั้งล่าสุด) ใช้ `-f`เพื่อแสดงให้เห็น Dockerfile และ กำหนด directory ของ build context:
>
> ```shell
> mkdir -p dockerfiles context
> mv Dockerfile dockerfiles && mv hello context
> docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
> ```

การไม่ใส่ใจในไฟล์ที่ไม่สำคัญที่ใช้สำหรับการสร้าง image ส่งผลให้ build context และขนาดของ image มีขนาดใหญ่กว่าเดิม ซึ่งทำให้ใช้เวลาในการสร้าง image pull และ push มาก และขนาดของ container runtime ที่มากขึ้น เพื่อดูขนาดของ build context ที่สร้าง ให้ดูข้อความดังกล่าวตอนที่กำลังสร้าง `Dockerfile`:

```none
Sending build context to Docker daemon  187.8MB
```
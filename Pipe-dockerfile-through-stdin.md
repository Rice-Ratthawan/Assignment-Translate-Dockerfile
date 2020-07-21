# General guidelines and recommendations

## Pipe Dockerfile through `stdin`

Docker มีความสามารถในการสร้าง images โดยทำการ piping `Dockerfile` ผ่าน `stdin`
ด้วย a _local หรือ remote build context_. การ Piping `Dockerfile` ผ่าน `stdin`
สามารถเป็นประโยชน์เพื่อสร้างเพียงครั้งเดียวโดยไม่จำเป็นต้องเขียน Dockerfile ลง Disk หรือบางสถานการณ์เมื่อ `Dockerfile` ถูกสร้าง และควรไม่คงอยู่ในภายหลัง

> ตัวอย่างในส่วนนี้ใช้ [เอกสารฉบับนี้](http://tldp.org/LDP/abs/html/here-docs.html)
> แต่วิธีอื่น ๆ ที่ให้ `Dockerfile` บน `stdin` สามารถใช้ได้เช่นกัน
>
> ใช้คำสั่งตามตัวอย่างข้างล่าง:
>
> ```bash
> echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
> ```
>
> ```bash
> docker build -<<EOF
> FROM busybox
> RUN echo "hello world"
> EOF
> ```
>
> สามารถใช้ตัวอย่างดังกล่าวคู่กับวิธีการที่เราต้องการ หรือวิธีการที่เหมาะสมกับ use-cases

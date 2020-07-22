## ENV
 คำสั่งนี้ คือไว้สำหรับ กำหนดหรืออัปเดต `PATH Environment variable` 

 - ตัวอย่าง: `ENV PATH /usr/local/nginx/bin:$PATH`

คำสั่ง `ENV` ยังสามารถกำหนด Environment variable สำหรับ service ที่คุณต้องการ containerize ตัวอย่างเช่น Postgres’s `PGDATA`

และคำสั่ง ENV ยังสามารถใช้เพื่อsetเลขเวอร์ชันที่ใช้กันทั่วไปเพื่อให้การกำหนดเวอร์ชันนั้นง่ายต่อmaintenance
```
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```
##
ในแต่ละบรรทัด ของคำสั่ง `ENV` จะสร้างเลเยอร์ใหม่ขึ้นมา เหมือนคำสั่ง `RUN` ถึงแม้ว่าจะ unset environment variable ในเลเยอร์ในอนาคต แต่ก็ยังคงอยู่ในเลเยอร์นี้และไม่สามารถลบในเลเยอร์นั้นได้ โดยสามารถทดสอบสิ่งนี้ได้โดยสร้าง Dockerfile และ build เพื่อทำการทดสอบ
```
FROM alpine
ENV ADMIN_USER="mark"
RUN echo $ADMIN_USER > ./mark
RUN unset ADMIN_USER
```
```
$ docker run --rm test sh -c 'echo $ADMIN_USER'

mark
```
และเพื่อป้องกัน ควรจะ unset envrionment variable โดยใช้ คำสั่ง `Run` ด้วย `shell commands` เพื่อ  `set, use, unset` ในvariableทั้งหมด ใน single layer คุณสามารถแบ่งหรือแยกคำสั่งด้วย ` ; `หรือ `&&` ถ้าคุณใช้ทั้งสองวิธี และ 1 ในคำสั่งนั้นเกิด fails การbuild docker ก็จะเกิดการfails ไปด้วย การใช้ `\` เป็นการใช้เพื่อบอกว่า สิ่งนั้นอยู่ในบรรทัดเดียวกัน สำหรับ linux Dockerfiles ซึ่งช่วยทำให้อ่านง่ายขึ้น คุณสามารถใช้คำสั่งทั้งหมดใน shell script ได้ 

```
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh

```

```
$ docker run --rm test sh -c 'echo $ADMIN_USER'
```

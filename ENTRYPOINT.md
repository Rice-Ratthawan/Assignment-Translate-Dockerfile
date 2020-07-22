## ENTRYPOINT
 คำสั่ง `ENTRYPOINT` คือ การ ใช้เพื่อคำสั่งset imageหลัก เพื่อให้ image สามารถ run คำสั่งนั้นได้ (และใช้`CMD`เป็นค่าเริ่มต้น)

เริ่มต้นด้วยตัวอย่าง ของการ image โดยใช้เครื่องมือ commad line `s3cmd`
```
ENTRYPOINT ["s3cmd"]
CMD ["--help"]
```
ตอนนี้ image สามารถรันได้ตามตัวอย่างนี เพื่อแสดง command's help
```
docker run s3cmd
```
หรือใช้พารามิเตอร์ที่เหมาะสมเพื่อดำเนินการคำสั่ง:
```
docker run s3cmd ls s3://mybucket
```
สิ่งนี้มีประโยชน์เพราะ ชื่อของ image สามารถอ้างอิงไปยังไปbinary ที่โชว์ในคำสั่งข้างต้น

คำสั่ง `ENTRYPOINT` ยังสามารถใช้ร่วมกับ ตัวช่วยของ script ซึ่งสามารถทำงานลักษณะเดียวกันได้ตามคำสั่งข้างต้น แม้ว่าการstart อาจต้องใช้เครื่องมือหลายอย่างหลายขั้นตอน

**ตัวอย่าง : Postgres ímage ใช้ script คำสั่ง `ENTRYPOINT`**
```
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```

> **กำหนดค่าแอพ เป็น PID 1**
> script นี้ ใช้คำสั่ง exec ใน bash command ซึ่ง
> เป็นการรันแอพพลิเคชั่นครั้งสุดท้ายให้กลายเป็น container's PID 1
> สิ่งนี้ช่วยอนุญาตให้แอปพลิเคชั่นรับสัญญาณ Unix ส่งให้ container

ตัวช่วย script จะถูกคัดลอก เข้าcontainer และ run โดยเรียกใช้ผ่าน `ENTRYPOINT` ในการ start container :

```
COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres"]
```
script นี้อนุญาตให้ผู้ใช้ใช้งานร่วมกับ Postgres ได้หลายวิธี
Start Postgres : 
```
$ docker run postgres
```
หรือ สามารถเรียกใช้ Postgres และส่งพารามิเตอร์ไปยังเซิร์ฟเวอร์ : 
```
 docker run postgres postgres --help
```
และสุดท้ายยังสามารถ start ด้วยเครื่องมือที่แตกต่างกันไปได้ เช่น Bash
```
docker run --rm -it postgres bash
```


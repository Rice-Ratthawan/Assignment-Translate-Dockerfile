## ADD OR COPY
 คำสั่ง `ADD` และ `COPY` จะมีฟังก์ชั่นการทำงานที่คล้ายกัน แต่คำสั่งที่แนะนำให้ใช้ คือ `COPY` เพราะมีความ transparent มากกว่า คำสั่ง `ADD` 
 ในคำสั่ง `COPY` จะรองรับเฉพาะ การก็อปปี้ local files ใน container
 ส่วนคำสั่ง `ADD` จะมี features หลายอย่าง เช่น (การแยก tar ใน local อย่างเดียว และ การ remote URL ) ดังนั้นการใช้คำสั่ง `ADD` ที่เหมาะสม คือ การดึงไฟล์ tar ใน local แบบการแยกอัตโนมัติ ในimage เช่น `ADD 
 rootfs.tar.xz /.`

ถ้ามีหลาย Dockerfile ที่ใช้งานแตกต่างกัน ใช้`COPY` ทีละไฟล์ แทนที่จะทำทั้งหมดพร้อมกัน วิธีนี้ช่วยให้มั่นใจว่า แต่ละขั้นตอน การสร้าง cache คือจะทำให้ใช้งานไม่ได้ (บังคับให้ต้อง re-run ) หากไฟล์ที่ต้องการโดยเฉพาะเปลี่ยนไป

**ตัวอย่าง**

```
COPY requirements.txt /tmp/
RUN pip install --requirement /tmp/requirements.txt
COPY . /tmp/
```

ผลลัพธ์ มีcacheน้อย ที่ไม่ถูกต้อง สำหรับขั้นตอนการ `RUN` หลังจากนั้นให้ใส่ `COPY` ก่อน /tmp/ 

เพราะ ขนาดของimageมีความสำคัญ การใช้ `ADD` เพื่อดึง packages จาก remote URLs ไม่สนับสนุนอย่างยิ่ง `curl` หรือ `wget` แทน วิธีนี้จะทำให้คุณสามารถลบไฟล์ที่คุณไม่ต้องการได้หลังการได้หลังจากที่แตกไฟล์ออกแล้ว และคุณไม่จำเป็นต้องเพิ่ม layer อื่น ในimage 

**ตัวอย่างที่ควรหลีกเลี่ยง** :  
```
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
**และตัวอย่างที่ควรทำ :**
```
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```
สำหรับรายการอื่นๆ (ไฟล์ , derectories) ที่ไม่ต้องการ `ADD` ไฟล์ tar ความสามารถของการแยกอัตโนมัติ คุณควรจะใช้คำสั่ง`COPY`แทน

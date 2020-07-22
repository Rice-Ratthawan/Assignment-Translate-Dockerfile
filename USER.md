## USER
ถ้าหาก service สามารถ run โดนไม่มีสิทธิ์การเข้าถึงใช้ของ `USER` ที่จะเปลี่ยนไปเป็น non-root user. 
Start ด้วยการสร้าง user และ group ใน Dockerfile อย่างเช่น :
`RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres`

>  **พิจารณาจาก UID/GID ให้ชัดเจน Users และ groups ใน image ได้ assign ให้**
> UID/GID ที่ไม่ได้กำหนดไว้ใน "next" UID/GID ซึ่งถูกassign โดยคำนึงถึง
> การ rebuild ของ image ซึ่งถ้ามันเกิดวิกฤต ควรจะกำหนด UID/GID ให้ชัดเจน

> เนื่องจากถ้าไม่ได้แก้ bug ในการเข้า archive/tar เพื่อจัดการ package
> ของไฟล์ที่กระจัดกระจายเพื่อสร้าง user ที่มี  UID ขนาดใหญ่ภายใน Docker
> container สามารถนำไปสู่การใช้ทรัพยากร disk ที่หนัก เพราะ
> `/var/log/faillog` ใน layer container ซึ่งเต็มไปด้วย NULL อักขระ (\0)
> และวิธีแก้ปัญหาคือส่ง `--no-log-init` ไปยังสถานะ useradd และ
> Debian/Ubuntu ไม่รองรับการตั้งค่าสถานะนี้ `adduser`

หลีกเลี่ยงการติดตั้ง หรือใช้ `sudo` เนื่องจาก `TTY` ไม่สามารถคาดเดาได้ และ พฤติกรรมการส่งสัญญาณ อาจทำให้เกิดปัญหา ถ้าหากคุณต้องการฟังก์ชั่นที่คล้ายกับการใช้ `sudo` เช่น การ initializing ใน daemon แล้วเป็น root แต่ถ้า run แล้วเป็น non-root ให้เปลี่ยนมาใช้ `gosu`
  
  สุดท้ายจะช่วยลด layers และ ความซับซ้อน และให้หลีกเหลี่ยงการสับเปลี่ยน `USER` ไปมาบ่อยๆ

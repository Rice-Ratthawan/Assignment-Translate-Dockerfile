## ONBUILD
 คำสั่ง `ONBUILD` คือการดำเนินการหลังจากการสร้าง Dockerfile สำเร็จ คำสั่ง `ONBUILD` ดำเนินการใน child image ต่างๆ ทีได้รับ `FROM` จาก image ปัจจุบัน เกี่ยวกับ คำสั่ง `ONBUILD` เป็นคำสั่งที่ ให้ parent Dockerfile ส่งข้อมูลต่อให้ child Dockerfile

Docker build เรียกใช้งานคำสั่ง `ONBUILD` ก่อน คำสั่งอื่นๆใน child Dockerfile. 

คำสั่ง `ONBUILD` มีประโยชน์สำหรับ images ที่จะ built  `FROM` ให้กับ image 
ตัวอย่าง : ควรจะใช้ `ONBUILD` สำหรับ  language stack image นั่น การเขียนสร้างซอฟแวร์สำหรับ user ในแต่ละภาษานั้น ภายในDockerfile ซึ่งสามารถเห็นได้จาก ตัวแปร `Ruby’s ONBUILD`

image built ด้วย  `ONBUILD` ควรจะแยกการใช้ tag 
ตัวอย่างเช่น : `ruby:1.9-onbuild ` หรือ `ruby:2.0-onbuild.`

โปรดระวังเมื่อ ใส่ `ADD or COPY` ในคำสั่ง `ONBUILD` และใน image onbuild ล้มเหลวหรือพัง ถ้าหาก การbuild ใหม่ คือ อาจทำให้ทรัพยากรที่เพิ่มเข้ามา อาจหายไปหรือไม่ครบ ซึ่งการเพิ่มแบบการแยกใช้ tag ซึ่งแนะนำตามข้างต้น จะช่วยลดปัญหานี้โดยอนุญาตให้ผู้เขียน Dockerfile ได้ตัดสินใจและเลือกใช้ได้


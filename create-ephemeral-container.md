# General guidelines and recommendations

## Create ephemeral containers

Image ที่ถูกระบุใน `Dockerfile` ควรสร้าง container ที่ใช้เวลาน้อยที่สุด โดยหมายถึง เมื่อมีการหยุดหรือลบ containers จากนั้นสามารถสร้างใหม่หรือแทนที่ด้วย การตั้งค่าที่สมบูรณ์หรือมีการ configulation น้อยที่สุด

อ้างถึง [Processes](https://12factor.net/processes) ภายใต้ _The Twelve-factor App_ methodology ที่ให้ความรู้สึกแรงจูงใจในการใช้ container เหมือนกับสิ่งที่กำลังนิยม

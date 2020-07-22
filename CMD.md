## CMD
คำสั่งCMD ควรใช้ในการrun softwareที่อยู่ในimage วัตถุประสงค์หลักของคำสั่งCMD คือจัดเตรียมdefaultสำหรับการexecute container

คำสั่งcmdมี 3 รูปแบบให้เลือกใช้ดังนี้
- `CMD ["executable","param1","param2"]` : *exec*form เป็นรูปแบบที่เลือกใช้กันเยอะที่สุด
- `CMD ["param1","param2"]` : as default parameters to ENTRYPOINT แนะนำให้ใช้ในกรณี service-based image
- `CMD command param1 param2` : *shell*form เช่น bash python perl

แต่จะต้องเลือกรูปแบบใดรูปแบบหนึ่งมาใช้ในDockerfile ถ้าใช้มากกว่า1รูปแบบ Dockerfileจะเลือกใช้คำสั่งสุดท้ายที่ใช้



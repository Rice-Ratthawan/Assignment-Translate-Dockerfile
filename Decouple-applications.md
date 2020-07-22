## Decouple applications

แต่ละ Container ควรคำนึงถึงการทำ decoupling applications โดยจะแยกเป็นหลายๆ container เพื่อให้ง่ายต่อการปรับขนาดและการนำไปใช้ใหม่ ตัวอย่างเช่น web application stack อาจะประกอบไปด้วย 3 Container ที่แยกจากกันเป็นแต่ละ Container ซึ่งแต่ละ Container นั้นก็อยู่ใน image ของตนเอง เพื่อจัดการ web application , database และcacheในหน่วยความจำ
การจำกัดให้ 1 Container มีเพียง 1 processนั้น ไม่ใช่ กฎระเบียบที่เคร่งครัดอะไร ตัวอย่างเช่น ไม่เพียงแต่ container ที่สามารถสร้าง process บางโปรแกรมมก็สามารถสร้าง process เป็นของตนเองได้ด้วยเช่นกัน Celery สามารถสร้าง process การทำงานได้หลายอย่าง , Apache สามารถสร้าง1 process ต่อ 1 request
ใช้การตัดสินใจของคุณเพื่อให้ container นั้น cleanและใช้งานได้ หาก Container ขึ้นอยู่กับที่อื่น สามารถใช้ Docker container networks ในการติดต่อสื่อสารของ container ได้

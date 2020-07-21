## EXPOSE
เป็นคำสั่งที่แจ้งให้Dockerทราบว่าcontainerอยู่ในport networkไหน สามารถระบุได้ว่าจะอยู่ภายใต้portแบบTCP(default)หรือUDP
```
EXPOSE <port> [<port>/<protocol>...]
//Apache
EXPOSE 80/tcp
EXPOSE 80/udp
//MongoDB
EXPOSE 27017
```
- External Access: ใช้ `-p` บนdocker runเพื่อpublish portขณะทำงานจริง แต่ `-p` เป็นแค่การเอาhost portไว้บนportแบบชั่วคราวเท่านั้น เพราะฉะนั้นportจะไม่เหมือนกับTCPและUDP
`docker run -p 80:80/tcp -p 80:80/udp ...`
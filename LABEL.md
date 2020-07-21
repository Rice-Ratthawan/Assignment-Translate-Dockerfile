## LABEL
labelเป็นกลไกสำหรับปรับmetadataเป็นdocker object ประกอบด้วย
- Images
- Containers
- Local daemons
- Volumes
- Networks
- Swarm nodes
- Swarm services

labelช่วยในการจัดการimage จดจำlicense อธิบายความสัมพันธ์ระหว่างcontainers,volumes,networks

labelเป็น key-value pair เก็บอยู่ในรูปแบบstring สามารถระบุหลายlabelสำหรับobject แต่แต่ละkey-valueจะต้องuniqueในobjectนั้นๆ

สามารถเขียนlabelภายในบรรทัดเดียวได้เพื่อป้องกันการสร้างextra layer

label syntax
`LABEL <key>=<value> <key>=<value> <key>=<value> ...`
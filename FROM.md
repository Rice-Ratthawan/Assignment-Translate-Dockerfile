## FROM
เป็นคำสั่งเริ่มต้นในการสร้างstageและset base imageเพื่อรองรับคำสั่งถัดๆไป.

ในdockerfile.
- จะต้องเริ่มต้นด้วยคำสั่งfrom.
- สามารถมีคำสั่งfromได้หลายอันเพื่อสร้างimageหลายๆอันหรือสร้างstageเฉพาะที่ไม่ใช้ร่วมกับอันอื่นได้ภายในdockerfileอันเดียว.

``
FROM [--platform=<platform>] <image> [AS <name>].

FROM [--platform=<platform>] <image>[:<tag>] [AS <name>].

FROM [--platform=<platform>] <image>[@<digest>] [AS <name>].
``
Tag/digest: ถ้าไม่ใส่builderจะให้มันเป็นlatest tag ถ้าbuilderหาtagไม่เจอจะเกิดerror.
As name: ตั้งชื่อstage.
--platform=<platform>: ใช้บอกว่าimageมาจากplatformไหนไว้ใช้ในกรณีที่คำสั่งFROMเอาimageมาจากหลายๆplatform.

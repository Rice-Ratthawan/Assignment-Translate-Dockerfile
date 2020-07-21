## RUN
การrunมี2รูปแบบ
- `RUN <command>` : shell form โดยdefaultเป็น `/bin/sh -c`บนLinuxหรือ `cmd /S /C` บนWindows
- `RUN ["executable", "param1", "param2"]` : exec form

คำสั่งrunจะexecuteคำสั่งที่อยู่บนlayerบนสุดของimageที่กำลังใช้งานอยู่และcommitผลลัพธ์ออกมา

### APT-GET
- เป็นคำสั่งที่ใช้ทั่วไปในการติดตั้งapp เพราะมีการติดตั้งพวกpackagesมาให้เลย
- หลีกเลี่ยงการใช้ `RUN apt-get upgrade` และ `dist-upgrade` เพราะpackageบางอย่างไม่สามารถอัปเกรดได้ภายในcontainer ดังนั้นเวลาใช่ให้ใช้เป็น
```
RUN apt-get update && apt-get install -y
```
- **cache busting / version pinning** : การระบุversionของpackageเพื่อบังคับให้เรียกใช้cacheได้ การใช้วิธีนี้จะช่วยลดความผิดพลาดการเปลี่ยนแปลงภายในpackage
```
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo=1.3.*
```

- รูปแบบคำสั่งrunที่แนะนำ
```
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    libcap-dev \
    libsqlite3-dev \
    mercurial \
    reprepro \
    ruby1.9.1 \
    ruby1.9.1-dev \
    s3cmd=1.1.* \
 && rm -rf /var/lib/apt/lists/*
```
### USING PIPES
- คำสั่งrunบางคำสั่งต้องพึ่งพาความสามารถในการpipe outputจากคำสั่งหนึ่งไปยังอีกคำสั่งหนึ่ง

- สัญลักษณ์ของpipe คือ |


## Sort multi-line arguments

การจัดเรียง arguments แบบหลายๆบรรทัดควรเรียงลำดับตามตัวอักษรและตัวเลขนั้น จะช่วยฃให้หลีกเลี่ยงการทำซ้ำของ package และสามารถทำให้อัปเดตรายการต่างๆได้ง่ายขึ้น นอกจากนี้ยังช่วยให้อ่านและตรวจสอบ PRs ได้ง่ายขึ้น โดยการเพิ่ม backslash (\)
ตัวอย่างจากการ buildpack-deps image:

```
RUN apt-get update && apt-get install -y \
 bzr \
 cvs \
 git \
 mercurial \
 subversion
```

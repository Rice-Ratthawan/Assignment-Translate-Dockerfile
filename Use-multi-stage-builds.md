## Use multi-stage builds

การทำ Multi-stage builds จะช่วยให้ลดขนาดของ image สุดท้ายอย่างมาก โดยไม่จำเป็นต้องไปจัดการอะไรเลยใน Layerก่อนหน้า เพราะ image จะถูกสร้างขึ้นในขั้นตอนสุดท้ายของการ Build ซึ่งสามารถลด image layer ได้จากการทำ Leverage build cache
ตัวอย่างเช่น ในการ Build ของคุณนั้นมีหลาย layer คุณสามารถเลือก layer จากส่วนที่มีการเปลี่ยนแปลงน้อยครั้งหรือไม่ค่อยได้ใช้งาน (ต้องมั่นใจว่า cacheที่จะนำมาใช้นั้น สามารถใช้ซ้ำกับงานอื่นๆได้ )นำมาใช้งานเพื่อให้มีการใช้งานได้บ่อยขึ้น

- ติดตั้งเครื่องมือที่จำเป้นกับแอปพลิเคชัน
- ติดตั้งหรืออัปเดตไลบรารี่ที่เกี่ยวข้อง
- สร้างแอปพลิเคชัน
  Dockerfile สำหรับสร้างแอปพลิเคชันมีดังนี้ :

```
FROM golang:1.11-alpine AS build

# Install tools required for project
# Run `docker build --no-cache .` to update dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# List project dependencies with Gopkg.toml and Gopkg.lock
# These layers are only re-built when Gopkg files are updated
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# Install library dependencies
RUN dep ensure -vendor-only

# Copy the entire project and build it
# This layer is rebuilt when a file changes in the project directory
COPY . /go/src/project/
RUN go build -o /bin/project

# This results in a single layer image
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```

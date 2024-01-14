
#ถ้าจะไม่บิ้วต้องทำ multi stage image

#stage แรก บิ้ว
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-first-stage
WORKDIR /source
COPY *.csproj .
#คล้ายๆ npm install โหลด ไลบารี่เข้ามา
RUN dotnet restore -a amd64

#บิ้ว โดยใช้คำสั่ง publish (แปลว่าเอาไปทำ web aspnet ได้)
COPY . .
RUN dotnet publish -a amd64 --no-restore -o /app


# เปลี่ยน image เป็นอันที่ 2 คือ run time
FROM mcr.microsoft.com/dotnet/runtime:8.0
WORKDIR /app

# EXPOSE 8080 จะทำเว็บ ก็อย่าลืมเปิด port

COPY --from=build-first-stage /app .
# USER $APP_UID ใช้สำหรับ Kubernetes

# คล้ายๆ CMD แต่ว่า จะบังคับ คำนำหน้cdา ใน string เสมอ
ENTRYPOINT ["./docker-net8"] 
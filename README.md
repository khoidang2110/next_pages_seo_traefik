# 🚀 Hướng Dẫn Chạy Traefik Trên Ubuntu

## 🧰 1. Tạo thư mục và file cấu hình

cd /home/root
mkdir traefik
cd traefik
nano traefik.yml
 
📄 2. Nội dung file traefik.yml
yaml
entryPoints:
  http:
    address: ":80"
  https:
    address: ":443"

api:
  dashboard: true
  insecure: true

certificatesResolvers:
  letsencrypt:
    acme:
      email: khoidang2110@gmail.com  # Thay bằng email của bạn
      storage: acme.json
      httpChallenge:
        entryPoint: http

providers:
  docker:
    network: traefik
    exposedByDefault: false
    
🐳 3. Tạo file docker-compose.yml

version: '3'

services:
  reverse-proxy:
    image: traefik:v2.11
    command: --configFile=/etc/traefik/traefik.yml
    ports:
      - "80:80"
      - "443:443"
      - "8080:8080"  # Dashboard
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik.yml:/etc/traefik/traefik.yml
      - ./acme.json:/acme.json
    networks:
      - traefik
    restart: always

networks:
  traefik:
    external: true
    
🔐 4. Tạo file acme.json và phân quyền

touch acme.json
chmod 600 acme.json

🌐 5. Tạo Docker network traefik

docker network create traefik

🚀 6. Khởi động Traefik

docker-compose up -d


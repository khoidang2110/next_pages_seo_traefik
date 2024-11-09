Hướng dẫn chạy traefik:
1. ở unbutu cd vào home root:

mkdir traefik
 cd traefik
 nano traefik.yml
 
2. copy: 

entryPoints:
  http:
    address: ":80"
  https:
    address: ":443"
api:
  dashboard: true
  insecure: true
certificatesResolvers:
  letsencrypt:  # Đây là tên resolver bạn đang sử dụng trong nhãn
    acme:
      email: nctuan.dev@gmail.com  # Thay bằng email của bạn
      storage: acme.json  # File lưu chứng chỉ SSL
      httpChallenge:
        entryPoint: http  # Xác thực qua HTTP challenge trên cổng 80

providers:
  docker:
    network: traefik
    exposedByDefault: false
3. nano docker-compose.yml
version: '3'
services:
  reverse-proxy:
    image: traefik:v2.11
    command: --configFile=/etc/traefik/traefik.yml
    ports:
      - "80:80"
      - "443:443"
      - "8080:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik.yml:/etc/traefik/traefik.yml
      - ./acme.json:/acme.json
    networks:
      - traefik

networks:
  traefik:
    external: true
4. tao file acme.json
  touch acme.json
 chmod 600 acme.json

5. tao network 
docker network create traefik

docker-compose up -d


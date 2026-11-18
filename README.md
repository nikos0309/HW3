Задание на 4 балла:
Логи запуска:
2026-11-12 16:58:27,602 listening port=8000 storage=sqlite

2026-11-12 16:59:28,146 http 172.17.0.1 "GET / HTTP/1.1" 200 -

2026-11-12 16:59:28,442 http 172.17.0.1 "GET /favicon.ico HTTP/1.1" 404 -

Задание на 6 баллов:

Настройки compose:

services:
  app:
    build: .
    ports:
      - "${INPUT_PORT:-8080}:8000"
    environment:
      GREETING: "${INPUT_GREETING:-Hello, Docker!}"
      STORAGE: sqlite
    volumes:
      - storage:/data

volumes:
  storage:

Команды запуска:

Команда: curl -fsS http://127.0.0.1:8080/

Вывод: {"greeting": "Hello, Docker!", "count": 0, "hostname": "a657665f77aa", "storage": "sqlite"}

Команда: curl -fsS -X POST http://localhost:8080/increment 

Вывод: {"greeting": "Hello, Docker!", "count": 1, "hostname": "50549b1484f1", "storage": "sqlite"}

Команда: docker compose up -d --build --force-recreate

Вывод:  
✔ Image hw_3_template-app       Built
✔ Container hw_3_template-app-1 Started   

Команда: curl -fsS http://127.0.0.1:8080/

Вывод: {"greeting": "Hello, Docker!", "count": 1, "hostname": "a657665f77aa", "storage": "sqlite"}

Команда: curl -fsS http://127.0.0.1:8080/readyz

Вывод: {"status": "ready", "storage": "sqlite"}

Задание на 8 баллов:

Настройки compose: в файле compose.yaml

Команда: cp .env.example .env (создание файла настроек, который потом можно менять)

Команда: docker compose up -d --build

Вывод: 

[+] Building 35.0s (12/12) FINISHED
 => [internal] load local bake definitions                                                                         0.0s
 => => reading from stdin 524B                                                                                     0.0s
 => [internal] load build definition from Dockerfile                                                               0.3s
 => => transferring dockerfile: 227B                                                                               0.0s
 => [internal] load metadata for docker.io/library/python:3.13-slim                                               33.4s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [1/5] FROM docker.io/library/python:3.13-slim@sha256:e48f59cdba69a3cbd9ed2cf0f348d90d45a8d350d98134cf1070bc10  0.1s
 => => resolve docker.io/library/python:3.13-slim@sha256:e48f59cdba69a3cbd9ed2cf0f348d90d45a8d350d98134cf1070bc10  0.1s
 => [internal] load build context                                                                                  0.1s
 => => transferring context: 94B                                                                                   0.0s
 => CACHED [2/5] WORKDIR /app                                                                                      0.0s
 => CACHED [3/5] COPY app/requirements.txt .                                                                       0.0s
 => CACHED [4/5] RUN pip install --no-cache-dir -r requirements.txt                                                0.0s
 => CACHED [5/5] COPY app/app.py .                                                                                 0.0s
 => exporting to image                                                                                             0.2s
 => => exporting layers                                                                                            0.0s
 => => exporting manifest sha256:925989059ef8373c7f21871a827cb3592ec3f278b598f299d3e5b41aeb526774                  0.0s
 => => exporting config sha256:1b333e4f1ab30c7801161f2e8c7a026f46c13e9dbb2846a5e8b07704ca88a1a0                    0.0s
 => => exporting attestation manifest sha256:297edad3dade0409c89c86738d9b6cdac55db52eadeb839c37b8694c358765a9      0.1s
 => => exporting manifest list sha256:9ccac2d2fa56049738ef5a669b705a3dcfd5b79ec953375ccd2c1212a36f1394             0.0s
 => => naming to docker.io/library/hw_3_template-app:latest                                                        0.0s
 => => unpacking to docker.io/library/hw_3_template-app:latest                                                     0.0s
 => resolving provenance for metadata file                                                                         0.0s
[+] up 3/3
 ✔ Image hw_3_template-app         Built                                                                           36.0s
 ✔ Container hw_3_template-redis-1 Started                                                                          1.1s
 ✔ Container hw_3_template-app-1   Started   

Команда (ввёл 4 раза): curl -fsS -X POST http://127.0.0.1:8080/increment

Вывод:

{"greeting": "Hello, Docker!", "count": 4, "hostname": "69aae7471d93", "storage": "redis"}

Команда: docker compose down

Вывод: [+] down 3/3
 ✔ Container hw_3_template-app-1   Removed                                                                          6.1s
 ✔ Container hw_3_template-redis-1 Removed                                                                          0.9s
 ✔ Network hw_3_template_default   Removed  

Команда: docker compose up -d

Вывод: [+] up 3/3
 ✔ Network hw_3_template_default   Created                                                                          0.1s
 ✔ Container hw_3_template-redis-1 Started                                                                          0.8s
 ✔ Container hw_3_template-app-1   Started 

Команда: curl -fsS http://127.0.0.1:8080/

Вывод: {"greeting": "Hello, Docker!", "count": 4, "hostname": "47f6c9c2e07f", "storage": "redis"}

Команда: docker compose stop redis

Вывод: [+] stop 1/1
 ✔ Container hw_3_template-redis-1 Stopped 

Команда: curl -i http://127.0.0.1:8080/healthz

Вывод: HTTP/1.0 200 OK
Server: BaseHTTP/0.6 Python/3.13.18
Date: Wed, 18 Nov 2026 20:14:51 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 19
Cache-Control: no-store

Команда: curl -fsS -X POST http://127.0.0.1:8080/increment

Вывод: curl: (22) The requested URL returned error: 503

Команда: curl -fsS http://127.0.0.1:8080/readyz

Вывод: curl: (22) The requested URL returned error: 503

Команда: docker compose start redis

Вывод: [+] start 1/1
 ✔ Container hw_3_template-redis-1 Started 

Команда: curl -fsS http://127.0.0.1:8080/

Вывод: {"greeting": "Hello, Docker!", "count": 4, "hostname": "47f6c9c2e07f", "storage": "redis"}

Команда: curl -fsS -X POST http://127.0.0.1:8080/increment

Вывод: {"greeting": "Hello, Docker!", "count": 5, "hostname": "47f6c9c2e07f", "storage": "redis"}

1. Создайте папку tg-vpn-proxy и в этой папке:
2. Создайте файл config.json и добавьте туда код:
	
```json
{ "inbounds": [ { "port": 1080, "listen": "0.0.0.0", "protocol": "socks", "settings": { "auth": "noauth", "udp": true } } ], "outbounds": [ { "protocol": "vless", "settings": { "domainStrategy": "UseIPv4", "vnext": [ { "address": "188.214.107.72", "port": 443, "users": [ { "id": "d0f314d0-25d8-4f0f-8e14-f90757d75aa8", "encryption": "none", "flow": "xtls-rprx-vision" } ] } ] }, "streamSettings": { "network": "tcp", "security": "reality", "realitySettings": { "fingerprint": "firefox", "serverName": "google.com", "publicKey": "DKXGl9ug-giztmxMRRX_iKV9j-UGbdnFmWIJjBimyWA", "shortId": "a4", "spiderX": "" } } } ] }
```

3.  Создайте файл **`docker-compose.yml`**
		
	```
	services:
		vless-client:
		    image: teddysun/xray:latest
		    container_name: xray-vless-socks
		    restart: unless-stopped
		    ports:
		      - "1081:1080"
		    volumes:
		      - ./config.json:/etc/xray/config.json
	```

	3. Перейти в папку с проектом:
```
cd C:\...\tg-vpn-proxy
```

4.  Запустите контейнер в фоновом режиме:
	```
	docker compose up -d
	```
5. Проверьте лог работы, чтобы убедиться в отсутствии ошибок:
```
docker compose logs -f
```

6. После успешного запуска прокси начнет работать локально на вашем ПК. Вы можете использовать его в приложениях:

- **Адрес прокси:** `127.0.0.1` (или `localhost`)
    
- **Тип:** `SOCKS5`
    
- **Порт:** `1081
    

**Где это применить:**

- В настройках **Telegram** (Промт / Подключения / Использовать прокси SOCKS5).
    
- В браузерах через расширения для управления прокси (например, **FoxyProxy**), чтобы пускать через него только определенные сайты.
    
- В других десктопных приложениях, поддерживающих подключение через прокси-сервер.
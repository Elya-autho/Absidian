Вот адаптированная инструкция для настройки и запуска этого прокси-сервера на **Linux** (через терминал).

1. Создание папки и файлов

Откройте терминал и выполните команды для создания структуры проекта:

bash

```
mkdir -p ~/tg-vpn-proxy && cd ~/tg-vpn-proxy
```

Используйте код с осторожностью.

Создайте файл конфигурации `config.json`:

bash

```
cat << 'EOF' > config.json
{
  "inbounds": [
    {
      "port": 1080,
      "listen": "0.0.0.0",
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "89.125.24.175",
            "port": 443,
            "users": [
              {
                "id": "43f19854-9311-4064-bbaf-cca7dab4e6d0",
                "encryption": "none",
                "flow": "xtls-rprx-vision"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "fingerprint": "firefox",
          "serverName": "google.com",
          "publicKey": "aBgUKoFnYyscJkcJ2tna_mGFXfAMXv_dBp5CbLIummo",
          "shortId": "7d",
          "spiderX": ""
        }
      }
    }
  ]
}
EOF
```

Используйте код с осторожностью.

Создайте файл конфигурации контейнера `docker-compose.yml`:

bash

```
cat << 'EOF' > docker-compose.yml
services:
  vless-client:
    image: teddysun/xray:latest
    container_name: xray-vless-socks
    restart: unless-stopped
    ports:
      - "1081:1080"
    volumes:
      - ./config.json:/etc/xray/config.json
EOF
```

Используйте код с осторожностью.

2. Запуск и проверка

Запустите контейнер в фоновом режиме:

bash

```
docker compose up -d
```

Используйте код с осторожностью.

_(Если Docker установлен старой версии, команда может быть через дефис: `docker-compose up -d`)_

Проверьте логи работы контейнера:

bash

```
docker compose logs -f
```

Используйте код с осторожностью.

Для выхода из режима просмотра логов нажмите **Ctrl + C**.

3. Использование

Настройки подключения на Linux остаются такими же:

- **Адрес:** `127.0.0.1`
- **Тип:** `SOCKS5`
- **Порт:** `1081`

**Как применить в Linux:**

- **Telegram:** Настройки -> Продвинутые настройки -> Тип подключения -> Использовать собственный прокси -> Добавить прокси SOCKS5.
- **Браузер:** Через расширение **FoxyProxy** или **SmartProxy**.
- **Системный прокси:** В настройках сети вашей графической оболочки (GNOME/KDE) в разделе «Прокси» (однако учитывайте, что не все консольные утилиты подхватывают SOCKS5 автоматически без дополнительных утилит вроде `proxychains`).

Если у вас возникли **ошибки при запуске** docker-compose или вам нужно настроить автоматический запуск прокси при старте системы, сообщите об этом. Какой **дистрибутив Linux** (Ubuntu, Debian, Arch) вы используете?
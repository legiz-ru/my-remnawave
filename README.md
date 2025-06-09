# my-remnawave
## remnawave-subscription-page [сlient configuration](https://remna.st/docs/install/remnawave-subscription-page#configuring-subscription-page-optional)
  - [simple (Clash Verge Rev for PC, Clash Meta for Android, sing-box for iOS)](https://github.com/legiz-ru/my-remnawave/blob/main/sub-page/app-config.json)

<details>
  <summary>multiapp ↓ </summary>
  
[link to multiapp app-config.json](https://github.com/legiz-ru/my-remnawave/blob/main/sub-page/multiapp/app-config.json)

included apps:
  - iOS:
  - - sing-box
  - - Happ
  - - Streisand
  - - ShadowRocket
  - - Clash Mi
  - Android:
  - - Clash Meta for Android
  - - Happ
  - - sing-box
  - - v2rayNG
  - - Exclave
  - PC:
  - - Clash Verge Rev
  - - FlClash
  - - Happ (alpha)

</details>

## remnawave-subscription-page [custom web template](https://remna.st/docs/install/remnawave-subscription-page#mounting-custom-template)
  - [simple happ only](https://github.com/legiz-ru/my-remnawave/blob/main/sub-page/customweb/happ-only/index.html)
  - [marzbanify clash-sing](https://github.com/legiz-ru/my-remnawave/blob/main/sub-page/customweb/clash-sing/index.html)

## remnawave subscription  templates for clients
  - [mihomo](https://github.com/legiz-ru/mihomo-rule-sets/blob/main/examples/remnawave.yaml)
  - [sing-box (1.11 sing-box, 1.10 sing-box legacy)](https://github.com/legiz-ru/sb-rule-sets/tree/main/.github/sub2sing-box)
  - [simple xray-json template](https://github.com/legiz-ru/marz-sub/blob/main/v2ray/default.json)
  - - [xray-json template with ru-bundle](https://github.com/legiz-ru/mihomo-rule-sets/blob/main/other/marzban-v2ray-ru-bundle.json)
  - happ routing:
  - - [simple-ru-routing by frayZV](https://github.com/frayZV/simple-ru-routing/blob/master/README.md#happ-routing) (fullproxy with category-ban-ru without RU)
  - - re-filter:
```shell
happ://routing/onadd/ewogICAgIk5hbWUiOiAiUmU6ZmlsdGVyIiwKICAgICJHbG9iYWxQcm94eSI6ICJmYWxzZSIsCiAgICAiUmVtb3RlRG5zIjogIjEuMS4xLjEiLAogICAgIkRvbWVzdGljRG5zIjogIjc3Ljg4LjguOCIsCiAgICAiR2VvaXB1cmwiOiAiaHR0cHM6Ly9naXRodWIuY29tLzFhbmRyZXZpY2gvUmUtZmlsdGVyLWxpc3RzL3JlbGVhc2VzL2xhdGVzdC9kb3dubG9hZC9nZW9pcC5kYXQiLAogICAgIkdlb3NpdGV1cmwiOiAiaHR0cHM6Ly9naXRodWIuY29tLzFhbmRyZXZpY2gvUmUtZmlsdGVyLWxpc3RzL3JlbGVhc2VzL2xhdGVzdC9kb3dubG9hZC9nZW9zaXRlLmRhdCIsCiAgICAiRG5zSG9zdHMiOiB7fSwKICAgICJEaXJlY3RTaXRlcyI6IFtdLAogICAgIkRpcmVjdElwIjogWwogICAgICAgICIxMC4wLjAuMC84IiwKICAgICAgICAiMTcyLjE2LjAuMC8xMiIsCiAgICAgICAgIjE5Mi4xNjguMC4wLzE2IiwKICAgICAgICAiMTY5LjI1NC4wLjAvMTYiLAogICAgICAgICIyMjQuMC4wLjAvNCIsCiAgICAgICAgIjI1NS4yNTUuMjU1LjI1NSIKICAgIF0sCiAgICAiUHJveHlTaXRlcyI6IFsKICAgICAgICAiZ2Vvc2l0ZTpyZWZpbHRlciIKICAgIF0sCiAgICAiUHJveHlJcCI6IFsKICAgICAgICAiZ2VvaXA6cmVmaWx0ZXIiCiAgICBdLAogICAgIkJsb2NrU2l0ZXMiOiBbXSwKICAgICJCbG9ja0lwIjogW10sCiAgICAiRG9tYWluU3RyYXRlZ3kiOiAiSVBPbkRlbWFuZCIKfQ==
```

## remnawave xhttp inbound tls via nginx + stream separation
<details>
  <summary>↓ XHTTP indound json:</summary>

```json
    {
      "tag": "Sweden_XHTTP",
      "listen": "/dev/shm/xrxh.socket,0666",
      "protocol": "vless",
      "settings": {
        "clients": [],
        "fallbacks": [],
        "decryption": "none"
      },
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls",
          "quic"
        ]
      },
      "streamSettings": {
        "network": "xhttp",
        "xhttpSettings": {
          "mode": "auto",
          "path": "/xhttppath/",
          "extra": {
            "noSSEHeader": true,
            "xPaddingBytes": "100-1000",
            "scMaxBufferedPosts": 30,
            "scMaxEachPostBytes": 1000000,
            "scStreamUpServerSecs": "20-80"
          }
        }
      }
    }
```

</details>

<details>
  <summary>↓ XHTTP nginx reverse proxy:</summary>

```nginx
        location /xhttppath/ {
            client_max_body_size 0;
            grpc_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            client_body_timeout 5m;
            grpc_read_timeout 315;
            grpc_send_timeout 5m;
            grpc_pass unix:/dev/shm/xrxh.socket;
        }
```

</details>

<details>
  <summary>↓ host settings for inbound screenshot</summary>

![image](https://github.com/user-attachments/assets/5cc1c68a-517f-4f35-86ef-c81a668df793)

</details>

<details>
  <summary>↓ host extra xhttp json:</summary>

```json
{
  "xmux": {
    "cMaxReuseTimes": 0,
    "maxConcurrency": "16-32",
    "maxConnections": 0,
    "hKeepAlivePeriod": 0,
    "hMaxRequestTimes": "600-900",
    "hMaxReusableSecs": "1800-3000"
  },
  "noGRPCHeader": false,
  "xPaddingBytes": "100-1000",
  "downloadSettings": {
    "port": 443,
    "address": "another.domain",
    "network": "xhttp",
    "security": "tls",
    "tlsSettings": {
      "alpn": [
        "h2,http/1.1"
      ],
      "show": false,
      "serverName": "another.domain",
      "fingerprint": "chrome",
      "allowInsecure": false
    },
    "xhttpSettings": {
      "path": "/xhttppath/"
    }
  },
  "scMaxEachPostBytes": 1000000,
  "scMinPostsIntervalMs": 30,
  "scStreamUpServerSecs": "20-80"
}
```

</details>

<details>
  <summary>↓ remnanode docker compose:</summary>

```yaml
services:

  remnanode:
    image: remnawave/node:latest
    container_name: remnanode
    hostname: remnanode
    restart: always
    env_file:
      - .env-node
    volumes:
      - /dev/shm:/dev/shm
    network_mode: host
```

</details>

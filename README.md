# آموزش تانل با استفاده از دو X-UI 
سلام خب قراره ما یاد بگیریم چجوری دو تا X-UI رو باهم متصل کنیم یا همون تانل کنیم برای مثال ما دو تا سرور داریم یکی داخل ایران و یکی هم خارج از ایران یا حتی دو تا سرور خارج از ایران و مخوایم اینارو بهم وصل کنیم بریم سر آموزش :)

اول میریم یک کانفیگ رو داخل X-UI سرور خارج از ایران خودمون درست میکنیم حالا از نوعی هیچ تفاوتی نداره. برای مثلا و میبریم پشت کلود فلیر چون محدودیت ها که داخل کلود فلیر داریم رو بعضی از سرور ها ایران نداریم :) بازم حالا شاید داشته باشیم ولی خب میتونین جا اون از AWS استفاده کنید یا هر چی یا بدون هیچ گونه CDN هر کانفیگی که فکر میکنید داخل سرور داخل ایران شما کار میکنه


برای مثال :
Vmess WS TLS 

خب بعد از این که این کارو کردیم این کانفیگ رو داخل Nekoray وارد میکنید. بعد از اون میریم که کانفیگ کامل رو اکسپورت کنیم به چه شکلی؟ به این شکل 
Right click -> share -> export v2ray config
اگر هم مخواید با استفاده از v2rayNG داخل اندروید اینکارو کنید کافیه 
Click share icon ->  export full configuration to clipboard

خب بعد از اون این رو داخل یک VsCode یا هر ابزار دیگه قرار میدیم و میریم قسمت outbounds که برای مثال به این شکله

```bash
"outbounds": [
    {
     "domainStrategy": "AsIs",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "...",
            "port": 443,
            "users": [
              {
                "alterId": 0,
                "id": "...",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "tlsSettings": { "serverName": "..." },
        "wsSettings": {
          "headers": { "Host": "..." },
          "path": "/1c292a50151a/"
        }
      },
      "tag": "proxy"
    },
   ...
],
```

خب ما یا اولینش کار داریم که اینجا میبینم برای مثال حالا این میتونه هر چی باشه Vmess,Vless,Trojan,... 

خب این رو کپی میکنیم و میریم به سرور داخل ایرانمون یا اون سروری که قراره تانل کنه

اول وارد X-UI میشیم و بعد به این قسمت میریم
Setting -> Xray Config

و داخلش اسکرول میکنیم و میایم به قسمت outbounds که بصورت عادی به این شکله

```bash
"outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
``` 

و به این شکل میایم کد قبلی که کپی کرده بودیم رو اضافه میکنیم به این قسمت


```bash
"outbounds": [
     {
     "domainStrategy": "AsIs",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "...",
            "port": 443,
            "users": [
              {
                "alterId": 0,
                "id": "...",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "tlsSettings": { "serverName": "..." },
        "wsSettings": {
          "headers": { "Host": "..." },
          "path": "/1c292a50151a/"
        }
      },
      "tag": "proxy"
    },
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
``` 

و بعد از این  Save میزنیم و Restart رو میزنیم.

خب کار تمومه :) و حالا میتونین یوزر ها خودتون رو داخل سرور وسط یا تانل بسازین.

اگر سرور واسط شما ایران هستش خوبی این رو داره که شما میتونید کانکشن Socks5 هم بسازین به این دلیل که این نوع کانکشن به داخل ایران باز هستش و متونید به عنوان پروکسی تلگرام هم استفاده کنید :)

و بعد از اون میتونید هر نوع کانفیگ رو ایجاد کنید چه با tls چه بدون tls و... به همین راحتی :)

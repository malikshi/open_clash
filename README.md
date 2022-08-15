**Table of Contents**

- [Openclash Config](#openclash-config)
- [Features](#features)
- [Persiapan](#persiapan)
  - [Modem/WAN](#modemwan)
  - [Interface Modem/WAN](#interface-modemwan)
- [Openclash](#openclash)
  - [Download Config](#download-config)
    - [Pemilihan Config](#pemilihan-config)
  - [Setting Multi-WAN OC](#setting-multi-wan-oc)
  - [2 Modem/WAN](#2-modemwan)
  - [1 Modem/WAN](#1-modemwan)
  - [Cara Mengisi Akun](#cara-mengisi-akun)
    - [Edit Files Proxy Provider](#edit-files-proxy-provider)
      - [Shadowsocks](#shadowsocks)
      - [Vmess](#vmess)
      - [Snell](#snell)
      - [Trojan](#trojan)
- [Rule Direct/Bypassed Connection](#rule-directbypassed-connection)
- [Setting Openclash App](#setting-openclash-app)
  - [Global Setting](#global-setting)
    - [Operation Mode](#operation-mode)
    - [DNS Setting](#dns-setting)
    - [GEOIP Update](#geoip-update)
  - [Manage Config](#manage-config)
    - [Import Main.yaml](#import-mainyaml)
    - [Import Proxy Provider](#import-proxy-provider)
    - [Import Rule Provider](#import-rule-provider)
  - [Overviews](#overviews)
- [Setting Yacd](#setting-yacd)
  - [Proxies](#proxies)
    - [Toggle TrafficDirect](#toggle-trafficdirect)
    - [Toggle Traffic Multi-WAN](#toggle-traffic-multi-wan)
    - [Toggle TrafficPortGames](#toggle-trafficportgames)
    - [Toggle TrafficGaming](#toggle-trafficgaming)
    - [Toggle TrafficAds](#toggle-trafficads)
    - [Toggle TrafficP0rn](#toggle-trafficp0rn)
    - [Toggle Traffic GLOBAL](#toggle-traffic-global)
  - [Config](#config)
- [Test Adblock 100%](#test-adblock-100)

# Openclash Config

OpenClash Config untuk VVIP IPTUNNELS
- [Buy VVIP IPTUNNELS](https://linktr.ee/iptunnelscom)
- [Join Telegram](https://t.me/+O08-QK6VNXU5NzU1)
- [Requests Rules](https://github.com/malikshi/open_clash/issues/new/choose)

# Features

* Support Multi-WAN/Modem
* Pisah traffik umum, sosmed, streaming, gaming.
* Support rule Adblock, Privacy & P0rn.
* Support Gaming sites
* Support Game port filtering
* Support web streaming/VOD/TV region ID.
* Support 10 marketplace ID.
* Support E-wallet,Payment,Bank ID
* Support Direct/Bypass traffik.
* Support Rules Online and Offline.

# Persiapan

Openclash config yang disediakan pada repositori ini dikhususkan untuk pengguna VVIP IPTUNNELS dan pengguna openclash di Indonesia.Silahkan untuk membaca baik - baik tutorial yang diberikan, dan jika ada pertanyaan silahkan open issues atau chat digroup telegram [IPTUNNELS](https://t.me/iptunnelscom).

## Modem/WAN

Disini Pertama kalian tentuin berapa modem/router 4G atau WAN sebagai sumber internet yang akan digunakan. Secara Default Config ini menggunakan 3 Modem sebagai berikut:

```conf
Modem OrbitMAX B818-263   : WAN A
Modem E8372h              : WAN B
Modem B315                : WAN C
```

Modem tersebut mempunyai fungsi masing-masing agar memungkinkan mendapatkan performa internet dengan baik.

MODEM | FUNGSI
------------ | -------------
WAN A | INJECT LOAD-BALANCE
WAN B | INJECT LOAD-BALANCE
WAN C | TRAFFIC DIRECT, TRAFFIC GAME DAN BACKUP DARI LOAD-BALANCE


## Interface Modem/WAN

Untuk menentukan interface-name modem/WAN bisa melalui `LuCi > Network >> Interfaces` lihat gambar berikut dengan seksama.

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/interfacename.jpg" border="0">

Perhatikan tanda kotak biru yang merupakan **interface-name** dengan berikut detailsnya,

MODEM | INTERFACE-NAME
------------ | -------------
WAN A | eth1
WAN B | wlan0
WAN C | eth2

# Openclash

Plugin ini adalah klien Clash yang bisa dijalankan di OpenWrt. Kompatibel dengan Shadowsocks ShadowsocksR, Vmess, Trojan, Snell dan protokol lainnya, dan mengimplementasikan proxy kebijakan sesuai dengan konfigurasi aturan yang fleksibel.

## Download Config

Download zip master dan ekstrak file [**open_clash-main.zip**](https://codeload.github.com/malikshi/open_clash/zip/refs/heads/main)

### Pemilihan Config

Disini sudah disediakan main configuration openclash dengan mode online rules dan mode offline rules dengan detail sebagai berikut:

MODE | Filename | Keuntungan | Kelemahan
------------ | ------------- | ------------- | -------------
<span style="color: green"> Online </span> | <span style="color: green"> [main.yaml](main.yaml) </span> | Semua rule dapat update otomatis | Tidak cocok untuk yang tidak memiliki kouta utama & Start Openclash lama karena download rule
<span style="color: red"> Offline </span> | <span style="color: red"> [main_offline.yaml](main_offline.yaml) </span> | Dapat modif manual & start Openclash Cepat | Jika ingin update rule dari kami harus pakai script [updaterule](https://github.com/malikshi/updaterule) yang kami sediakan

Jika sudah menentukan main configuration yang akan digunakan maka gunakan tipe tersebut sampai tutorial selesai, baik edit dan upload [main configuration](#import-mainyaml)

## Setting Multi-WAN OC

Setelah menentukan jumlah modem/WAN yang akan digunakan ketika setting Multi-WAN di Openclash. Kita akan jelaskan jika menggunakan 2 modem/wan dan jika satu modem/wan.

## 2 Modem/WAN

Jika menggunakan 2 modem/WAN kita anggap modem tersebut dengan details berikut:

MODEM | INTERFACE-NAME
------------ | -------------
WAN A | eth1
WAN B | wlan0

kita perlu edit configuration utama `main.yaml` dengan cara menghapus segala yang berkaitan dengan **WAN C** yakni,

```yaml
- name: Direct WAN C
  type: select
  disable-udp: false
  interface-name: eth2
  proxies:
  - DIRECT
```

dan hapus baris yang memiliki kata `Direct WAN C` pada settingan berikut

```yaml
- name: Direct Multi-WAN
  type: select
  disable-udp: false
  proxies:
  - Direct WAN A
  - Direct WAN B
  - Direct WAN C
```

setelah dihapus maka jadi seperti ini,

```yaml
- name: Direct Multi-WAN
  type: select
  disable-udp: false
  proxies:
  - Direct WAN A
  - Direct WAN B
```

## 1 Modem/WAN

Jika kalian hanya menggunakan satu modem hapus hal-hal yang berkaitan dengan `Direct WAN B` dan `Direct WAN C` seperti contoh pengaturan 2 modem.

## Cara Mengisi Akun

Cara mengisi akun supaya load-balance pada tiap proxy-provider berjalan dengan baik. Sangat direkomendasikan tiap proxy-provider diisi dengan 2 akun dengan contoh sebagai berikut:

* untuk file proxy-provider [vvip-id.yaml](https://github.com/malikshi/open_clash/blob/main/proxy_provider/vvip-id.yaml)

NAMA | ISP | INTERFACE-NAME
------------ | ------------- | -------------
VVIP-ID1 | BIZNET | eth1
VVIP-ID2 | UNINET | wlan0

* untuk file proxy-provider [vvip-sg.yaml](https://github.com/malikshi/open_clash/blob/main/proxy_provider/vvip-sg.yaml)

NAMA | ISP | INTERFACE-NAME
------------ | ------------- | -------------
VVIP-SG1 | DO | eth1
VVIP-SG2 | HE | wlan0

* untuk file proxy-provider [vvip-game.yaml](https://github.com/malikshi/open_clash/blob/main/proxy_provider/vvip-game.yaml)

NAMA | ISP | INTERFACE-NAME
------------ | ------------- | -------------
VVIP-GM1 | BIZNET | eth1
VVIP-GM2 | UNINET | wlan0

contoh isi akun untuk **`vvip-id.yaml`** perhatikan juga interface-name!

```yaml
- name: "VVIP-ID1"
  type: trojan
  server: aaa.bbb.ccc.ddd
  port: 443
  password: PASSWORD
  udp: true
  sni: BUGSNI.COM
  alpn:
    - h2
    - http/1.1
  skip-cert-verify: true
  interface-name: eth1

- name: "VVIP-ID2"
  type: vmess
  server: aaa.bbb.ccc.ddd
  port: 443
  uuid: UUID
  alterId: 0
  cipher: auto
  udp: true
  tls: true
  skip-cert-verify: true
  servername: aaa.bbb.ccc.ddd
  network: ws
  ws-opts:
    path: /iptunnelscom
    headers:
      Host: aaa.bbb.ccc.ddd
    max-early-data: 2048
    early-data-header-name: Sec-WebSocket-Protocol
  interface-name: wlan0
```

### Edit Files Proxy Provider

Mengisi akun tunnel pada 3 files pada folder proxy_provider yang terdiri dari vvip-sg, vvip-id, dan vvip-game.
Fungsi dari proxy_provider diatas:
* vvip-sg.yaml, Gunakan akun VVIP IPTUNNELS berlokasi SINGAPORE untuk memperoleh speed/traffic yang lebih bagus.
* vvip-id.yaml, Gunakan akun VVIP IPTUNNELS berlokasi INDONESIA untuk keperluan akses websites/marketplace/live stream apps/Video on Demand yang mengharuskan memakai IP Address Publik Indonesia.
* vvip-game.yaml, Gunakan akun VVIP IPTUNNELS yang memiliki ping atau latency rendah seperti trojan, shadowsocks, gRPC, Snell.

Keterangan lebih lanjut:
Karena config akan disetting dengan sistem fallback dimana jika proxy list pertama gagal maka akan menggunakan proxy list kedua, jika proxy list pertama dan kedua gagal maka akan menggunakan DIRECT, namun jika 30 detik kemudian proxy list pertama kembali aktif maka proxy list urutan pertama akan digunakan, sehingga sangat menguntungkan karena tidak perlu memikirkan jika salah satu proxy gagal.

#### Shadowsocks

* Shadowsocks Original / tanpa plugin
```yaml
- name: "shadowsocks"
  type: ss
  server: aaa.bbb.ccc.ddd
  port: 34963
  cipher: chacha20-ietf-poly1305
  password: passwordss
  udp: true
  interface-name: eth1
```
* Shadowsocks dengan plugin obfs
```yaml
- name: "shadowsocks obfs"
  type: ss
  server: aaa.bbb.ccc.ddd
  port: 32033
  cipher: chacha20-ietf-poly1305
  password: passwordss
  plugin: obfs
  plugin-opts:
    mode: tls
    host: BUG.COM
  interface-name: eth1
```

#### Vmess

* Vmess websocket dengan BUG SNI
```yaml
- name: "Vmess ws bug SNI"
  type: vmess
  server: domainserver.com
  port: 443
  uuid: UUIDMU
  alterId: 0
  cipher: auto
  udp: true
  tls: true
  skip-cert-verify: true
  servername: BUGSNI.COM
  network: ws
  ws-opts:
    path: /iptunnelscom
    headers:
      Host: BUGSNI.COM
    max-early-data: 2048
    early-data-header-name: Sec-WebSocket-Protocol
  interface-name: eth1
```
* Vmess websocket dengan BUG CDN (bolak-balik)
```yaml
- name: "vmess ws bug CDN"
  type: vmess
  server: IP/HOST_CDN_CLOUDFLARE
  port: 443
  uuid: UUIDMU
  alterId: 0
  cipher: auto
  udp: true
  tls: true
  skip-cert-verify: false
  servername: domainservermu.com
  network: ws
  ws-opts:
    path: /iptunnelscom
    headers:
      Host: domainservermu.com
    max-early-data: 2048
    early-data-header-name: Sec-WebSocket-Protocol
  interface-name: eth1
```
* Vmess gRPC bug SNI
```yaml
- name: vmess grpc SNI
  server: domainservermu.com
  port: 443
  type: vmess
  uuid: UUIDMU
  alterId: 0
  cipher: auto
  network: grpc
  tls: true
  servername: BUGSNI.COM
  skip-cert-verify: true
  grpc-opts:
    grpc-service-name: iptunnelsvgrpc
  interface-name: eth1
```
* Vmess gRPC bug CDN
```yaml
- name: vmess grpc CDN
  server: IP/HOST_CDN_CLOUDFLARE
  port: 443
  type: vmess
  uuid: UUIDMU
  alterId: 0
  cipher: auto
  network: grpc
  tls: true
  servername: domainservermu.com
  skip-cert-verify: false
  grpc-opts:
    grpc-service-name: iptunnelsvgrpc
  interface-name: eth1
```

#### Snell

* Snell Server v3 (support udp).
```yaml
- name: "snell server"
  type: snell
  server: aaa.bbb.ccc.ddd
  port: 33223
  psk: password
  version: 3
  udp: true
  obfs-opts:
    mode: tls
    host: BUGSNI.COM
  interface-name: eth1
```

#### Trojan

* Trojan-gfw bug SNI
```yaml
- name: "trojan-gfw SNI"
  type: trojan
  server: domainservermu.com
  port: 443
  password: PASSWORD
  udp: true
  sni: BUGSNI.COM
  alpn:
    - h2
    - http/1.1
  skip-cert-verify: true
  interface-name: eth1
```
* Trojan-go websocket bug CDN
```yaml
- name: trojan ws cdn
  server: IP/HOST_CDN_CLOUDFLARE
  port: 443
  type: trojan
  password: PASSWORD
  network: ws
  sni: domainservermu.com
  skip-cert-verify: false
  udp: true
  ws-opts:
    path: /iptunnelstrgo
    headers:
        Host: domainservermu.com
  interface-name: eth1
```
* Trojan gRPC bug SNI
```yaml
- name: "trojan gRPC SNI"
  type: trojan
  server: domainservermu.com
  port: 443
  password: PASSWORD
  udp: true
  sni: BUGSNI.COM
  alpn:
  - h2
  skip-cert-verify: true
  network: grpc
  grpc-opts:
    grpc-service-name: iptunnelstrojangrpc
  interface-name: eth1
```
* Trojan gRPC bug CDN
```yaml
- name: "trojan gRPC CDN"
  type: trojan
  server: IP/HOST_CDN_CLOUDFLARE
  port: 443
  password: PASSWORD
  udp: true
  sni: domainservermu.com
  alpn:
  - h2
  skip-cert-verify: false
  network: grpc
  grpc-opts:
    grpc-service-name: iptunnelstrojangrpc
  interface-name: eth1
```

# Rule Direct/Bypassed Connection

rule_direct.yaml bersifat offline dimana pengguna dapat mengedit traffic apa saja yang di direct/bypass (tidak menggunakan tunnel). Agar lebih mudah sebagai default sudah disetting bypass traffic Whatsapp dan traffic game Mobile Legends (port tcp & udp ingame) jadi untuk dilobby akan menggunakan traffic dari proxy-groups Gaming.

# Setting Openclash App

Setelah mengedit config main.yaml dan setiap file pada folder proxy_provider serta rule_direct.yaml pada folder rule_provider maka kita akan setting openclash via luCI. Silahkan Login LuCI dan masuk ke Services > Openclash

## Global Setting

Hasil settingan pada global setting akan meng-overide settingal awal pada file main.yaml.

### Operation Mode

* Operation Mode **SWITCH PAGE TO FAKE IP MODE** terlebih dahulu.
* Ceklist/centang opsi sesuai gambar berikut:

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/operation-mode.jpg" border="0">


### DNS Setting

* Ceklist/Centang sesuai gambar:

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dnssetting.jpg" border="0">

* Isi Fallback-filter

Perlu diedit isi fallback-filternya supaya mendapatkan hostname di Yacd Connection log. isi fallback-filter dengan ini.

```yaml
fallback-filter:
  geoip: true
  geoip-code: ID
  ipcidr:
    - 0.0.0.0/8
    - 10.0.0.0/8
    - 100.64.0.0/10
    - 127.0.0.0/8
    - 169.254.0.0/16
    - 172.16.0.0/12
    - 192.0.0.0/24
    - 192.0.2.0/24
    - 192.88.99.0/24
    - 192.168.0.0/16
    - 198.18.0.0/15
    - 198.51.100.0/24
    - 203.0.113.0/24
    - 224.0.0.0/4
    - 240.0.0.0/4
    - 255.255.255.255/32
  domain:
    - "+.google.com"
    - "+.facebook.com"
    - "+.youtube.com"
    - "+.githubusercontent.com"
    - "+.googlevideo.com"
    - "+.msftconnecttest.com"
    - "+.msftncsi.com"
    - msftconnecttest.com
    - msftncsi.com
    - "+.*"
```

* Tambahkan Server Group name server dan fallback masing masing 2 seperti pada gambar berikut.

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-fallback.jpg" border="0">

DNS Server Group | DNS Server Address | DNS Server Port | DNS Server Type
------------ | ------------- | ------------- | -------------
NameServer | dns.quad9.net/dns-query |   | HTTPS
NameServer | 9.9.9.9 | 853 | TLS
FallBack | dns.quad9.net/dns-query |   | HTTPS
FallBack | 149.112.112.112 | 853 | TLS

### GEOIP Update

Pada rule_indo.yaml menggunakan geoip:ID dimana jika IP tersebut bercode/berasal negara Indonesia maka akan menggunakan trafficIndo.yaml dan itu membutuhkan mmdb yang selalu updated sebagai data geoip seluruh negara.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/geoip-update.jpg" border="0">

## Manage Config

Mulai dari versi v0.44.22-beta pada branch dev, menu Manage Config support dengan fungsi create,edit,delete file jadi tidak perlu login ssh untuk edit via terminal router openwrt. Tidak membutuhkan tiny fm (file manager) karena config editor built-in openclash terdapat validator config jika terdapat kesalahan/error saat melakukan pengeditan.

### Import Main.yaml

Setelah melakukan pengeditan main.yaml maka kita import main.yaml via Manage Config. Dan khusus main.yaml jangan import/edit melalui winscp/sftp.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/main-upload.jpg" border="0">

### Import Proxy Provider

Jika Semua file pada folder proxy_provider yang terdiri dari vvip-id.yaml, vvip-sg.yaml, vvip-game.yaml yang sudah diisi dengan akun maka selanjutnya import file-file tersebut pada **Upload File Type : Proxy Provider File**.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/proxy-upload.jpg" border="0">

### Import Rule Provider

traffic direct/bypass sudah disikan ke rule_direct.yaml maka bisa langsung import semua files pada folder rule_provider pada **Upload File Type : Rule Provider File**.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/rule-upload.jpg" border="0">

## Overviews

Pada Overviews kita bisa melihat sepintas status Openclash yang berjalan, status versi openclash terbaru dan menjalankan tools openclash yakni dengan klik `Enable Openclash` untuk start, klik `Disable Openclash` untuk stop.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/overviews.jpg" border="0">

# Setting Yacd

Yacd adalah yet another clash dashboard, yakni dashboard clash yang dapat digunakan untuk mengatur proxy dan memantau services clash yang dijalankan oleh Openclash.

## Proxies

Kita akan mengatur toggle/pilihan proxies yang kan digunakan tiap proxy-groups.

### Toggle TrafficDirect

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/direct.jpg" border="0">

### Toggle Traffic Multi-WAN

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/multiwan.jpg" border="0">

### Toggle TrafficPortGames

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/portgames.jpg" border="0">

### Toggle TrafficGaming

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/gaming.jpg" border="0">

### Toggle TrafficAds

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/ads.jpg" border="0">

### Toggle TrafficP0rn

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/adlt.jpg" border="0">

### Toggle Traffic GLOBAL
Untuk pertama kali start openclash maka harus setting proxies `GLOBAL` ke traffic proxy-groups `Umum`.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/yacd-proxies-global.jpg" border="0">


## Config

Pada menu yacd > config untuk pertama kali menjalankan openclash maka perlu setting Mode ke `Rule` dan `Enable Allow LAN` serta bisa mengaktifkan log level untuk melakukan debugging/tracing traffic jika ada rule yang salah.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/yacd-config.jpg" border="0">

# Test Adblock 100%

Silahkan test rules adblock melalui [https://d3ward.github.io/toolz/adblock.html](https://d3ward.github.io/toolz/adblock.html)

Hasil test:
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/d3ward.jpg" border="0">
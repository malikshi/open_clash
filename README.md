**Table of Contents**

- [Openclash Config](#openclash-config)
- [Features](#features)
- [Persiapan](#persiapan)
  - [Modem/WAN](#modemwan)
  - [Interface Modem/WAN](#interface-modemwan)
- [Openclash](#openclash)
  - [Download Config](#download-config)
  - [Setting Multi-WAN OC](#setting-multi-wan-oc)
  - [2 Modem/WAN](#2-modemwan)
      - [1 Modem/WAN](#1-modemwan)
    - [Edit Files Proxy Provider](#edit-files-proxy-provider)
      - [Shadowsocks](#shadowsocks)
      - [Vmess](#vmess)
      - [Snell](#snell)
      - [Trojan](#trojan)
    - [Edit Files Rule Provider](#edit-files-rule-provider)
      - [Rule Direct/Bypassed Connection](#rule-directbypassed-connection)
    - [Setting Openclash App](#setting-openclash-app)
      - [Global Setting](#global-setting)
        - [Operation Mode](#operation-mode)
        - [DNS Setting](#dns-setting)
        - [GEOIP Update](#geoip-update)
        - [Update Version](#update-version)
      - [Manage Config](#manage-config)
        - [Import Main.yaml](#import-mainyaml)
        - [Import Proxy Provider](#import-proxy-provider)
        - [Import Rule Provider](#import-rule-provider)
      - [Overviews](#overviews)
    - [Setting Yacd](#setting-yacd)
      - [Proxies](#proxies)
      - [Config](#config)

# Openclash Config

OpenClash Config untuk VVIP IPTUNNELS
- [Buy VVIP IPTUNNELS](https://linktr.ee/iptunnelscom)
- [Join Telegram](https://t.me/joinchat/RihiceTtK1QhBMm7)
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

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/interfacemodem.jpg" border="0">

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


## Setting Multi-WAN OC

Setelah menentukan jumlah modem/WAN yang akan digunakan maka kita setting Multi-WAN di Openclash. Kita akan jelaskan jika menggunakan 2 modem/wan dan jika satu modem/wan.

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

#### 1 Modem/WAN

Jika kalian hanya menggunakan satu modem hapus hal-hal yang berkaitan dengan `Direct WAN B` dan `Direct WAN C` seperti contoh pengaturan 2 modem.


* Edit Setiap file pada folder Proxy_Provider

    Sebagai Contoh punya 1 atau lebih akun trojan server Singapore yakni proxy X, proxy Y, dan 1 akun trojan server Indo sebut proxy Z.
    Disini karena akan digunakan untuk Loadbalance MULTI-WAN disarankan setiap interface disetting 1 akun dengan perbandingan 1:1. Sebagaimana pada pembukaan Multi-WAN ISP, dengan 2 WAN untuk LB dan 1 WAN untuk direct/bypass traffic atau gaming. Cara Setting Proxies menggunakan sumber koneksi WAN yang mana, misal proxy X menggunakan WAN A/`eth1` dengan routing-mark `1333` dan proxy Y menggunakan WAN B/`wwan` dengan routing-mark `2333`bisa diliat sebagai berikut:
    ```yaml
    - name: "proxy X"
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
      routing-mark: 1333
    
    - name: "proxy Y"
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
      interface-name: wwan
      routing-mark: 2333
    ```


### Edit Files Proxy Provider


Mengisi akun tunnel pada 3 files pada folder proxy_provider yang terdiri dari vvip-sg, vvip-id, dan vvip-game.
Fungsi dari proxy_provider diatas:
* vvip-sg.yaml, Gunakan akun VVIP IPTUNNELS berlokasi SINGAPORE untuk memperoleh speed/traffic yang lebih bagus.
* vvip-id.yaml, Gunakan akun VVIP IPTUNNELS berlokasi INDONESIA untuk keperluan akses websites/marketplace/live stream apps/Video on Demand yang mengharuskan memakai IP Address Publik Indonesia.
* vvip-game.yaml, Gunakan akun VVIP IPTUNNELS yang memiliki ping atau latency rendah seperti trojan,shadowsocks,grPC,Snell.

Keterangan lebih lanjut:
Karena config akan disetting dengan sistem fallback dimana jika proxy list pertama gagal maka akan menggunakan proxy list kedua, jika proxy list pertama dan kedua gagal maka akan menggunakan DIRECT, namun jika 30 detik kemudian proxy list pertama kembali aktif maka proxy list urutan pertama akan digunakan, sehingga sangat menguntungkan karena tidak perlu memikirkan jika salah satu proxy gagal.

Traffic dengan Fallback:

* TrafficUmum : VVIP SG -> VVIP ID -> Direct Multi-WAN
* TrafficStreaming : VVIP SG -> VVIP ID -> Direct Multi-WAN
* TrafficSosmed : VVIP ID -> VVIP SG -> Direct Multi-WAN
* TrafficIndo : VVIP ID -> VVIP SG -> Direct Multi-WAN

Traffic dengan Select:

* TrafficDirect : Direct Multi-WAN / VVIP ID / VVIP SG
* TrafficGaming : TrafficDirect / VVIP GAME
* TrafficAds : REJECT / VVIP SG / VVIP ID
* TrafficP0rn : REJECT / VVIP SG / VVIP ID


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
```

#### Vmess

* Vmess websocket dengan BUG SNI
```yaml
- name: "Vmess ws bug SNI"
  type: vmess
  server: aaa.bbb.ccc.ddd
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
```
* Vmess websocket dengan BUG CDN (bolak-balik)
```yaml
- name: "vmess ws bug CDN"
  type: vmess
  server: IPCDN/BUGCDN.COM
  port: 443
  uuid: UUIDMU
  alterId: 0
  cipher: auto
  udp: true
  tls: true
  skip-cert-verify: false
  servername: namadomainservermu.com
  network: ws
  ws-opts:
    path: /iptunnelscom
    headers:
      Host: namadomainservermu.com
    max-early-data: 2048
    early-data-header-name: Sec-WebSocket-Protocol
```
* Vmess gRPC bug SNI
```yaml
proxies:
  - name: vmess grpc SNI
    server: aaa.bbb.ccc.ddd
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
      grpc-service-name: grpcpath
```
* Vmess gRPC bug CDN
```yaml
proxies:
  - name: vmess grpc CDN
    server: IPCDN/BUGCDN.COM
    port: 443
    type: vmess
    uuid: UUIDMU
    alterId: 0
    cipher: auto
    network: grpc
    tls: true
    servername: namadomainservermu.com
    skip-cert-verify: false
    grpc-opts:
      grpc-service-name: grpcpath
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
```

#### Trojan

* Trojan-gfw bug SNI
```yaml
- name: "trojan-gfw SNI"
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
```
* Trojan-go websocket bug CDN
```yaml
- name: trojan ws cdn
  server: IPCDN/BUGCDN.COM
  port: 443
  type: trojan
  password: PASSWORD
  network: ws
  sni: namadomainservermu.com
  skip-cert-verify: false
  udp: true
  ws-opts:
    path: /iptunnelstrgo
    headers:
        Host: namadomainservermu.com
```
* Trojan gRPC bug SNI
```yaml
- name: "trojan gRPC SNI"
  type: trojan
  server: aaa.bbb.ccc.ddd
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
```
* Trojan gRPC bug CDN
```yaml
- name: "trojan gRPC CDN"
  type: trojan
  server: IPCDN/BUGCDN.COM
  port: 443
  password: PASSWORD
  udp: true
  sni: namadomainservermu.com
  alpn:
  - h2
  skip-cert-verify: false
  network: grpc
  grpc-opts:
    grpc-service-name: iptunnelstrojangrpc
```


### Edit Files Rule Provider

Config Openclash VVIP IPTUNNELS kalian tidak perlu melakukan update manual dari banyaknya rules pada folder rule_provider. Cukup edit rule_direct.yaml dikarenakan itu customize tiap orang untuk bypass trafficnya. Untuk rule yang lain jika ada tambahan silahkan chat ke grup telegram IPTUNNELS atau open issues pada repository ini.

#### Rule Direct/Bypassed Connection

rule_direct.yaml bersifat offline dimana pengguna dapat mengedit traffic apa saja yang di direct/bypass (tidak menggunakan tunnel). Agar lebih mudah sebagai default sudah disetting bypass traffic Whatsapp dan traffic game Mobile Legends (port tcp & udp ingame) jadi untuk dilobby akan menggunakan traffic dari proxy-groups Gaming.

### Setting Openclash App

Setelah Selesai Setting https dns proxy(DoH). mengedit config main.yaml dan setiap file pada folder proxy_provider serta rule_direct.yaml pada folder rule_provider maka kita akan setting openclash via luCI. Silahkan Login LuCI dan masuk ke Services > Openclash

#### Global Setting

Hasil settingan pada global setting akan meng-overide settingal awal pada file main.yaml.

##### Operation Mode

* Operation Mode **SWITCH PAGE TO FAKE IP MODE** terlebih dahulu.
* Ceklist/centang opsi sesuai gambar berikut:

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/operation-mode.jpg" border="0">


##### DNS Setting

* Ceklist/Centang sesuai gambar:

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-setting.jpg" border="0">

* Tambahkan Server Group name server dan fallback masing masing 2. Kemudian isi daril hasil setting https-dns-proxy pada nameserver dan fallback pertama. Setelah itu isi nameserver dan fallback yang kedua dengan dns legacy/udp google dan cloudflare seperti pada gambar berikut.

<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-setting-2.jpg" border="0">

##### GEOIP Update

Pada rule_indo.yaml menggunakan geoip:ID dimana jika IP tersebut bercode/berasal negara Indonesia maka akan menggunakan trafficIndo.yaml dan itu membutuhkan mmdb yang selalu updated sebagai data geoip seluruh negara.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/geoip-update.jpg" border="0">

##### Update Version

Pada Update Version Openclash ada dua branch/cabang yakni versi master dan developer. Secara umum banyak yang menggunakan versi master karena bisa dibilang versi beta yang stable. Dan satu lagi versi developer dimana branch/cabang ini digunakan untuk fix issues/problem jadi bisa dibilang cabang yang selalu update dan jika sudah cukup stable maka akan diriliskan keversi master. Berikut cara settingnya:
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/version-update.jpg" border="0">

1. Pilih Branch master atau developer.
2. Jika sudah memilih branch maka update dengan klik `Check & Update`.
3. Jika ada Versi Terbaru sesuai Branch yang dipilih akan terdapat status \<New>.

#### Manage Config

Mulai dari versi v0.44.22-beta pada branch dev, menu Manage Config support dengan fungsi create,edit,delete file jadi tidak perlu login ssh untuk edit via terminal router openwrt. Tidak membutuhkan tiny fm (file manager) karena config editor built-in openclash terdapat validator config jika terdapat kesalahan/error saat melakukan pengeditan.

##### Import Main.yaml

Setelah melakukan pengeditan main.yaml maka kita import main.yaml via Manage Config. Dan khusus main.yaml jangan import/edit melalui winscp/sftp.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/main-yaml.jpg" border="0">

##### Import Proxy Provider

Jika Semua file pada folder proxy_provider yang terdiri dari umum.yaml, trafficIndo.yaml, streaming.yaml, sosmed.yaml dan gaming.yaml sudah diisi dengan akun maka selanjutnya import file-file tersebut pada **Upload File Type : Proxy Provider File**.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/proxy-provider.jpg" border="0">

##### Import Rule Provider

traffic direct/bypass sudah disikan ke rule_direct.yaml maka bisa langsung import semua files pada folder rule_provider pada **Upload File Type : Rule Provider File**.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/rule-provider.jpg" border="0">

#### Overviews

Pada Overviews kita bisa melihat sepintas status Openclash yang berjalan, status versi openclash terbaru dan menjalankan tools openclash yakni dengan klik `Enable Openclash` untuk start, klik `Disable Openclash` untuk stop.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/overviews.jpg" border="0">

### Setting Yacd

Yacd adalah yet another clash dashboard, yakni dashboard clash yang dapat digunakan untuk mengatur proxy dan memantau services clash yang dijalankan oleh Openclash.

#### Proxies

Untuk pertama kali start openclash maka harus setting proxies `GLOBAL` ke traffic proxy-groups `Umum`.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/yacd-config-2.jpg" border="0">

#### Config

Pada menu yacd > config untuk pertama kali menjalankan openclash maka perlu setting Mode ke `Rule` dan `Enable Allow LAN` serta bisa mengaktifkan log level untuk melakukan debugging/tracing traffic jika ada rule yang salah.
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/yacd-config.jpg" border="0">
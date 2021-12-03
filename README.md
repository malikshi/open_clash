# OpenClash Config
OpenClash Config untuk VVIP IPTUNNELS
## Langkah Pertama
Sangat disarankan untuk menggunakan DNS over HTTPS(DoH) dari apps https-dns-proxy supaya tidak ada kebocoran dns dari isp WAN atau isp LTE yang dipakai.
- Untuk penggunaanya harap untuk menginstall `https-dns-proxy` & `luci-app-https-dns-proxy` caranya :
```sh
opkg update &&\
opkg install https-dns-proxy luci-app-https-dns-proxy
```
- Setting dns https proxy, saran memakai DoH dari ControlD (Block Malware + Ads) atau dari AdGuard (Standard). Lihat screenshot cara settingnya
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-https-proxy.jpg" border="0">
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-https-proxy-2.jpg" border="0">

- Save & Apply

## Langkah Kedua
Setelah langkah pertama sudah selesai maka lanjut dengan setting openclash. Tutorial ini menggunakan gambar supaya pengguna awam juga paham.
- Download zip master dan ekstrak file zip [**open_clash-main.zip**](https://codeload.github.com/malikshi/open_clash/zip/refs/heads/main)
- Edit Setiap Proxy Provider
    - Umum.yaml : Proxy Group ini akan digunakan di **GLOBAL** saat setting **Yacd**.
    - gaming.yaml : Proxy Group ini digunakan untuk traffic gaming yang sudah diatur *rule_games.yaml*, *rule_gaming.yaml*, *rule_portgames*.
    - sosmed.yaml : Proxy Group ini digunakan untuk traffic Sosial Media, seperti *LINE, Metaverse(fb,ig,wa), twitter, telegram, tiktok*.
    - streaming.yaml : Proxy Group ini digunakan untuk traffic Video Streaming seperti *Youtube, DisneyPlus, hbo, iqiyi, netflix, primevideo, twitch*.
    - trafficIndo.yaml : Proxy Group ini dikhususkan jika server yang digunakan memiliki kualitas traffic lokal yang bagus dan traffic internasionalnya jelek sekali.
- Import **main.yaml** ke Upload File Type **Config File** dan Klik Upload.
- Import setiap file dari folder **proxy_provider** ke Upload File Type **Proxy Provider File** dan Klik Upload.
- Import setiap file dari folder **rule_provider** ke Upload File Type **Rule Provider File** dan Klik Upload.
- Selanjutnya Setting **GLOBAL SETTINGS** Harap untuk perhatikan ScreenShot dibawah ini.
    - Operation Mode **SWITCH PAGE TO FAKE IP MODE** terlebih dahulu.
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/operation-mode.jpg" border="0">
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/operation-mode-2.jpg" border="0">
    - DNS Setting, Tambah add custom dns **UDP 127.0.0.1:5054** dan **HTTPS freedns.controld.com/p2**
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-setting.jpg" border="0">
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-setting-2.jpg" border="0">
    - GEOIP Update
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/geoip-update.jpg" border="0">
- **JANGAN LUPA UNTUK APPLY SETTING**
- Selanjutnya Setting Panel Yacd
    - Yacd Config
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/yacd-config.jpg" border="0">
    - Yacd Proxies
        <img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/yacd-config-2.jpg" border="0">
## Happy Browsing
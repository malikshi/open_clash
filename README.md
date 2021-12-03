# OpenClash Config
OpenClash Config untuk orang-orang indonesia.
## Langkah Pertama
Sangat disarankan untuk menggunakan DNS over HTTPS(DoH) dari apps https-dns-proxy supaya tidak ada kebocoran dns dari isp WAN atau isp LTE yang dipakai.
- Untuk penggunaanya harap untuk menginstall `https-dns-proxy` & `luci-app-https-dns-proxy` caranya :
```sh
opkg update &&\
opkg install https-dns-proxy luci-app-https-dns-proxy
```
- Setting dns https proxy, saran memakai DoH dari ControlD (Block Malware + Ads) atau dari AdGuard (Standard). Lihat screenshot cara settingnya
<img src="https://raw.githubusercontent.com/malikshi/open_clash/main/assets/dns-https-proxy.jpg" border="0">
## Usage

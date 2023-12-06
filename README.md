# Jarkom-Modul-4-B03-2023
Berikut adalah laporan resmi untuk praktikum modul 4 jarkom.

| Nama | NRP |
|---------------------------|------------|
|Wan Sabrina Mayzura | 5025211023 |
|Syarifah Talitha Erfany | 5025211175 |

## Daftar Isi
  - [Topologi PKT CIDR](#topologi-pkt-cidr)
  - [Topologi GNS VLSM](#topologi-gns-vlsm)
  - [Rute](#rute)
- [VLSM](#vlsm)
  - [Tree](#tree)
  - [Pembagian IP](#pembagian-ip)
  - [Config GNS3](#config-gns3)
  - [Routing](#routing)
  - [Testing](#testing)
- [CIDR](#cidr)
  - [Penggabungan IP](#penggabungan-ip)
  - [Tree](#tree-1)
  - [Pembagian IP](#pembagian-ip-1)
  - [Routing](#routing-1)
  - [Testing](#testing-1)

## Topologi GNS VLSM 
![gambar](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/raw/main/img/VLSM/topologi%20GNS3.png)

## Topologi Cisco Packet Tracer CIDR
![](img/CIDR/topologi.png)

## Rute
![gambar](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/raw/main/img/VLSM/Rute.png)

Untuk menghitung jumlah IP yang dibutuhkan pada tiap subnet, untuk 1 router akan membutuhkan 1 IP, 1 server membutuhkan 1 IP, masing-masing client menyesuaikan dengan jumlah hostnya, dan untuk switch tidak memerlukan IP. Sebagai contoh, pada A1, terdiri dari 2 client yang masing-masing memiliki 397 dan 625 host, lalu terdapat 1 router, sehingga total host dari kedua client akan ditambah 1.

## VLSM
Berikut ini adalah grouping subnet, terdapat 21 subnet pada topologi ini. Dimana di tiap subnet dapat terdiri dari router dan router, router dan client, router dan server, atau gabungan antara router, server, dan client.

![gambar](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/raw/main/img/VLSM/topologi%20GNS3%20VLSM.png)

### Tree
Berikut merupakan tree perhitungan untuk pembagian IP VLSM, dimana dimulai dari /19 yang merupakan length untuk keseluruhan IP yang dibutuhkan seluruh subnet.

Untuk gambar tree yang lebih jelas lihat [disini](https://miro.com/app/board/uXjVNItkd-k=/).

![gambar](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/raw/main/img/VLSM/Tree%20VLSM%20-%20Tree%20VLSM.jpg)

### Pembagian IP
Berikut adalah hasil pembagian IP dari tree yang sudah dibuat dan direkap pada tabel di bawah:

![gambar](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/raw/main/img/VLSM/pembagian%20IP.png)

### Config GNS3
#### Aura
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 10.10.24.129
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.10.24.149
    netmask 255.255.255.252

auto eth3
iface eth3 inet static
    address 10.10.24.125
    netmask 255.255.255.252
```

Untuk menentukan address masing masing eth suatu node, bisa dilakukan dengan melihat subnetnya dan lihat dari NIDnya. Tetapi tidak bisa menggunakan NID, sehingga gunakan IP selanjutnya. Misal untuk Aura, eth1 merupakan bagian subnet A12, memiliki NID 10.10.24.128, sehingga addressnya menjadi 10.10.24.129 dan netmasknya 255.255.255.252. Untuk eth2, merupakan bagian subnet A18, memiliki NID 10.10.24.148, sehingga addressnya menjadi 10.10.24.149 dan netmasknya 255.255.255.252. Dan untuk eth3, merupakan bagian subnet A10, memiliki NID 10.10.24.124, sehingga addressnya menjadi 10.10.24.125 dan netmasknya 255.255.255.252.

Lalu untuk eth0 Frieren yang juga merupakan bagian dari subnet 10, akan memiliki address 10.10.24.125 (melanjutkan IP dari eth3 Aura). Sedangkan untuk gateway yang hanya digunakan pada eth0, menggunakan address eth yang terhubung dengan eth0  node tersebut. Misal untuk gateway pada eth0 Frieren, gatewaynya adalah 10.10.24.125 yang merupakan address eth3 dari node Aura. Dan begitu seterusnya untuk node yang lain.

#### Frieren
```
auto eth0
iface eth0 inet static
    address 10.10.24.126
    netmask 255.255.255.252
    gateway 10.10.24.125

auto eth1
iface eth1 inet static
    address 10.10.24.121
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.10.24.65
    netmask 255.255.255.224

```

#### LakeKorridor
```
auto eth0
iface eth0 inet static
	address 10.10.24.66
	netmask 255.255.255.224
	gateway 10.10.24.65
```

#### Flamme
```
auto eth0
iface eth0 inet static
	address 10.10.24.122
	netmask 255.255.255.252
        gateway 10.10.24.121

auto eth1
iface eth1 inet static
	address 10.10.24.113
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 10.10.24.117
	netmask 255.255.255.252

auto eth3
iface eth3 inet static
	address 10.10.8.1
	netmask 255.255.255.0
```

#### RohrRoad
```
auto eth0
iface eth0 inet static
	address 10.10.8.2
	netmask 255.255.255.0
	gateway 10.10.8.1
```

#### Fern
```
auto eth0
iface eth0 inet static
    address 10.10.24.114
    netmask 255.255.255.252
    gateway 10.10.24.113

auto eth1
iface eth1 inet static
    address 10.10.0.1
    netmask 255.255.248.0
```

#### AppettRegion
```
auto eth0
iface eth0 inet static
    address 10.10.0.2
    netmask 255.255.248.0
    gateway 10.10.0.1
```

#### LaubHills
```
auto eth0
iface eth0 inet static
    address 10.10.0.3
    netmask 255.255.248.0
    gateway 10.10.0.1
```

#### Himmel
```
auto eth0
iface eth0 inet static
	address 10.10.24.118
	netmask 255.255.255.252
	gateway 10.10.24.117

auto eth1
iface eth1 inet static
	address 10.10.24.97
	netmask 255.255.255.248
```

#### SchwerMountains
```
auto eth0
iface eth0 inet static
    address 10.10.24.98
    netmask 255.255.248.0
    gateway 10.10.24.97
```

#### Denken
```
auto eth0
iface eth0 inet static
	address 10.10.24.150
	netmask 255.255.255.252
        gateway 10.10.24.149

auto eth1
iface eth1 inet static
	address 10.10.22.1
	netmask 255.255.255.0
```

#### RoyalCapital
```
auto eth0
iface eth0 inet static
	address 10.10.22.2
	netmask 255.255.255.0
	gateway 10.10.22.1
```

#### WilleRegion
```
auto eth0
iface eth0 inet static
	address 10.10.22.3
	netmask 255.255.255.0
	gateway 10.10.22.1
```

#### Eisen
```
auto eth0
iface eth0 inet static
    address 10.10.24.130
    netmask 255.255.255.252
    gateway 10.10.24.129

auto eth1
iface eth1 inet static
	address 10.10.24.141
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 10.10.24.105
	netmask 255.255.255.248

auto eth3
iface eth3 inet static
	address 10.10.24.133
	netmask 255.255.255.252

auto eth4
iface eth4 inet static
	address 10.10.24.137
	netmask 255.255.255.252
```

#### Stark
```
auto eth0
iface eth0 inet static
	address 10.10.24.134
	netmask 255.255.255.252
	gateway 10.10.24.133
```

#### Richter
```
auto eth0
iface eth0 inet static
	address 10.10.24.106
	netmask 255.255.255.248
	gateway 10.10.24.105
```

#### Revolte
```
auto eth0
iface eth0 inet static
	address 10.10.24.107
	netmask 255.255.255.248
	gateway 10.10.24.105
```

#### Lugner
```
auto eth0
iface eth0 inet static
	address 10.10.24.138
	netmask 255.255.255.252
        gateway 10.10.24.137

auto eth1
iface eth1 inet static
	address 10.10.16.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.10.23.1
	netmask 255.255.252.0
```

#### TurkRegion
```
auto eth0
iface eth0 inet static
	address 10.10.16.2
	netmask 255.255.255.0
	gateway 10.10.16.1
```

#### GrobeForest
```
auto eth0
iface eth0 inet static
	address 10.10.23.2
	netmask 255.255.252.0
	gateway 10.10.23.1
```

#### Linie
```
auto eth0
iface eth0 inet static
	address 10.10.24.142
	netmask 255.255.255.252
        gateway 10.10.24.141

auto eth1
iface eth1 inet static
	address 10.10.24.145
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 10.10.20.1
	netmask 255.255.254.0
```

#### GranzChannel
```
auto eth0
iface eth0 inet static
	address 10.10.20.2
	netmask 255.255.254.0
	gateway 10.10.20.1
```

#### Lawine
```
auto eth0
iface eth0 inet static
	address 10.10.24.146
	netmask 255.255.255.252
        gateway 10.10.24.145

auto eth1
iface eth1 inet static
	address 10.10.24.1
	netmask 255.255.255.192
```

#### BredtRegion
```
auto eth0
iface eth0 inet static
	address 10.10.24.3
	netmask 255.255.255.192
	gateway 10.10.24.1
```

#### Heiter
```
auto eth0
iface eth0 inet static
	address 10.10.24.2
	netmask 255.255.255.192
	gateway 10.10.24.1

auto eth1
iface eth1 inet static
	address 10.10.12.1
	netmask 255.255.252.0
```

#### Sein
```
auto eth0
iface eth0 inet static
	address 10.10.12.3
	netmask 255.255.255.192
	gateway 10.10.12.1
```

#### RiegelCanyon
```
auto eth0
iface eth0 inet static
	address 10.10.12.2
	netmask 255.255.255.192
	gateway 10.10.12.1
```

### Routing

Pada routing, perintah `route add` digunakan untuk menambahkan rute baru ke tabel routing sistem. Dalam setiap perintah, `-net` menandakan jaringan tujuan (menggunakan NID), `netmask` menentukan pembagian subnet, dan `gw` menunjukkan alamat gateway yang harus digunakan untuk mengarahkan lalu lintas ke jaringan tersebut. Lalu default gateway diatur dengan netmask 0.0.0.0, yang berarti bahwa jika tidak ada rute lain yang cocok, lalu lintas akan diarahkan melalui gateway tersebut. 

#### Aura
```shell
# ke arah frieren
# gateway menggunakan eth0 dari frieren
route add -net 10.10.24.64 netmask 255.255.255.224 gw 10.10.24.126 
route add -net 10.10.24.120 netmask 255.255.255.252 gw 10.10.24.126
route add -net 10.10.8.0 netmask 255.255.255.252 gw 10.10.24.126
route add -net 10.10.0.0 netmask 255.255.248.0 gw 10.10.24.126
route add -net 10.10.24.116 netmask 255.255.255.252 gw 10.10.24.126
route add -net 10.10.24.112 netmask 255.255.255.252 gw 10.10.24.126
route add -net 10.10.24.96 netmask 255.255.255.248 gw 10.10.24.126

# ke arah denken
# gateway menggunakan eth0 dari denken
route add -net 10.10.22.0 netmask 255.255.255.0 gw 10.10.24.150 

# ke arah eisen
# gateway menggunakan eth0 dari eisen
route add -net 10.10.24.140 netmask 255.255.255.252 gw 10.10.24.130 
route add -net 10.10.24.104 netmask 255.255.255.252 gw 10.10.24.130
route add -net 10.10.24.132 netmask 255.255.255.252 gw 10.10.24.130
route add -net 10.10.23.0 netmask 255.255.255.0 gw 10.10.24.130
route add -net 10.10.16.0 netmask 255.255.255.0 gw 10.10.24.130
route add -net 10.10.24.136 netmask 255.255.255.252 gw 10.10.24.130
route add -net 10.10.24.144 netmask 255.255.255.252 gw 10.10.24.130
route add -net 10.10.20.0 netmask 255.255.255.252 gw 10.10.24.130
route add -net 10.10.24.0 netmask 255.255.255.192 gw 10.10.24.130
```

#### Denken
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.10.24.149
```

#### Lugner
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.10.24.137
```

#### Linie
```
route add -net 10.10.24.0 netmask 255.255.255.192 gw 10.10.24.146
route add -net 10.10.12.0 netmask 255.255.252.0 gw 10.10.24.146
```

#### Lawine
```
route add -net 10.10.12.0 netmask 255.255.252.0 gw 10.10.24.2
```

#### Heiter
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.10.24.1
```

#### Himmel
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.10.24.122
```

#### Flamme
```
route add -net 10.10.0.0 netmask 255.255.248.0 gw 10.10.24.114
route add -net 10.10.24.96 netmask 255.255.255.248 gw 10.10.24.118
```

#### Fern
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.10.24.122
```

#### Frieren
```
route add -net 10.10.8.0 netmask 255.255.255.252 gw 10.10.24.122
route add -net 10.10.0.0 netmask 255.255.248.0 gw 10.10.24.122
route add -net 10.10.24.112 netmask 255.255.255.252 gw 10.10.24.122
route add -net 10.10.24.116 netmask 255.255.255.252 gw 10.10.24.122
route add -net 10.10.24.96 netmask 255.255.255.248 gw 10.10.24.122
```

#### Eisen
```
route add -net 10.10.23.0 netmask 255.255.255.0 gw 10.10.24.138
route add -net 10.10.16.0 netmask 255.255.255.0 gw 10.10.24.138

route add -net 10.10.24.144 netmask 255.255.255.252 gw 10.10.24.142
route add -net 10.10.20.0 netmask 255.255.255.252 gw 10.10.24.142
route add -net 10.10.24.0 netmask 255.255.255.192 gw 10.10.24.142
route add -net 10.10.12.0 netmask 255.255.252.0 gw 10.10.24.142
```

### Testing
#### Sein ke Richter (Server ke Server)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing1.png)

#### Granzchannel ke TurkRegion (Client ke Client)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing2.png)

#### RiegelCanyon ke Aura (Client ke Router Utama)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing3.png)

#### Fern ke Linie (Router ke Router)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing4.png)

#### RoyalCapital ke Laubhills (Client ke Client)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing5.png)

#### Heiter ke Denken (Router ke Router)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing6.png)

#### ScwherMountains ke Lugner (Client ke Router)

![Alt text](https://github.com/tlithaee/Jarkom-Modul-4-B03-2023/blob/main/img/VLSM/testing7.png)

## CIDR
![a](image-1.png)
![b](img/CIDR/b.png)
![c](img/CIDR/c.png)
![d](img/CIDR/d.png)
![e](img/CIDR/e.png)
![f](img/CIDR/f.png)
![g](img/CIDR/g.png)
![h](img/CIDR/h.png)
![i](img/CIDR/i.png)
![j](img/CIDR/j.png)

### Tree
Berikut adalah pembagian tree CIDR. IP didapatkan dari perhitungan melalui [Visual Subnet Calculator](https://www.davidc.net/sites/default/subnets/subnets.html)

![](img/CIDR/tree.jpg)

### Pembagian IP
Dari IP yang telah diketahui sebelumnya, dimasukkan kedalam file excel agar lebih teratur.

![](img/CIDR/ip.png)

### Penggabungan
Penggabungan yang sebelumnya telah berhasil akan dicatat di excel serta netmask tiap subnet yang ada.

![](img/CIDR/gabung.png)

![](img/CIDR/gabung(1).png)

### Config Cisco Packet Tracer

- A1
  - Fern (Router)
    ```
    IPv4 Address  : 10.8.24.1
    Subnet Mask   : 255.255.248.0
    ```

  - AppetitRegion (Client)
    ```
    IPv4 Address  : 10.8.24.2
    Subnet Mask   : 255.255.248.0
    ```

  - LaubHills (Client)
    ```
    IPv4 Address  : 10.8.24.3
    Subnet Mask   : 255.255.248.0
    ```

- A2
  - Flamme (Router)
    ```
    IPv4 Address  : 10.8.12.1
    Subnet Mask   : 255.255.252.0
    ```

  - RorhRoad (Client)
    ```
    IPv4 Address  : 10.8.12.2
    Subnet Mask   : 255.255.252.0
    ```

- A3
  - Himmel (Router)
    ```
    IPv4 Address  : 10.8.0.2
    Subnet Mask   : 255.255.255.248
    ```

  - Schwer Mountains (Client)
    ```
    IPv4 Address  : 10.8.0.1
    Subnet Mask   : 255.255.255.248
    ```

- A4
  - Heiter (Router)
    ```
    IPv4 Address  : 10.15.128.1
    Subnet Mask   : 255.255.255.0
    ```

  - RiegelCanyon (Client)
    ```
    IPv4 Address  : 10.15.128.2
    Subnet Mask   : 255.255.255.0
    ```

  - Sein (Server)
    ```
    IPv4 Address  : 10.15.128.3
    Subnet Mask   : 255.255.255.0
    ```

- A5
  - Fern (Router)
    ```
    IPv4 Address  : 10.8.15.253
    Subnet Mask   : 255.255.255.252
    ```

  - Flamme (Router)
    ```
    IPv4 Address  : 10.8.15.254
    Subnet Mask   : 255.255.255.252
    ```

- A6
  - Flamme (Router)
    ```
    IPv4 Address  : 10.8.0.14
    Subnet Mask   : 255.255.255.252
    ```

  - Himmel (Router)
    ```
    IPv4 Address  : 10.8.0.13
    Subnet Mask   : 255.255.255.252
    ```

- A7
  - Flamme (Router)
    ```
    IPv4 Address  : 10.8.63.253
    Subnet Mask   : 255.255.255.252
    ```

  - Frieren (Router)
    ```
    IPv4 Address  : 10.8.63.254
    Subnet Mask   : 255.255.255.252
    ```

- A8
- A9
- A10
- A11
- A12
- A13
- A14
- A15
- A16
- A17
- A18
- A19
- A20
- A21

### Routing
![](img/CIDR/r.1.png)
![](img/CIDR/r.2.png)
![](img/CIDR/r.3.png)
![](img/CIDR/r.4.png)
![](img/CIDR/r.5.png)

### Testing
Router - Router 

![](img/CIDR/router%20-%20router.png)

Router - Client

![](img/CIDR/router%20-%20client.png)

Client - Client

![](img/CIDR/client%20-%20client.png)
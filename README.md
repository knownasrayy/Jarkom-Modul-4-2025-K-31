# Jarkom Modul 4 - VLSM Network Configuration
## Kelompok K31

| Nama                        | NRP        |
| --------------------------- | ---------- |
| Shinta Alya Ramadani        | 5027241016 |
| Rayhan Agnan Kusuma	        | 5027241102 |

### Topologi Jaringan
Base IP: `10.79.0.0`
Method: VLSM (Variable Length Subnet Masking)

---

# Network Configuration Guide - VLSM (Cisco) & CIDR (GNS)

## VLSM (Cisco)

### Topologi
<img width="2167" height="1185" alt="gambar cpt" src="https://github.com/user-attachments/assets/9e371424-c684-4776-9ec4-d9cb21d25c14" />

### Tree VLSM
<img width="2000" height="1600" alt="Poppins (50 x 40 cm)" src="https://github.com/user-attachments/assets/9fc286c0-51f6-4b9f-ad7d-15ad5f127b45" />

### VLSM Rute
<img width="1123" height="611" alt="image" src="https://github.com/user-attachments/assets/9f5c989f-e5a9-4b2c-8927-c717823384b4" />

### Pembagian IP VLSM
<img width="1212" height="579" alt="image" src="https://github.com/user-attachments/assets/085b397f-a42e-44c3-a9d5-85ef0afde221" />

### Konfigurasi Router

**1. Router AMONSUL (Hub Utama)**
```
enable
configure terminal
hostname Amonsul

! === Configure Interfaces ===
! Interface to Fornost (A5: 10.79.15.192/30)
interface FastEthernet1/0
ip address 10.79.15.193 255.255.255.252
no shutdown
exit

! Interface to Eregion (A6: 10.79.15.196/30)
interface FastEthernet1/1
ip address 10.79.15.197 255.255.255.252
no shutdown
exit

! Interface to Minastir (A17: 10.79.15.216/30)
interface FastEthernet0/1
ip address 10.79.15.217 255.255.255.252
no shutdown
exit

! === Static Routes ===
! Routes to Fornost's networks
ip route 10.79.15.184 255.255.255.248 10.79.15.194
ip route 10.79.10.0 255.255.254.0 10.79.15.194
ip route 10.79.15.128 255.255.255.224 10.79.15.194
ip route 10.79.15.64 255.255.255.192 10.79.15.194

! Routes to Eregion's networks
ip route 10.79.14.0 255.255.255.128 10.79.15.198
ip route 10.79.15.200 255.255.255.252 10.79.15.198
ip route 10.79.0.0 255.255.252.0 10.79.15.198
ip route 10.79.15.204 255.255.255.252 10.79.15.198
ip route 10.79.14.128 255.255.255.128 10.79.15.198
ip route 10.79.15.160 255.255.255.240 10.79.15.198
ip route 10.79.15.208 255.255.255.252 10.79.15.198
ip route 10.79.15.212 255.255.255.252 10.79.15.198
ip route 10.79.12.0 255.255.254.0 10.79.15.198
ip route 10.79.8.0 255.255.254.0 10.79.15.198

! Routes to Minastir's networks
ip route 10.79.15.220 255.255.255.252 10.79.15.218
ip route 10.79.4.0 255.255.252.0 10.79.15.218
ip route 10.79.15.224 255.255.255.252 10.79.15.218
ip route 10.79.15.176 255.255.255.248 10.79.15.218
ip route 10.79.15.232 255.255.255.248 10.79.15.218
ip route 10.79.15.0 255.255.255.192 10.79.15.218

end
write memory
```

**2. Router FORNOST**
```
enable
configure terminal
hostname Fornost

! === Configure Interfaces ===
! Interface to Amonsul (A5)
interface FastEthernet0/0
ip address 10.79.15.194 255.255.255.252
no shutdown
exit

! Interface to Switch A3 (Valinor, Valmar)
interface FastEthernet0/1
ip address 10.79.15.185 255.255.255.248
no shutdown
exit

! === Static Routes ===
! Default route to Amonsul
ip route 0.0.0.0 0.0.0.0 10.79.15.193

! Routes to Valinor
ip route 10.79.10.0 255.255.254.0 10.79.15.186

! Routes to Valmar
ip route 10.79.15.128 255.255.255.224 10.79.15.186
ip route 10.79.15.64 255.255.255.192 10.79.15.187

end
write memory
```

**3. Router VALINOR**
```
enable
configure terminal
hostname Valinor

! === Configure Interfaces ===
! Interface to Fornost switch (A3)
interface FastEthernet0/0
ip address 10.79.15.186 255.255.255.248
no shutdown
exit

! Interface to Switch A1 (Shadow, Anarion, Lindon)
interface FastEthernet0/1
ip address 10.79.10.1 255.255.254.0
no shutdown
exit

! Interface to Switch A2 (Doriath, Arnor)
interface FastEthernet1/0
ip address 10.79.15.129 255.255.255.224
no shutdown
exit

! === Static Routes ===
! Default route to Fornost
ip route 0.0.0.0 0.0.0.0 10.79.15.185

end
write memory
```

**4. Router VALMAR**
```
enable
configure terminal
hostname Valmar

! === Configure Interfaces ===
! Interface to Fornost switch (A3)
interface FastEthernet0/0
ip address 10.79.15.187 255.255.255.248
no shutdown
exit

! Interface to Switch A2 (Doriath, Arnor)
interface FastEthernet0/1
ip address 10.79.15.130 255.255.255.224
no shutdown
exit

! Interface to Switch A4 (Imrahil, Utumno, Gwaith)
interface FastEthernet1/0
ip address 10.79.15.65 255.255.255.192
no shutdown
exit

! === Static Routes ===
! Default route to Fornost
ip route 0.0.0.0 0.0.0.0 10.79.15.185

end
write memory
```

**5. Router EREGION**
```
enable
configure terminal
hostname Eregion

! === Configure Interfaces ===
! Interface to Amonsul (A6)
interface FastEthernet0/0
ip address 10.79.15.198 255.255.255.252
no shutdown
exit

! Interface to Switch A7 (Mirkwood, Morgul)
interface FastEthernet0/1
ip address 10.79.14.1 255.255.255.128
no shutdown
exit

! Interface to Numenor (A8)
interface FastEthernet1/0
ip address 10.79.15.201 255.255.255.252
no shutdown
exit

! === Static Routes ===
! Routes to Amonsul and its branches
ip route 10.79.15.192 255.255.255.252 10.79.15.197
ip route 10.79.15.216 255.255.255.252 10.79.15.197

! Routes to Fornost branch
ip route 10.79.15.184 255.255.255.248 10.79.15.197
ip route 10.79.10.0 255.255.254.0 10.79.15.197
ip route 10.79.15.128 255.255.255.224 10.79.15.197
ip route 10.79.15.64 255.255.255.192 10.79.15.197

! Routes to Minastir branch
ip route 10.79.15.220 255.255.255.252 10.79.15.197
ip route 10.79.4.0 255.255.252.0 10.79.15.197
ip route 10.79.15.224 255.255.255.252 10.79.15.197
ip route 10.79.15.176 255.255.255.248 10.79.15.197
ip route 10.79.15.232 255.255.255.248 10.79.15.197
ip route 10.79.15.0 255.255.255.192 10.79.15.197

! Routes to Numenor branch
ip route 10.79.0.0 255.255.252.0 10.79.15.202
ip route 10.79.15.204 255.255.255.252 10.79.15.202
ip route 10.79.14.128 255.255.255.128 10.79.15.202
ip route 10.79.15.160 255.255.255.240 10.79.15.202
ip route 10.79.15.208 255.255.255.252 10.79.15.202
ip route 10.79.15.212 255.255.255.252 10.79.15.202
ip route 10.79.12.0 255.255.254.0 10.79.15.202
ip route 10.79.8.0 255.255.254.0 10.79.15.202

end
write memory
```

**6. Router NUMENOR**
```
enable
configure terminal
hostname Numenor

! === Configure Interfaces ===
! Interface to Eregion (A8)
interface FastEthernet0/0
ip address 10.79.15.202 255.255.255.252
no shutdown
exit

! Interface to Mordor (A13)
interface FastEthernet0/1
ip address 10.79.15.209 255.255.255.252
no shutdown
exit

! Interface to Switch A12 (Arthedain, Mirdain)
interface FastEthernet1/0
ip address 10.79.0.1 255.255.252.0
no shutdown
exit

! Interface to Gudur (A9)
interface FastEthernet1/1
ip address 10.79.15.205 255.255.255.252
no shutdown
exit

! === Static Routes ===
! Default route to Eregion
ip route 0.0.0.0 0.0.0.0 10.79.15.201

! Routes to Gudur
ip route 10.79.14.128 255.255.255.128 10.79.15.206
ip route 10.79.15.160 255.255.255.240 10.79.15.206

! Routes to Mordor-Erain
ip route 10.79.15.212 255.255.255.252 10.79.15.210
ip route 10.79.12.0 255.255.254.0 10.79.15.210
ip route 10.79.8.0 255.255.254.0 10.79.15.210

end
write memory
```

**7. Router GUDUR**
```
enable
configure terminal
hostname Gudur

! === Configure Interfaces ===
! Interface to Numenor (A9)
interface FastEthernet0/0
ip address 10.79.15.206 255.255.255.252
no shutdown
exit

! Interface to Switch A10 (Palantir, Endhil)
interface FastEthernet0/1
ip address 10.79.14.129 255.255.255.128
no shutdown
exit

! Interface to Switch A11 (IronCrown, Grond, Hobbiton)
interface FastEthernet1/0
ip address 10.79.15.161 255.255.255.240
no shutdown
exit

! === Static Routes ===
! Default route to Numenor
ip route 0.0.0.0 0.0.0.0 10.79.15.205

end
write memory
```

**8. Router MORDOR**
```
enable
configure terminal
hostname Mordor

! === Configure Interfaces ===
! Interface to Numenor (A13)
interface FastEthernet0/0
ip address 10.79.15.210 255.255.255.252
no shutdown
exit

! Interface to Erain (A14)
interface FastEthernet0/1
ip address 10.79.15.213 255.255.255.252
no shutdown
exit

! === Static Routes ===
! Default route to Numenor
ip route 0.0.0.0 0.0.0.0 10.79.15.209

! Routes to Erain
ip route 10.79.12.0 255.255.254.0 10.79.15.214
ip route 10.79.8.0 255.255.254.0 10.79.15.214

end
write memory
```

**9. Router ERAIN**
```
enable
configure terminal
hostname Erain

! === Configure Interfaces ===
! Interface to Mordor (A14)
interface FastEthernet0/0
ip address 10.79.15.214 255.255.255.252
no shutdown
exit

! Interface to Switch A15 (Melkor, Khazad)
interface FastEthernet0/1
ip address 10.79.12.1 255.255.254.0
no shutdown
exit

! Interface to Switch A16 (Balrog, Gothmog, Thranduil)
interface FastEthernet1/0
ip address 10.79.8.2 255.255.254.0
no shutdown
exit

! === Static Routes ===
! Default route to Mordor
ip route 0.0.0.0 0.0.0.0 10.79.15.213

end
write memory
```

10. Router MINASTIR
```
enable
configure terminal
hostname Minastir

! === Configure Interfaces ===
! Interface to Amonsul (A17)
interface FastEthernet0/0
ip address 10.79.15.218 255.255.255.252
no shutdown
exit

! Interface to Amroth (A20)
interface FastEthernet0/1
ip address 10.79.15.225 255.255.255.252
no shutdown
exit

! Interface to Anor (A18)
interface FastEthernet1/0
ip address 10.79.15.221 255.255.255.252
no shutdown
exit

! === Static Routes ===
! Default route to Amonsul
ip route 0.0.0.0 0.0.0.0 10.79.15.217

! Routes to Anor
ip route 10.79.4.0 255.255.252.0 10.79.15.222

! Routes to Amroth branch
ip route 10.79.15.176 255.255.255.248 10.79.15.226
ip route 10.79.15.232 255.255.255.248 10.79.15.226
ip route 10.79.15.0 255.255.255.192 10.79.15.226

end
write memory
```

**11. Router ANOR**
```
enable
configure terminal
hostname Anor

! === Configure Interfaces ===
! Interface to Minastir (A18)
interface FastEthernet0/0
ip address 10.79.15.222 255.255.255.252
no shutdown
exit

! Interface to Switch A19 (Beacon, Silmaris)
interface FastEthernet0/1
ip address 10.79.4.1 255.255.252.0
no shutdown
exit

! === Static Routes ===
! Default route to Minastir
ip route 0.0.0.0 0.0.0.0 10.79.15.221

end
write memory
```

**12. Router AMROTH**
```
enable
configure terminal
hostname Amroth

! === Configure Interfaces ===
! Interface to Minastir (A20)
interface FastEthernet0/0
ip address 10.79.15.226 255.255.255.252
no shutdown
exit

! Interface to Switch A21 (Throne, Morgoth)
interface FastEthernet0/1
ip address 10.79.15.177 255.255.255.248
no shutdown
exit

! === Static Routes ===
! Default route to Minastir
ip route 0.0.0.0 0.0.0.0 10.79.15.225

! Routes to Throne
ip route 10.79.15.232 255.255.255.248 10.79.15.179

! Routes to Morgoth
ip route 10.79.15.0 255.255.255.192 10.79.15.178

end
write memory
```

**13. Router THRONE**

```
enable
configure terminal
hostname Throne

! === Configure Interfaces ===
! Interface to Amroth switch (A21)
interface FastEthernet0/0
ip address 10.79.15.179 255.255.255.248
no shutdown
exit

! Interface to Switch A22 (Erebor)
interface FastEthernet0/1
ip address 10.79.15.233 255.255.255.248
no shutdown
exit

! === Static Routes ===
! Default route to Amroth
ip route 0.0.0.0 0.0.0.0 10.79.15.177

end
write memory
```

**14. Router MORGOTH**

```
enable
configure terminal
hostname Morgoth

! === Configure Interfaces ===
! Interface to Amroth switch (A21)
interface FastEthernet0/0
ip address 10.79.15.178 255.255.255.248
no shutdown
exit

! Interface to Switch A23 (Erendis, Elrond)
interface FastEthernet0/1
ip address 10.79.15.1 255.255.255.192
no shutdown
exit

! === Static Routes ===
! Default route to Amroth
ip route 0.0.0.0 0.0.0.0 10.79.15.177

end
write memory
```

### Konfigurasi Host
| **Host / Device**       | **Subnet** | **IP Address** | **Netmask**     | **Gateway**  |
| ----------------------- | ---------- | -------------- | --------------- | ------------ |
| Shadow                  | A1         | 10.79.10.2     | 255.255.254.0   | 10.79.10.1   |
| Anarion                 | A1         | 10.79.10.3     | 255.255.254.0   | 10.79.10.1   |
| Lindon                  | A1         | 10.79.10.4     | 255.255.254.0   | 10.79.10.1   |
| Doriath                 | A2         | 10.79.15.131   | 255.255.255.224 | 10.79.15.130 |
| Arnor                   | A2         | 10.79.15.132   | 255.255.255.224 | 10.79.15.130 |
| — (Router Valinor Link) | A3         | 10.79.15.186   | 255.255.255.248 | 10.79.15.185 |
| — (Router Valmar Link)  | A3         | 10.79.15.187   | 255.255.255.248 | 10.79.15.185 |
| Imrahil                 | A4         | 10.79.15.66    | 255.255.255.192 | 10.79.15.65  |
| Utumno                  | A4         | 10.79.15.67    | 255.255.255.192 | 10.79.15.65  |
| Gwaith                  | A4         | 10.79.15.68    | 255.255.255.192 | 10.79.15.65  |
| Mirkwood                | A7         | 10.79.14.2     | 255.255.255.128 | 10.79.14.1   |
| Morgul                  | A7         | 10.79.14.3     | 255.255.255.128 | 10.79.14.1   |
| Palantir                | A10        | 10.79.14.130   | 255.255.255.128 | 10.79.14.129 |
| Endhil                  | A10        | 10.79.14.131   | 255.255.255.128 | 10.79.14.129 |
| IronCrown               | A11        | 10.79.15.162   | 255.255.255.240 | 10.79.15.161 |
| Grond                   | A11        | 10.79.15.163   | 255.255.255.240 | 10.79.15.161 |
| Hobbiton                | A11        | 10.79.15.164   | 255.255.255.240 | 10.79.15.161 |
| Arthedain               | A12        | 10.79.0.2      | 255.255.252.0   | 10.79.0.1    |
| Mirdain                 | A12        | 10.79.0.3      | 255.255.252.0   | 10.79.0.1    |
| Melkor                  | A15        | 10.79.12.2     | 255.255.254.0   | 10.79.12.1   |
| Khazad                  | A15        | 10.79.12.3     | 255.255.254.0   | 10.79.12.1   |
| Balrog                  | A16        | 10.79.8.3      | 255.255.254.0   | 10.79.8.2    |
| Gothmog                 | A16        | 10.79.8.4      | 255.255.254.0   | 10.79.8.2    |
| Thranduil               | A16        | 10.79.8.5      | 255.255.254.0   | 10.79.8.2    |
| Beacon                  | A19        | 10.79.4.2      | 255.255.252.0   | 10.79.4.1    |
| Silmaris                | A19        | 10.79.4.3      | 255.255.252.0   | 10.79.4.1    |
| — (Router Throne Link)  | A21        | 10.79.15.179   | 255.255.255.248 | 10.79.15.177 |
| — (Router Morgoth Link) | A21        | 10.79.15.178   | 255.255.255.248 | 10.79.15.177 |
| Erebor                  | A22        | 10.79.15.234   | 255.255.255.248 | 10.79.15.233 |
| Erendis                 | A23        | 10.79.15.2     | 255.255.255.192 | 10.79.15.1   |
| Elrond                  | A23        | 10.79.15.3     | 255.255.255.192 | 10.79.15.1   |


---

# CIDR (Classless Inter-Domain Routing)

Implementasi **CIDR** pada topologi ini bertujuan untuk melakukan *Supernetting*, yaitu penggabungan subnet-subnet kecil (VLSM) menjadi blok jaringan yang lebih besar (Supernet). Hal ini berguna untuk meringkas tabel routing (*Route Summarization*) agar lebih efisien.

## 1. Pohon CIDR (CIDR Tree)
Visualisasi hierarki penggabungan dari subnet terkecil (*Leaf*) menuju supernet utama (*Root*).

![CIDR Tree Diagram](https://github.com/user-attachments/assets/PLACEHOLDER_LINK_GAMBAR_POHON_CIDR_KAMU)
*(Diagram alur penggabungan subnet A menjadi blok B, C, dan D)*

## 2. Tabel Penggabungan Subnet (Route Summarization)

Tabel ini menunjukkan logika penggabungan subnet berdasarkan kedekatan bit network.

### Tahap 1 (Level B)
Menggabungkan subnet-subnet tetangga menjadi blok `/24` hingga `/21`.

| Subnet Baru | Gabungan Dari (Subnet 1) | Gabungan Dari (Subnet 2) | Netmask Baru | Network ID Hasil |
| :--- | :--- | :--- | :--- | :--- |
| **B1** | A7 (`/25`) | A10 (`/25`) | **/24** | `10.79.14.0/24` |
| **B2** | B1 (`/24`) | Subnet 15.x (`/24`)* | **/23** | `10.79.14.0/23` |
| **B3** | A16 (`/23`) | A1 (`/23`) | **/22** | `10.79.8.0/22` |
| **B4** | A15 (`/23`) | B2 (`/23`) | **/22** | `10.79.12.0/22` |
| **B5** | A12 (`/22`) | A19 (`/22`) | **/21** | `10.79.0.0/21` |

> **Catatan:** *Subnet 15.x* adalah representasi gabungan seluruh subnet kecil (A2, A3, A4, dst) yang berada di range `10.79.15.0` - `10.79.15.255`.

### Tahap 2 (Level C)
Menggabungkan hasil dari Tahap 1 menjadi blok menengah.

| Subnet Baru | Gabungan Dari (Subnet 1) | Gabungan Dari (Subnet 2) | Netmask Baru | Network ID Hasil |
| :--- | :--- | :--- | :--- | :--- |
| **C1** | B3 (`/22`) | B4 (`/22`) | **/21** | `10.79.8.0/21` |

### Tahap 3 (Level D - Supernet Utama)
Penggabungan akhir menjadi satu blok jaringan utama yang mencakup seluruh topologi.

| Subnet Baru | Gabungan Dari (Subnet 1) | Gabungan Dari (Subnet 2) | Netmask Baru | Network ID Hasil |
| :--- | :--- | :--- | :--- | :--- |
| **D1** | B5 (`/21`) | C1 (`/21`) | **/20** | `10.79.0.0/20` |

---

## 3. Tabel Alokasi IP CIDR (Details)

Rincian teknis untuk blok-blok jaringan hasil penggabungan (*Supernet Blocks*).

| Kode Blok | Network ID | Netmask | Broadcast Address | Range IP Valid | Total IP |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **B1** | `10.79.14.0` | `255.255.255.0` | `10.79.14.255` | `10.79.14.1 - 10.79.14.254` | 256 |
| **B2** | `10.79.14.0` | `255.255.254.0` | `10.79.15.255` | `10.79.14.1 - 10.79.15.254` | 512 |
| **B3** | `10.79.8.0` | `255.255.252.0` | `10.79.11.255` | `10.79.8.1 - 10.79.11.254` | 1024 |
| **B4** | `10.79.12.0` | `255.255.252.0` | `10.79.15.255` | `10.79.12.1 - 10.79.15.254` | 1024 |
| **B5** | `10.79.0.0` | `255.255.248.0` | `10.79.7.255` | `10.79.0.1 - 10.79.7.254` | 2048 |
| **C1** | `10.79.8.0` | `255.255.248.0` | `10.79.15.255` | `10.79.8.1 - 10.79.15.254` | 2048 |
| **D1** | **10.79.0.0** | **255.255.240.0** | **10.79.15.255** | **10.79.0.1 - 10.79.15.254** | **4096** |

---

## Kesimpulan Routing

Dengan skema CIDR di atas, Router Eksternal (Internet/ISP) hanya perlu mengetahui satu rute untuk menghubungi seluruh jaringan Kelompok K31:

`ip route 10.79.0.0 255.255.240.0 [Gateway_Amonsul]`

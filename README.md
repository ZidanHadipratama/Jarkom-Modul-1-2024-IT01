# Laporan Resmi Jarkom Modul 1
## Kelompok IT01
| NRP | Nama |
| ------ | ------ |
| 5027221042 |Nicholas Marco Weinandra |
| 5027221052 | Mochamad Zidan Hadipratama |

## Daftar Isi
- [ATM or ATP or FTP?](#ATM-or-ATP-or-FTP?)
- [Evidence](#Evidence)
- [How Many Packets?](#How-Many-Packets?)
- [Trace Him](#Trace-Him)
- [Creds](#Creds)
- [malwleowleo](#malwleowleo)
- [whoami](#whoami)
- [secret](#secret)
- [fuzz](#fuzz)
- [malwaew](#malwaew)

<br/>

# ATM or ATP or FTP?
<details><summary>Soal</summary>
Pradityo mencoba mengembangkan server ftp, tetapi seseorang mencoba melakukan bruteforce login, bisakah Anda menganalisis apa yang terjadi?

author: youdaemon

nc 10.15.40.20 10004

**Pertanyaan**: Apa password yang berhasil didapatkan oleh hacker setelah melakukan bruteforce login ftp?

</details>

## Pengerjaan Soal
**Pertanyaan**: Apa password yang berhasil didapatkan oleh hacker setelah melakukan bruteforce login ftp?

1. Pertama, saya menganalisa packet yang ada di file capture. Kemudian saya mem-follow TCP dari salah satu packet yang berisikan login attempt:<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/ftp1.png">
<br/>Dari capture salah satu packet tersebut, dapat diketahui bahwa response dari server jika attacker salah memasukkan password adalah "Login incorrect."
<br/>
2. Dengan mengetahui response dari server saat client mencoba untuk login adalah Login incorrect, maka untuk mencari password yang didapatkan oleh hacker setelah melakukan bruteforce login http adalah dengan mencoba mencari packet yang berisikan kata kunci "success". Oleh karena itu, saya terapkan display filter ```tcp and frame contains "success"```<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/ftp2.png">
<br/>
3. Kemudian saya melakukan follow stream pada FTP packet yang berisikan response server "Login Successful" dan ditemukan string password yang berhasil didapatkan oleh hacker.<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/ftp3.png">

Maka, Jawaban dari pertanyaan soal ini adalah:  ```m4y_th3_Kn!fe_ch1p_&_sh4tter```

Flag: ```JARKOM2024{Brut3f0rce_FtP_9J8kXbAygRFe8rt}```



# Evidence
<details><summary>Soal</summary>
Perusahaan nanomate baru saja kebobolan. Mereka menyewamu untuk mencari tahu bagaimana caranya pelaku bisa masuk.

Attachment: attachment Author: kiseki

nc 10.15.40.20 10002
</details>

## Pengerjaan Soal

# How Many Packets?
<details><summary>Soal</summary>
Sebagai kewajiban untuk laporan, aku diminta untuk mencari tahu berapa kali attempt login yang dilakukan oleh hacker. Dapatkah kamu membantuku untuk menganalisanya?

attachment: same as ATM or ATP or FTP ? 🤔

author: youdaemon

nc 10.15.40.20 10005
</details>

## Pengerjaan Soal

# Trace Him
<details><summary>Soal</summary>
Selain menghitung jumlah packet, coba lacak juga ip penyerang tersebut!

attachment: same as ATM or ATP or FTP ? 🤔

author: youdaemon

nc 10.15.40.20 10006
</details>

## Pengerjaan Soal

# Creds
<details><summary>Soal</summary>
Attacker menyadari jika dia bisa membuat clone ftp server dari target, temukan kredensialn dari server ftp yang dibuat oleh attacker

author: youdaemon

nc 10.15.40.20 10007
</details>

## Pengerjaan Soal

# malwleowleo
<details><summary>Soal</summary>
Dapatkah kamu menemukan file malware yang dikirim oleh attacker melalui ftp?

attachment: same as creds

author: youdaemon

nc 10.15.40.20 10008
</details>

## Pengerjaan Soal

# whoami
<details><summary>Soal</summary>
Dapatkah kamu menemukan siapa identitas attacker?

attachment: same as creds

author: youdaemon

nc 10.15.40.20 10009
</details>

## Pengerjaan Soal

# secret    
<details><summary>Soal</summary>
Temukan pesan rahasia dari attacker

attachment: same as creds

author: youdaemon

nc 10.15.40.20 10010
</details>

## Pengerjaan Soal

# fuzz
<details><summary>Soal</summary>
My website got hacked. Can you analyze this network traffic to help me track the attacker?

Attachment: here

Author: kiseki

nc 10.15.40.20 10001
</details>

## Pengerjaan Soal

# malwaew
<details><summary>Soal</summary>
Ini adalah network traffic dari salah satu komputer di DPSSI yang terkena malware. Pak Sunhi, memintamu untuk membantu menganalisisnya. Bantulah Pak Sunhi untuk menemukan malware tersebut.

note: Network traffic berikut mengandung malware asli. #DYOR Attachment: attachment

Author: kiseki

nc 10.15.40.20 10003
</details>

## Pengerjaan Soal




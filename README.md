# Laporan Resmi Jarkom Modul 1
## Kelompok IT01
| NRP | Nama |
| ------ | ------ |
| 5027221042 |Nicholas Marco Weinandra |
| 5027221052 | Mochamad Zidan Hadipratama |

## Daftar Isi
- [ATM or ATP or FTP?](#atm-or-atp-or-ftp)
- [Evidence](#evidence)
- [How Many Packets?](#how-many-packets)
- [Trace Him](#trace-him)
- [Creds](#creds)
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
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/ftp1.png"><br/>
Dari capture salah satu packet tersebut, dapat diketahui bahwa response dari server jika attacker salah memasukkan password adalah "Login incorrect."
<br/><br/>
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
**Pertanyaan 1**: Apa domain milik korban?

1. Pertama, dianalisa terlebih dahulu IP attacker dan IP tujuan. Setelah diketahui, maka diaplikasikan display filter ```ip.src==192.168.1.7 && ip.dst==192.168.1.5``` untuk mencari packet dimana client dan server terhubung. <br/>Darisitu maka akan ditemukan nama domain korban yaitu ```nanomate-solutions.com```.<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/evidence1.png"><br/>

**Pertanyaan 2**: Apa web server yang digunakan oleh korban?

2. Untuk mencari webserver yang digunakan, dapat digunakan display filter ```ip.src==192.168.1.7 && ip.dst==192.168.1.5&&http``` untuk memfilter packets dari IP attacker ke IP tujuan dengan protokol HTTP. Lalu kita akan follow HTTP stream dari packet yang memiliki request method GET. <br/>Maka ditemukan bahwa webserver yang digunakan adalah ```apache-2.4.56```<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/evidence2.png"><br/>

**Pertanyaan 3**: Apa endpoint yang digunakan untuk login sebagai user biasa?

3. Untuk mencari endpoint, cara yang digunakan sama dengan nomor sebelumnya yaitu menggunakan display filter ```ip.src==192.168.1.7 && ip.dst==192.168.1.5&&http```. Namun, sekarang akan dicari HTTP stream dari packet yang memiliki request method POST. <br/>Maka ditemukan bahwa endpoint yang digunakan adalah ```app/includes/process_login.php```
<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/evidence3.png"><br/>

**Pertanyaan 4**: Apa email dan password yang berhasil digunakan untuk login sebagai user biasa?

4. Dengan mengetahui respon dari server apabila login attempt attacker gagal adalah Invalid Username or Password, maka kita dapat mencari login attempt attacker yang berhasil dengan mencari kata kunci "Login Success". Oleh karena itu, digunakan display filter ```tcp and frame contains "Login Success"```, kemudian follow TCP stream-nya. <br/>Email yang ditemukan adalah ```tareq@gmail.com``` dengan password ```tareq@nanomate```<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/evidence5.png"><br/>


Flag: ```JARKOM2024{m4innya_h3bat_Ih8lXzpflzktkAB}```



# How Many Packets?
<details><summary>Soal</summary>
Sebagai kewajiban untuk laporan, aku diminta untuk mencari tahu berapa kali attempt login yang dilakukan oleh hacker. Dapatkah kamu membantuku untuk menganalisanya?

attachment: same as ATM or ATP or FTP ? 🤔

author: youdaemon

nc 10.15.40.20 10005

**Pertanyaan**: Berapa total attempt login(bruteforce) yang dilakukan oleh hacker?
</details>

## Pengerjaan Soal
**Pertanyaan**: Berapa total attempt login(bruteforce) yang dilakukan oleh hacker?

1. Dengan diketahui bahwa IP dari attacker adalah 10.15.40.20 dan IP korban adalah 10.30.3.4, dan response dari server ketika attempt login gagal adalah "Login incorrect", maka kita dapat menggunakan display filter ```ip.src==10.15.40.20 && ip.dst==10.30.3.4&&tcp and frame contains "Login incorrect"``` untuk mencari total attempt login.<br/>
Hasilnya dapat kita lihat pada berapa packet yang di display pada kanan bawah layar.<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/howmanypackets.png"><br/>

Jawaban Soal: ```934```

Flag: ```JARKOM2024{c0unT_uR_P4cket5_cJ8kXcxygAFs84B}```

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

**Pertanyaan**: Siapa nama attacker yang sudah melakukan serangan ini?
</details>

## Pengerjaan Soal

**Pertanyaan**: Siapa nama attacker yang sudah melakukan serangan ini?

1. Pada attempt login yang dilakukan oleh attacker, ditemukan bahwa attacker akan mengirimkan suatu file malware bernama ```m4L1c10us_W4re.c```.<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/whoami1.png"><br/>

2. Download file yang akan dikirimkan oleh attacker dengan cara ```File->Export Objects->FTP-Data```. Kemudian decode pesan yang ada di file .c tersebut dengan menggunakan base64.
<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/whoami2.png"><br/>

3. Setelah di decode, maka hasilnya adalah "Hello my name is Paul Atreides" yang berarti jawabannya adalah ```Paul Atreides```.


Flag: ```JARKOM2024{Duk3_0f_4rak!s_LISAN AL GHAIB!_9Jf8RO9Hl6dsR8q}```


# secret    
<details><summary>Soal</summary>
Temukan pesan rahasia dari attacker

attachment: same as creds

author: youdaemon

nc 10.15.40.20 10010

**Pertanyaan**: Pertanyaan: Ternyata attacker menyisipkan file lainnya selain dari file malware, temukan pesan yg dikutip oleh attacker?
</details>

## Pengerjaan Soal
**Pertanyaan**: Pertanyaan: Ternyata attacker menyisipkan file lainnya selain dari file malware, temukan pesan yg dikutip oleh attacker?

1. Langkah pertama adalah untuk mencari packet yang mengandung login attempt dari attacker. Dapat dicari dengan menggunakan display filter ```tcp and frame contains "Login incorrect"```
<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/mirza1.png"><br/>

2. Pada attempt login yang dilakukan oleh attacker, ditemukan bahwa attacker akan mengirimkan suatu file image ```MIRZA.jpg```<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/mirza2.png"><br/>

3. Kita dapat men-download file MIRZA.jpg dengan cara ```File->Export Objects->HTTP```. Setelah di download, maka gambarnya adalah<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/mirza3.png"><br/>

4. Dari gambar tersebut dapat diketahui bawha pesan rahasia yang dikutip oleh attacker adalah ```MIO MIRZA```


Flag: ```JARKOM2024{l0_Blm_tW_MIO_MIRZA?_cJClvcAHlAVHR8t}```


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

**Pertanyaan**: Apa SHA-256 hash dari malware yang dimaksud?
</details>

## Pengerjaan Soal
**Pertanyaan**: Apa SHA-256 hash dari malware yang dimaksud?

1. Pertama, agar dapat melakukan memfilter jenis protocol HTTP, maka perlu dilakukan decode TLS terlebih dahulu menggunakan file ```keylog.txt``` yang diberikan.<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/malwaew1.png"><br/>

2. Kemudian, memasukkan command display filter ```http``` untuk memfilter packet HTTP. Setelah ditampilkan hasilnya, ditemukan ada satu packet yang mencurigakan. Packet tersebut melakukan request GET file .dll<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/malwaew2.png"><br/>

3. Setelah ditemukan attacker mengirimkan file .dll, maka harus di-download dengan cara ```File->Export Objects->HTTP``` dan save file ```invest_20.dll```<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/malwaew.png"><br/>

4. Setelah file ```invest_20.dll``` didapatkan, langkah selanjutnya adalah untuk melakukan hash SHA-256 dari file tersebut. Digunakan command ```shasum -a 256 invest_20.dll``` pada terminal.<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/malwaeww.png"><br/>

5. Maka akan ditemukan hasil hash SHA-256 dari file malware, yaitu ```31cf42b2a7c5c558f44cfc67684cc344c17d4946d3a1e0b2cecb8eb58173cb2f```<br/>
<img src= "https://github.com/ZidanHadipratama/jarkom-Modul-1-2024-IT01/blob/main/gambar/malwaewww.png"><br/>
<br/>

Flag: ```JARKOM2024{wow_how_u_solve_this_uJ8RvcAHizFtR89}```



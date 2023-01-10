# PDF Analyzer Lets Defend Challenges

![image](https://user-images.githubusercontent.com/43168046/210167881-5ad9edce-fed1-4012-ad2e-a8420dd3122a.png)

### Link : 
https://app.letsdefend.io/challenge/pdf-analysis

### Latar Belakang Masalah : 
Salah satu karyawan telah menerima email yang mencurigakan dengan detail sebagai berikut:

- Email From: SystemsUpdate@letsdefend.io 
- Email To: Paul@letsdefend.io 
- Subject: Critical - Annual Systems UPDATE NOW 
- Body: Tolong lakukan proses updated sebelum batas waktu hari ini. 
- Lampiran: Update.pdf 
- Kata Sandi: letsdefend

Karyawan telah melaporkan kejadian ini kepada tim SOC sebagai tim analisis kemanan dengan maksud memvalidasi email tersebut. Karyawan tersebut juga memberikan informasi bahawa mereka tidak mengunduh atau membuka lampiran yang dikirimkan dikarenakan merasa sangat mencurigakan. 

### Noted : Tolong berhati-hati dalam membuka file tersebut dikarenakan file tersebut benar termasuk berbahaya. 

# Analisis
Proses analisis dimulai dengan saya mencoba mendownload file tersebut terlebih dahulu, dan saya langsung membukanya pada environtment linux di virtualbox dengan host only untuk meminimalisir hal yang tidak diinginkan.  File tersebut kemudian saya ektrak dengan menggunakan <a href="#"><code>peepdf</code></a>, dilanjutkan dengan menganalisisnya menggunakan tools <a href="https://github.com/jesparza/peepdf" target="_blank"><code>peepdf</code></a>. 

![image](https://user-images.githubusercontent.com/43168046/210489392-8b56f939-dc0f-4523-8916-f7945f88dc6f.png)

Berdasarkan hasi dari pengecekkan diatas, kami menemukan malicious object pada object 19. Terlihat dari awalan object tersebut menggunakan proses encode, sehingga kami mencoba untuk mendecoded object tersebut.  Diketahui selain menggunakan proses encoding berbasis base 64, attacker juga menggunakan prose reversing object yang berfungsi untuk mengubah urutan kata dari tiap huruf pada object tersebut. 

![image](https://user-images.githubusercontent.com/43168046/210490648-f10ab7d8-7ae2-4367-a0d2-6c3d0d8eeb96.png)

Melalui hasi encoding object 19, kami mengetahui bahwa attacker mencoba menargetkan serangan pada local directori yaitu  <code>C:\documents\</code> dengan membuat malware berkestensi <code>zip</code> dan memiliki nama <code>d0csz1p</code>. 

Setelah mengetahui informasi tersebut, kami berusaha mencari informasi lebih detail mengenai file pdf tersebut. Berdasarkan objec 33, kami mengetahui bahwa object tersebut telah melewati proses obfuscate by Javasript. Sehingga untuk membaca object tersebut perlu proses deobfuscate, pada proses ini kami menggunakan batuan tools <a href="https://beautifier.io/"><code>beautifier.io.</code></a>.

![image](https://user-images.githubusercontent.com/43168046/210493323-eabb88f0-91f9-486a-87c2-acc6b822a6e2.png)

Terlihat pada gambar dibawah, hasil object yang telah berhasil di deobfuscate sehingga object tersebut lebih mudah untuk dianalisis.

![image](https://user-images.githubusercontent.com/43168046/210496308-2b591877-ac73-494d-9444-af4ab0158275.png)

diketahui attacker menggunakan external web domain <code>filebin.net</code> dalam melancarkan aksinya. Terlihat juga attacker menggunakan metode <code>POST</code> dalam menjalankan req url tersebut.

Setelah berhasil menganalisis object 19 sebalumnya, kami kembali fokus pada object lain yang masuk dalam kategori </>Objects with JS code</code> yaitu object 26. Dimana pada object 26 ini kami menemukan nilai variabel $base64, sehingga kami daapt langsung menjalankannya untuk mendapatkan command perintah asli yang dikeluarkannya. Disini kami menggunakan tools <a href="https://tio.run/#powershell">tio.run<></a>.
  
![image](https://user-images.githubusercontent.com/43168046/210505530-f5b99ee5-87c3-4038-92b6-56f44e699e8d.png)

![image](https://user-images.githubusercontent.com/43168046/210505586-1ca061c7-fdca-49bd-a075-20babd70b332.png)

berdasarkan hasil ouput yang didapet dari proses debug melalui tools tersebut kami mendapakan bahwa attacker menggunakann tools WMIC untuk menjalankan mekanisme persistensi. singkatan dari Windows Management Interface Command, adalah alat prompt perintah sederhana yang mengembalikan informasi tentang sistem tempat Anda menjalankannya.  Berdasarkan hasil yang kami dapatkat proses persisntensi yang coba dilaukan berlangsung selama 9000 detik atau sekita 2,5 jam . Selain mengetahui informasi tersebut, kami juga mendapati informasi yang berupa binari yang dapat dimanfaatkan oleh attacker dalam basis pasca-eksploitasi, untuk mempertahankan atau meningkatkan hak istimewa. berdasarkan informasi yang kami dapatkan dari webiste <a href="https://lolbas-project.github.io/">lolbash.io</a>, attacker menggunakan lolbin <code>Powerpnt.exe</code>, yang berguna untuk membantu menjalankan proses download payload dari server jarak jauh. 

![image](https://user-images.githubusercontent.com/43168046/211643184-d289cc5f-e4ba-42f2-b9ad-cecd19b6819a.png)

Setelah mengetahui bahwa attacker menggunakan lolbin <code>Powerpnt.exe</code>, kami dengan mudah mengetahui file apa yang menjadi tujuan dari dari proses tersebut. Terlihat bahwa attacker berusahan menjalankan proses download file yang bernama <code>wallpaper482.scr</code>. File tersebut terkoneksi dengan eksternal ip yaitu <code>60*187*184*54 </code> yang berasal dari negara china. Ip Tersebut termasuk dalam bad reputation dibuktikan dengan 5 security vendors flagged this IP address as malicious pada virustotals. 

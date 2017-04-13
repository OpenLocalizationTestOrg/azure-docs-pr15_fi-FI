<properties
   pageTitle="Vianmääritys hidas varmuuskopion tiedostoja ja kansioita Azure varmuuskopion | Microsoft Azure"
   description="Vianmäärityksen ohjeita, joiden avulla voit selvittää Azure varmuuskopiointi suorituskykyongelmia syy"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Vianmääritys hidas varmuuskopion tiedostoja ja kansioita Azure varmuuskopiointi

Tässä artikkelissa on vianmäärityksen ohjeita voit selvittää tiedostojen ja kansioiden varmuuskopioinnin hidasta syy, kun käytät Azure varmuuskopion. Kun käytät Azure Backup agentti voit varmuuskopioida tiedostot, varmuuskopiointi saattaa kestää odotettua kauemmin. Tämän viiveen syynä saattaa olla vähintään yksi seuraavista toimista:

-   [Varmuuskopioidaan Jos tietokoneeseen on suorituskyvyn pullonkaulojen.](#cause1)
-   [Toinen prosessi tai virustentorjuntaohjelman häiritsee Azure varmuuskopiointi-prosessin.](#cause2)
-   [Varmuuskopiointi-agentti käytössä Azure virtuaalikoneen (AM).](#cause3)  
-   [Olet varmuuskopioidaan on runsaasti (miljoonia) tiedostoja.](#cause4)

Ennen kuin aloitat vianmääritys, suosittelemme, että lataat ja asennat [uusimman Azure Backup agentti](http://aka.ms/azurebackup_agent). Usein käytetyt päivitykset on tehdä varmuuskopio-agentti eri korjaaminen, ominaisuuksien ja suorituskyvyn parantaminen.

On suositeltavaa myös tarkistaa varmistaaksesi, että synkronoinnissa ei mitään yleisiä määritysongelmat [Azure varmuuskopiointi palvelun usein kysytyt kysymykset](backup-azure-backup-faq.md) .

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Syy: Suorituskyvyn pullonkaulojen tietokoneessa

Pullonkaulojen käyttöön tietokoneessa, jossa varmuuskopioidaan voi aiheuttaa viivästyksiä. Esimerkiksi tietokoneen mahdollisuus lukeminen tai tiedostoon levylle tai kaistanleveyttä lähettää tietoja verkossa, voivat aiheuttaa pullonkauloja.

Windowsissa on valmiita työkalu, jonka nimi on [Suorituskyvyn valvonnan](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) esiintyvien nämä pullonkaulojen.

Tässä on muutamia suorituskykylaskureita ja alueet, jotka voivat olla hyödyllisiä määrityksessä pullonkaulojen optimaalisen varmuuskopioiden hakeminen.

| Laskuri  | Tila  |
|---|---|
|Loogisen levyn (fyysinen levy) – käyttämättömänä %   | • 100 %: n vapaa käyttämättömänä 50 % = kunnossa</br>• 49 % vapaa käyttämättömänä 20 % = varoituksia tai näyttö</br>• 19 prosenttia vapaa käyttämättömänä 0 % = kriittinen tai poissa määritys|
|  Loogisen levyn (fyysinen levy)--% keskimääräinen Levyn Sec luku- tai kirjoitusoikeudet |  • 0.001 ms 0.015 MS = kunnossa</br>• 0.015 ms 0,025 MS = varoituksia tai näyttö</br>• 0.026 ms tai = enää kriittinen tai poissa määritys|
|  Loogisen levyn (fyysinen levy) – Nykyinen levyn jono (Saat kaikki esiintymät) | yli 6 tuntia 80 pyyntö |
| Muistin--ei Sivutettua|• Alle 60 prosenttia resurssivarantoon kulutettu = kunnossa<br>• 61 80 % resurssivarantoon kulutettu = varoituksia tai näyttö</br>• Yli 80 % resurssivarantoon kulutettu = kriittinen tai poissa määritys|
| Muistin--Sivutettua |• Alle 60 prosenttia resurssivarantoon kulutettu = kunnossa</br>• 61 80 % resurssivarantoon kulutettu = varoituksia tai näyttö</br>• Yli 80 % resurssivarantoon kulutettu = kriittinen tai poissa määritys|
| Muistin--käytettävissä megatavua| • 50 prosenttia vapaata muistia tai = Lisää kunnossa</br>Muisti käytettävissä • 25 % = näyttö</br>• 10 prosenttia vapaata muistia = varoitus</br>• Alle 100 Mt tai vapaata muistia 5 % = kriittinen tai poissa määritys|
|Suoritin--\%ajan (kaikki esiintymät)|• Alle 60 prosenttia kulutettu = kunnossa</br>• 61 90 % kulutettu = näytön tai varoitus</br>• 91 100 % kulutettu = kriittinen|


> [AZURE.NOTE] Jos päätät, että infrastruktuuri on virheen aiheuttaja, on suositeltavaa eheyttää levyjen säännöllisesti suorituskyvyn parantamiseksi.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Syy: Toinen prosessi tai virustentorjuntaohjelman häiritse Azure varmuuskopiointi

Olemme katsomista useita esiintymät, johon Windows-järjestelmän muiden prosessien heikentää ovat vaikuttaneet suorituskyvyn Azure varmuuskopiointi agent-prosessin. Esimerkiksi jos käytät Azure Backup agent ja toisessa ohjelmassa varmuuskopioida tiedot tai jos virustentorjuntaohjelma on käynnissä ja varmuuskopioitavien tiedostojen lukituksen, kerrannainen lukitsee tiedostoja voi aiheuttaa ristiriita. Tässä tilanteessa varmuuskopioinnin voi epäonnistua tai työn saattaa kestää odotettua kauemmin.

Tässä skenaariossa parhaat suositus on muiden näkevän Azure Backup agentti varmuuskopion ajan muuttuu varmuuskopion ohjelma käytöstä. Yleensä varmistamalla, että useita varmuuskopion töitä käytössäsi ei ole yhtä aikaa on riittävä, jotta ne eivät vaikuttavat toisiinsa.

Virustentorjuntaohjelmien on suositeltavaa poistaa seuraaviin tiedostoihin ja kansioihin:

- C:\Program Files\Microsoft Azure palautus Services Agent\bin\cbengine.exe kuin prosessi
- C:\Program Files\Microsoft Azure palautus Services Agent\ kansiot
- Scratch sijainti (Jos et käytä vakio sijainti)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Syy: Backup agentti Azure virtuaalikoneen käytössä

Jos käytät Backup agent AM-suorituskyky on hitaammin, kun suoritat sen fyysinen koneeseen. Tämä on normaalia IOPS rajoitusten takia.  Voit kuitenkin optimoida suorituskyvyn valitsemalla haluamasi tiedot asemat varmuuskopioidaan Azure Premium säilöön. Pyrimme parhaillaan korjaamaan virheen ongelman ja korjaustyökalu on saatavana uuteen versioon.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Syy: Varmuuskopioiminen on runsaasti (miljoonia) tiedostot

Päivittäin erittäin paljon tietojen siirtäminen kestää kauemmin kuin siirtäminen pienempi tietoja on paljon. Joissakin tapauksissa varmuuskopiota liittyy ei vain tiedot, mutta määrä tiedostojen tai kansioiden kokoa. Tämä on erityisen TOSI, kun pieniä tiedostoja (muutaman kilotavua muutaman tavua) miljoonia varmuuskopioidaan.

Tämä johtuu siitä samalla, kun olet varmuuskopioimalla tiedot ja siirtää sen Azure-Azure samanaikaisesti luetteloimisesta tiedostot. Joissakin harvinaisissa tilanteissa luettelon saattaa kestää odotettua kauemmin.

Seuraavien ilmaisimien osalta auttavat ymmärtämään pullonkaula ja käsitellä vastaavasti seuraavat vaiheet:

- **Käyttöliittymän on näkyvissä tiedonsiirron edistymistiedot**. Tietoja siirretään edelleen. Verkon kaistanleveyden tai tietojen koon saattaa aiheuttaa viivästyksiä.

- **Käyttöliittymän, ei näy tiedonsiirron edistymistiedot**. Avaa osoitteessa C:\Microsoft Azure palautus Services Agent\Temp lokit ja valitse lokit FileProvider::EndData-merkinnän. Tämän arvon ilmaisee, että tiedonsiirron valmiiksi ja luettelotoiminto tapahtuu. Älä Peruuta varmuuskopion työt. Sen sijaan Odota hieman enää luettelon toiminnon loppuun. Jos ongelma jatkuu, ota yhteyttä [Azure tukevat](https://portal.azure.com/#create/Microsoft.Support).

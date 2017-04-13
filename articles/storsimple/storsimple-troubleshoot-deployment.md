<properties 
   pageTitle="StorSimple käyttöönoton vianmääritys | Microsoft Azure"
   description="Vianmääritys ja korjaus virheitä, kun otat StorSimple käsitellään."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>StorSimple laitteen käyttöönoton ongelmien vianmääritys

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on hyötyä vianmäärityksen ohjeita Microsoft Azure StorSimple käyttöönottoa varten. Tässä artikkelissa kuvataan, yleisiä ongelmia, mahdollisia syitä ja suositeltua vaihetta, joiden avulla voit ratkaista ongelmat, jotka voi kärsiä, kun määrität StorSimple. Nämä tiedot koskevat StorSimple paikallisen fyysinen laite ja StorSimple virtual laite.

> [AZURE.NOTE] Laitteen määrittäminen liittyvät ongelmat, jotka voivat yleisölle tarkoitetun voi ilmetä, kun otat laitteen ensimmäistä kertaa tai ne voi ilmetä myöhemmin, kun laite on toiminnassa. Tässä artikkelissa keskitytään uusille käyttöönoton vianmääritys. Siirry vianmääritys toiminnallisia laitteen [toiminnallisia laitteen ongelmien vianmääritys](storsimple-troubleshoot-operational-device.md).

Tässä artikkelissa kuvataan vianmäärityksen StorSimple ominaisuuksissa Työkalut myös ja on vaiheittaiset ohjeet vianmäärityksen esimerkiksi.

## <a name="first-time-deployment-issues"></a>Uusille käyttöönotto-ongelmat

Jos kohtaat ongelma, kun otat laitteen ensimmäistä kertaa, toimi seuraavasti:

- Jos vianmäärityksen fyysistä laitetta, varmista, että laitteen asentanut ja määrittänyt kuvatulla [StorSimple 8100 laitteen asentaminen](storsimple-8100-hardware-installation.md) tai [asentaa StorSimple 8600 laitteen](storsimple-8600-hardware-installation.md).
- Tarkista edellytykset käyttöönottoa varten. Varmista, että kaikki tiedot, jotka on kuvattu [käyttöönoton määrityksen tarkistusluettelo](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
- Tarkista StorSimple julkaisutiedot nähdäksesi, jos ongelma on kuvattu. Julkaisutiedot ovat tunnetut asennusongelmat ratkaisuehdotuksia. 

Laitteen käyttöönoton aikana yleisimpiä ongelmia, jota käyttäjät hymiö ilmetä suoritettaessa ohjatun määritystoiminnon ja kun ne rekisteröityä Windows PowerShellin kautta laitteen StorSimple. (Voit käyttää Windows PowerShell StorSimple StorSimple laitteen määrittäminen ja rekisteröiminen. Katso lisätietoja laitteen rekisteröinti [Vaihe 3: Määritä ja Windows PowerShellin kautta laitteen Rekisteröidy StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Seuraavissa osissa avulla voit ratkaista ongelmat, jotka käytössä ilmenee, kun määrität StorSimple laitteen ensimmäisen kerran.

## <a name="first-time-setup-wizard-process"></a>Uusille ohjatun toiminnon vaihe

Seuraavat vaiheet on yhteenveto ohjatun asennuksen. Saat yksityiskohtaiset tiedot, [Ota paikallisen StorSimple laitteen](storsimple-deployment-walkthrough.md).

1. Käynnistä ohjattu määritystoiminto opastaa sinua annettuja ohjeita, [Käynnistä HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) cmdlet-komennon suorittaminen 
2. Verkon määrittäminen: ohjatun määritystoiminnon avulla voit määrittää tietojen 0 verkkoliittymän verkkoasetukset StorSimple laitteen. Nämä asetukset ovat seuraavat:
  - Virtuaalinen IP (VIP), aliverkkopeite ja gateway – [Määrittäminen HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet suoritetaan taustalla. Se määrittää IP-osoite, aliverkkopeite ja yhdyskäytävän tiedot 0 verkko-liittymän StorSimple laitteen.
  - Ensisijainen DNS-palvelin – [Määrittäminen HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet suoritetaan taustalla. Se määrittää StorSimple-ratkaisun DNS-asetukset.
  - NTP-palvelin – [Määrittäminen HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet suoritetaan taustalla. Se määrittää StorSimple-ratkaisun NTP palvelimen asetukset.
  - Valinnainen web-välityspalvelimen – [Määrittäminen HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet suoritetaan taustalla. Se asettaa ja mahdollistaa StorSimple ratkaisu web-välityspalvelimen määritykset.
3. Salasanojen määrittäminen: Seuraava vaihe on määritettävä laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanat. Jos käytössäsi on päivitys 1, valitse sinun ei tarvitse StorSimple tilannevedoksen hallinta salasanan määrittäminen.
  - Laitteen järjestelmänvalvojan salasanan käytetään kirjautua laitteeseen. Oletus-laitteen salasana on **Salasana1**.
  - StorSimple tilannevedoksen hallinta salasana on pakollinen, kun määrität laite, jota käytetään StorSimple tilannevedoksen hallinta. Sinun on ensin määritettävä salasana ohjatun ja sitten voit määrittää ja muuttaa StorSimple hallinta-palvelusta. Salasana todentaa laitteen StorSimple tilannevedoksen hallinta.
 
    > [AZURE.IMPORTANT] Salasanojen kerääminen ennen rekisteröitymistä, mutta voimaan vasta, kun Rekisteröi laite onnistuneesti. Jos on ilmennyt virhe, kun haluat ottaa käyttöön salasanan, jonka pyydetään antamaan salasana uudelleen, ennen kuin tarvittavat salasanoista (, jotka täyttävät monimutkaisuus) kerätään.

4. Rekisteröi laite: viimeinen vaihe on Rekisteröi laite suorittamalla: Microsoft Azure StorSimple hallinta-palvelussa. Rekisteröinti edellyttää hankkia [palvelun rekisteröinti avain](storsimple-manage-service.md#get-the-service-registration-key) perinteinen Azure-portaalista ja antaa ohjatun määritystoiminnon ohjeita. Laitteen rekisteröinti onnistuu, kun sinulle on annettu palvelun tiedot-salausavaimen. Muista pitää tämän salausavaimen turvalliseen paikkaan, koska se on suoritettava rekisteröidä kaikki seuraavat laitteet-palveluun.

## <a name="common-errors-during-device-deployment"></a>Yleisten virheiden laitteen käyttöönoton aikana

Seuraavissa taulukoissa luetellaan yleisiä virheitä saattaa ilmetä milloin voit:

- Määritä tarvittavat asetukset.
- Valinnainen web-välityspalvelimen asetusten määrittäminen.
- Laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanojen määrittäminen. 
- Rekisteröi laite. 

## <a name="errors-during-the-required-network-settings"></a>Virheet tarvittavat verkkoasetukset aikana

| Ei.| Virhesanoma | Mahdollisia syitä | Suositellut toimet |
| ---| ------------- | --------------- | ------------------ |
| 1  | Käynnistää-HcsSetupWizard: Tämä komento voidaan suorittaa vain aktiivisen ohjaimen. | Määritys on meneillään passiivinen ohjaimen.| Suorita tämä komento lähettämät aktiivinen. Lisätietoja on kohdassa [Identify aktiivinen ohjauskoneen, laitteessasi](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Käynnistää-HcsSetupWizard: Laite ei ole valmis. | Näytettäviä tietoja 0 verkkoyhteyden liittyvät ongelmat.| Tarkista fyysinen verkkoyhteyden tietojen 0.|
| 3 | Käynnistää-HcsSetupWizard: On IP-osoitteiden ristiriidan verkossa toisen järjestelmän kanssa (poikkeus kohteesta HRESULT: 0x80070263). | 0 tiedoille annettu IP oli jo käytössä toisessa järjestelmässä. | Anna uusi IP, joka ei ole käytössä.|
| 4 | Käynnistää-HcsSetupWizard: Klusteriresurssin epäonnistui. (Poikkeus kohteesta HRESULT: 0x800713AE). | Kaksoiskappaleiden VIP. Annettu IP on jo käytössä.| Anna uusi IP, joka ei ole käytössä.|
| 5 | Käynnistää-HcsSetupWizard: Virheellinen IPv4-osoite. | IP-osoite on annettu muoto on virheellinen.| Tarkista muoto ja kirjoita IP-osoite uudelleen. Lisätietoja on artikkelissa [Ipv4-osoitteet][1]. |
| 6 | Käynnistää-HcsSetupWizard: Virheellinen IPv6-osoite. | IP-osoite on annettu muoto on virheellinen.| Tarkista muoto ja kirjoita IP-osoite uudelleen. Lisätietoja on artikkelissa [Ipv6-osoitteet][2].|
| 7 | Käynnistää-HcsSetupWizard: On useita päätepisteitä saatavissa päätepistekartoituksen. (Poikkeus kohteesta HRESULT: 0x800706D9) | Klusterin-toiminto ei toimi. | [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) vaiheisiin.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Valinnainen web-välityspalvelimen aikaiset virheet

| Ei.| Virhesanoma | Mahdollisia syitä | Suositellut toimet |
| ---| ------------- | --------------- | ------------------ |
| 1  | Käynnistää-HcsSetupWizard: Parametri ei kelpaa (poikkeus kohteesta HRESULT: 0x80070057) | Jokin säädetty välityspalvelimen asetuksia ei ole kelvollinen.| URI ei ole annettu oikeassa muodossa. Käytä seuraavaa muotoa: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Käynnistää-HcsSetupWizard: RPC palvelin ei ole käytettävissä (poikkeus kohteesta HRESULT: 0x800706ba) | Pääkansio syynä on jokin seuraavista:<ol><li>Klusterin ei ole ylös.</li><li>Passiivinen ohjain ei saa yhteyttä aktiivinen ohjauskoneen ja komento suoritetaan lähettämät passiivinen.</li></ol> | Pääkansio: syy mukaan<ol><li>[Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) varmistaaksesi, että klusterin on ylöspäin.</li><li>Suorita-komento lähettämät aktiivinen. Jos haluat suorittaa komennon passiivinen ohjauskoneen, sinun on varmistettava passiivinen ohjauskoneen kommunikoida aktiivinen ohjaimella. Sinun on, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) , jos tämä on katkennut.</li></ol> |
| 3 | Käynnistää-HcsSetupWizard: Etäproseduurikutsu epäonnistui (poikkeus kohteesta HRESULT: 0x800706be) | Klusteria ei ole käytettävissä. | [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) varmistaaksesi, että klusterin on ylöspäin.|
| 4 | Käynnistää-HcsSetupWizard: Klusterin resurssia ei löydy (poikkeus kohteesta HRESULT: 0x8007138f) | Klusteriresurssin ei löydy. Näin voi käydä, jos asennus ei ole oikein. | Voit joutua factory-oletusasetusten palauttaminen laite. [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) ja luo klusteriresurssi.|
| 5 | Käynnistää-HcsSetupWizard: Klusterin resurssi ei online (poikkeus kohteesta HRESULT: 0x8007138c)| Klusteriresurssit eivät ole online-tilassa. | [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) vaiheisiin.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanat liittyvien virheiden

Oletusarvoinen laitteen järjestelmänvalvojan salasana on **Salasana1**. Salasana vanhentuu ensimmäisen kirjautumisen; Tämän vuoksi sinun on ohjatun määritystoiminnon avulla voit muuttaa sitä. Kun rekisteröidyt laitteen ensimmäistä kertaa, sinun on määritettävä uuden laitteen järjestelmänvalvojan salasanan. 

Jos käytät Windows Server-isännän StorSimple tilannevedoksen hallinta ohjelmiston hallitsee laitetta, valitse Anna myös StorSimple tilannevedoksen hallinta salasanan ensimmäistä kertaa rekisteröinnin yhteydessä. 

Varmista, että salasana on täytettävä seuraavat vaatimukset:

- Laitteen järjestelmänvalvojasalasana on oltava 8-15 merkkiä.
- StorSimple tilannevedoksen hallinta salasana on oltava 14 tai 15 merkkiä.
- Salasanojen tulisi sisältää seuraavanlaisia 4 merkin 3: pienet, isot, numeeriset ja määräten. 
- Salasana ei voi olla sama kuin viimeisen 24 salasanat.

Lisäksi pitää mielessä, salasanojen päättyy vuosittain eikä voi muuttaa vain, kun olet rekisteröinyt onnistuneesti laite. Jos rekisteröinti epäonnistuu jostakin syystä, salasanat ei voi muuttaa. Lisätietoja laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanoja Tutustu [StorSimple hallintapalveluun, voit muuttaa StorSimple salasanat](storsimple-change-passwords.md).

Vähintään yksi seuraavista virheistä saattaa ilmetä, kun laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanojen määrittäminen.

| Ei.| Virhesanoma | Suositellut toimet |
| ---| ------------- | ------------------ | 
| 1  | Salasana on suurempi kuin enimmäispituus. |Käytä salasanaa, joka täyttää seuraavat vaatimukset:<ul><li>Laitteen järjestelmänvalvojasalasana on oltava 8-15 merkkiä.</li><li>StorSimple tilannevedoksen hallinta salasana on oltava 14 tai 15 merkkiä.</li></ul> | 
| 2 | Salasana ei vastannut vaadittu pituus. | Käytä salasanaa, joka täyttää seuraavat vaatimukset:<ul><li>Laitteen järjestelmänvalvojasalasana on oltava 8-15 merkkiä.</li><li>StorSimple tilannevedoksen hallinta salasana on oltava 14 tai 15 merkkiä.</lu></ul> |
| 3 | Salasanassa on oltava pieniä kirjaimia. | Salasanojen on oltava 3 / 4 merkin seuraavanlaisia: pienet, isot, numeeriset ja määräten. Varmista, että salasana täyttää näitä vaatimuksia. |
| 4 | Salasanassa on oltava numeerista merkkiä. | Salasanojen on oltava 3 / 4 merkin seuraavanlaisia: pienet, isot, numeeriset ja määräten. Varmista, että salasana täyttää näitä vaatimuksia. |
| 5 | Salasanassa on oltava erikoismerkkejä. | Salasanojen on oltava 3 / 4 merkin seuraavanlaisia: pienet, isot, numeeriset ja määräten. Varmista, että salasana täyttää näitä vaatimuksia. |
| 6 | Salasanassa on oltava 3 / 4 merkin seuraavanlaisia: isoja, pieni, numeerinen ja määräten. | Salasana ei sisällä merkkejä tarvittavat tyypit. Varmista, että salasana täyttää näitä vaatimuksia. |
| 7 | Parametri ei vastaa vahvistus. | Varmista, että salasana vastaa kaikkia ja, että olet määrittänyt oikein. |
| 8 | Salasana ei pysty sovittamaan oletusarvo. | Oletus-salasana on *Salasana1*. Sinun on muutettava salasana, kun kirjaudut sisään ensimmäisen kerran. |
| 9 | Kirjoittamasi salasana ei vastaa laitteen salasana. Kirjoita salasana uudelleen. | Tarkista salasana ja kirjoita se uudelleen. |

Salasanojen kerätään, ennen kuin laite on rekisteröity, mutta niitä käytetään vain onnistu rekisteröinnin jälkeen. Salasanan palauttaminen-työnkulku edellyttää voi rekisteröidä laite. 

> [AZURE.IMPORTANT] Yleensä ottaa käyttöön salasanan epäonnistuu, jos ohjelmiston toistuvasti yrittää kerääminen salasana, ennen kuin se onnistuu. Harvoin salasanaa ei voi käyttää. Tässä tapauksessa voit rekisteröidä laite ja jatkaa, mutta ei muuteta salasanat. Näyttöön tulee hetken, joihin salasana ei ole muutettu – laitteen järjestelmänvalvojan salasanan tai StorSimple tilannevedoksen hallinta-salasana. Jos tilanne ilmenee, on suositeltavaa, että voit muuttaa molemmat salasanat.

Voit palauttaa Azure perinteinen portaalissa kautta StorSimple hallintapalvelu salasanat. Jos haluat lisätietoja, siirry osoitteeseen: 

- [Laitteen järjestelmänvalvojan salasanan vaihtaminen](storsimple-change-passwords.md#change-the-device-administrator-password).
- [StorSimple tilannevedoksen hallinta salasanan vaihtaminen](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Laitteen rekisteröinnin yhteydessä virheet

Rekisteröi laite suorittamalla: Microsoft Azure StorSimple hallinta-palvelun avulla. Vähintään yksi seuraavista ongelmista voi ilmetä laitteen rekisteröinnin yhteydessä.

| Ei.| Virhesanoma | Mahdollisia syitä | Suositellut toimet |
| ---| ------------- | --------------- | ------------------ |
| 1  | Virhe 350027: Ei voitu rekisteröidä laitteen StorSimple hallinnan avulla. |  | Odota muutama minuutti ja yritä sitten uudelleen. Jos ongelma jatkuu, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).|
| 2  | Virhe 350013: Virhe on tapahtunut rekisteröinnissä laite. Tämä voi olla väärä palvelun rekisteröinti avainta. | | Rekisteröi laite uudelleen oikein palvelun rekisteröinti-näppäimen kanssa. Lisätietoja on artikkelissa [palvelun rekisteröinti Key-tunnuksen hankkiminen.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | Virhe 350063: Todennus välitetty StorSimple hallintapalveluun, mutta rekisteröinti epäonnistui. Yritä toiminnon jälkeen jonkin aikaa. | Tämä virhe ilmaisee, että ACS todentaminen on ohitettu, mutta palvelun tehdyt Rekisteröi kutsu epäonnistui. Tämä voi johtua ja tuloksena on arviointi verkko-ongelma. | Jos ongelma jatkuu, ota yhteyttä [Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md). |
| 4 | Virhe 350049: Palvelu ei voitu tavoittaa rekisteröinnin yhteydessä. | Kun puhelu on tehnyt palvelun, web-poikkeuksen vastaanotetaan. Joissakin tapauksissa tämä saattaa Hae alussa toimintoa myöhemmin uudelleen. | Tarkista, onko IP-osoite ja DNS-nimi ja yritä sitten uudelleen. Jos ongelma jatkuu, [Ota yhteyttä Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Virhe 350031: Laite on jo rekisteröity. | | Toiminto ei ole tarpeen. |
| 6 | Virhe 350016: Laitteen rekisteröinti epäonnistui. | |Varmista, että rekisteröinti-näppäin on oikein. |
| 7 | Käynnistää-HcsSetupWizard: On virhe; laitteen rekisteröinti Tämä voi johtua virheellinen IP-osoite tai DNS-nimi. Tarkista verkkoasetukset ja yritä sitten uudelleen. Jos ongelma jatkuu, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md). (Virhe 350050) | Varmista, että laitteesi ping ulkopuolelta verkkoon. Jos sinulla ei ole connectivity ulkopuolelta verkkoon, rekisteröinti epäonnistuu ja seuraava virhesanoma. Tämä virhe voi olla yhdistelmän vähintään yksi seuraavista toimista:<ul><li>Virheellinen IP</li><li>Virheellinen aliverkon</li><li>Virheellinen yhdyskäytävä</li><li>Väärät DNS-asetukset</li></ul> | Katso, miten [vaiheittainen vianmäärityksen Esimerkki](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Käynnistää-HcsSetupWizard: Nykyisen toiminto epäonnistui vuoksi sisäinen palveluvirhe [0x1FBE2]. Yritä toiminnon kuluttua. Jos ongelma jatkuu, ota yhteyttä Microsoft Support. | Tämä on Yleinen virhe ilmenee kaikki näkymättömät virheet palvelun tai agentti. Yleisin syy voi olla ACS-todennus epäonnistuu. Mahdollinen syy virheen on, että on ongelmia NTP server-määritys ja aikaa laitteeseen ei ole määritetty oikein. | Korjaa aika (Jos on ongelmia) ja yritä rekisteröinti-toimintoa. Jos määrittäminen HcsSystem - aikavyöhyke-komennon avulla voit muuttaa aikavyöhykkeen, isolla aikavyöhykkeen (esimerkiksi "Tyynenmeren normaaliaika").  Jos ongelma jatkuu, [yhteyshenkilön Microsoft-tuen](storsimple-contact-microsoft-support.md) ohjeet Seuraava. |
| 9 | Varoitus: Laitetta ei voitu aktivoida. Laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanat ei ole muutettu. | Jos rekisteröinti epäonnistuu, laitteen järjestelmänvalvoja ja StorSimple tilannevedoksen hallinta salasanat eivät muutu. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Työkaluja StorSimple ominaisuuksissa vianmääritys

StorSimple sisältää useita työkaluja, joilla voit tehdä vianmäärityksen StorSimple ratkaisu. Näitä ovat:

- Tuki-paketit ja laitteen lokit 
- Suunniteltu erityisesti vianmääritys cmdlet-komennot 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Vianmääritys paketit ja laitteen lokit käytettävissä tuesta

Tuki-paketti sisältää kaikki asianmukaiset lokit, joka auttaa laitteen vianmääritykseen Microsoft Support-ryhmän. Voit käyttää Windows PowerShell StorSimple luomiseen salattuja tuki-paketin, jossa voit sitten jakaa tukihenkilöstöön.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Voit tarkastella lokit tai tuki-pakkauksen sisältö

1. Windows PowerShell StorSimple avulla voit luoda tuki-paketti, kuvatulla tavalla [Luo ja hallitse tuki-paketin](storsimple-create-manage-support-package.md).

2. Lataa paikallisesti asiakastietokoneen [salauksen komentosarjan](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) .

3. Tämä [vaihe vaiheelta etenevä ohje](storsimple-create-manage-support-package.md#edit-a-support-package) avulla voit avaaminen ja salauksen tuki-paketti.

4. Purettu tuki paketin lokit ovat tapahtumien seuranta/etvx-muodossa. Voit suorittaa seuraavat toimet voit tarkastella näitä tiedostoja Windowsin Tapahtumienvalvonta:
  1. Windows-asiakasohjelman **eventvwr** -komennon suorittaminen Tämä käynnistää Tapahtumienvalvonta.
  2. Valitse **Toiminnot** -ruudun **Avaa tallennettu** loki ja osoita lokitiedostojen etvx/tapahtumien seuranta-muodossa (tuki pakkaus). Voit nyt tarkastella tiedostoa. Kun avaat tiedoston, napsautat hiiren kakkospainikkeella ja Tallenna tiedosto tekstinä.
   
    > [AZURE.IMPORTANT] Voit myös näiden tiedostojen avaaminen Windows PowerShellin **Get-WinEvent** cmdlet-komento. Lisätietoja on artikkelissa [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) Windows PowerShellin cmdlet-oppaat.

5. Kun lokit Avaa Tapahtumienvalvonta, Etsi seuraavat lokit, jotka sisältävät laitteen työnkulkuun liittyvät ongelmat:

  - hcs_pfconfig/toiminnassa loki
  - hcs_pfconfig/Config

6. Etsi lokitiedostojen-Cmdlet-komentoja ohjatun määritystoiminnon kutsuu liittyvät merkkijonot. Katso [uusille ohjattu määritysprosessi](#first-time-setup-wizard-process) näiden cmdlet-komentojen luettelo. 

7. Jos et voi selvittää ongelman syyn, voit seuraavat vaiheet, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) . Vaiheiden avulla [Luo tukipyynnön](storsimple-contact-microsoft-support.md#create-a-support-request) , kun Microsoft Support pyydä apua.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlet-komennot käytettävissä vianmääritys

Käytä seuraavia Windows PowerShell cmdlet-komennot connectivity virheiden korjaaminen vianmäärityksen avulla.

- `Get-NetAdapter`: Käytä cmdlet esiintyvien verkkoliittymät kunto. 

- `Test-Connection`: Cmdlet avulla tarkistaa verkkoyhteyden sisäiset ja ulkopuoliset verkkoon.

- `Test-HcsmConnection`: Tätä cmdlet-komennon avulla tarkistaa onnistuneesti rekisteröidyn laitteen yhteys.

Jos käytössäsi on päivitys 1 StorSimple laitteen seuraavat diagnostiikan cmdlet-komennot ovat käytettävissä myös.

- `Sync-HcsTime`: Käytä cmdlet laitteen ja pakottaa ajan synkronoinnin NTP-palvelimen kanssa.

- `Enable-HcsPing`ja `Disable-HcsPing`: näiden cmdlet-komentojen avulla voit sallia verkkoliittymät StorSimple laitteen ping isännät. Oletusarvon mukaan StorSimple verkkoliittymät eivät vastaa ping-pyyntöihin.

- `Trace-HcsRoute`: Määritä cmdlet reitin jäljitys-työkalua. Lähettää paketit kunkin reitittimen lopulliseen kohteeseen matkalla ajanjakson aikana ja laskee tulokset kunkin kohteen palautettujen pakettien perusteella. Koska `Trace-HcsRoute` näyttää pakettien määrän annetulle reitittimen tai linkkiä, voit paikantaa mitä reitittimen tai linkit aiheuttavat verkko-ongelmista. 

- `Get-HcsRoutingTable`: Cmdlet avulla paikallinen IP-reititys taulukko näyttää.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Get-NetAdapter cmdlet-komento ja vianmääritys

Kun määrität verkkoliittymät uusille laitteen käyttöönottoa, laitteiston tila ei ole käytettävissä StorSimple hallinta-palvelun Käyttöliittymän, koska laite ei ole vielä rekisteröity-palvelussa. Lisäksi laitteen tila-sivu ei aina oikein ilmaise laitteen tila erityisesti, jos on ongelmia, jotka vaikuttavat synkronoinnin. Näissä tilanteissa, voit käyttää `Get-NetAdapter` cmdlet-komento, määrittämään terveys- ja verkkoliittymät tila.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Voit tarkastella kaikkien verkkosovittimien luettelo laitteessasi

1. Käynnistä Windows PowerShell StorSimple, ja kirjoita sitten `Get-NetAdapter`. 

2. Käytä tulosteen `Get-NetAdapter` cmdlet-komento ja ymmärtää verkon-liittymän tilan seuraavia ohjeita.
  - Käyttöliittymän on kunnossa ja käytössä, jos **ifIndex** -tila näkyy **ylöspäin**.
  - Jos liittymän on kunnossa, mutta se ei ole fyysisesti yhdistetty (tekijä: verkkokaapeleita), **ifIndex** näkyy **ei käytössä**.
  - Jos liittymän on kunnossa, mutta ei käytössä, **ifIndex** -tila näkyy **NotPresent**.
  - Liittymää ei ole, jos se ei näy tässä luettelossa. StorSimple hallintapalvelu Käyttöliittymän näkyy edelleen liittymän epäonnistui-tilaan.

Lisätietoja siitä, miten voit käyttää tätä cmdlet-komento Siirry Windows PowerShellin cmdlet-viittaus- [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) . 

Seuraavissa osissa Näytä tulosteen-objektit `Get-NetAdapter` cmdlet-komento. 

 Näitä esimerkkejä controller 0 oli passiivinen ohjauskoneen, ja se on määritetty seuraavasti:

- TIETOJEN 0, tietojen 1, tieto 2 ja tieto 3 verkon liittymät olivat laitteeseen.
- TIETOJEN 4 ja tietojen 5 verkon käyttöliittymän kortteja ei huomioida; Siksi ne eivät näy tulosteessa.
- TIETOJA 0 on käytössä.

Ohjaimen 1 on aktiivinen ohjauskoneen ja on määritetty seuraavasti:

- TIETOJEN 0, tietojen 1, tieto 2, tietojen 3, 4 tiedot ja tietoja 5 verkon liittymät olivat laitteeseen.
- TIETOJA 0 on käytössä.

**Esimerkki tulosteen – controller 0**

Seuraavassa on tuloste controller 0 (passiivinen ohjauskoneen). TIEDOT-1, tietojen 2 ja tieto 3 ei ole muodostettu. TIETOJEN 4 ja 5 tiedot eivät näy, koska ne eivät ole laitteessa. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Esimerkki tulosteen – ohjauskoneen 1**

Seuraavassa taulukossa on tulos lähettämät 1 (aktiivinen ohjauskoneen). Vain tietojen 0 verkkoliittymän laitteessa on määritetty ja toimimasta.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Vianmääritys ja Testaa yhteys cmdlet-komento

Voit käyttää `Test-Connection` cmdlet-komento, voit selvittää, onko laitteesi StorSimple voit muodostaa yhteyden ulkopuolelta verkkoon. Jos kaikki verkko parametrit, mukaan lukien DNS-on määritetty oikein ohjatun, voit käyttää `Test-Connection` cmdlet-komennolla ping tunnetut osoite verkkoon, kuten Outlook.comin ulkopuolella. 

Ota käyttöön ping cmdlet-yhteysongelmien vianmääritys, jos ping on poistettu käytöstä.

Katso tulosteen seuraavan esimerkin `Test-Connection` cmdlet-komento. 

> [AZURE.NOTE] Ensimmäinen esimerkki laite on määritetty virheelliset DNS. Toinen esimerkki DNS on oikein.
 
**Esimerkki tulosteen – virheellinen DNS**

Seuraavassa esimerkissä ei ole tulosteen IPV4 ja IPV6-osoitteet, mikä tarkoittaa, että DNS ei ratkennut. Tämä tarkoittaa, että ei ilman yhteyttä ulkopuolelta verkkoon oikea DNS on annettava. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Esimerkki tulosteen – oikea DNS**

Seuraavassa esimerkissä DNS palauttaa IPV4-osoite, joka ilmoittaa, että DNS on määritetty oikein. Tämä tarkoittaa, että yhteys ulkopuolelta verkko on. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Vianmääritys ja testaa HcsmConnection cmdlet-komento

Käytä `Test-HcsmConnection` cmdlet-komento, joka on jo liitetty ja StorSimple hallintapalvelu rekisteröityjä laitteen. Tämä cmdlet-komennon avulla voit tarkistaa rekisteröidyn laitteen ja vastaavan StorSimple hallintapalvelu väliset yhteydet. Voit suorittaa komennon Windows PowerShell-StorSimple varten. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Suorita testi HcsmConnection cmdlet-komento

1. Varmista, että laite on rekisteröity.

2. Tarkista laite. Jos laite on poistettu, ylläpito-tilassa tai offline-tilassa, saatat saada seuraavia virheitä: 

   - ErrorCode.CiSDeviceDecommissioned – Tämä ilmaisee, että laite on poistettu käytöstä.
   - ErrorCode.DeviceNotReady – Tämä ilmaisee, että laite on ylläpito-tilassa.
   - ErrorCode.DeviceNotReady – Tämä ilmaisee, että laite ei ole online-tilassa.

3. Varmista, että StorSimple hallintapalvelu on käynnissä (käytä [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet-komento). Jos palvelu ei ole käynnissä, näyttöön voi tulla seuraavia virheitä:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – Tämä ilmaisee, että poikkeus suorittaessasi Get-ClusterResource.

4. Tarkista Access ohjausobjektin Service (ACS)-tunnuksen. Jos se ilmoittaa web-poikkeus, se voi olla tuloksen yhdyskäytävän ongelma, puuttuu välityspalvelimen todennus, virheelliset DNS tai käyttöoikeuksien tarkistusvirhe. Saatat saada seuraavia virheitä:

   - ErrorCode.CiSApplianceGateway – Tämä ilmaisee HttpStatusCode.BadGateway poikkeus: nimi tulkintatoiminnon palvelua ei voitu selvittää isäntänimi. 
   - ErrorCode.CiSApplianceProxy – Tämä ilmaisee HttpStatusCode.ProxyAuthenticationRequired poikkeuksen (HTTP-tilakoodin 407): asiakas ei voinut todentaa välityspalvelimeen. 
   - ErrorCode.CiSApplianceDNSError – Tämä ilmaisee WebExceptionStatus.NameResolutionFailure poikkeus: nimi tulkintatoiminnon palvelua ei voitu selvittää isäntänimi.
   - ErrorCode.CiSApplianceACSError – Tämä ilmaisee, että palvelu palautettu Todennusvirhe, mutta on yhteys.
   
    Jos se ei ole palauttaa web-poikkeusten, tarkista ErrorCode.CiSApplianceFailure. Tämä ilmaisee, että laitteen epäonnistui.

5. Tarkista cloud palvelun yhteydet. Jos palvelun ilmoittaa web-poikkeus, saatat saada seuraavia virheitä:

  - ErrorCode.CiSApplianceGateway – Tämä ilmaisee HttpStatusCode.BadGateway poikkeus: keskitason välityspalvelimen vastaanotettu pyyntö ei kelpaa toisen välityspalvelimen tai alkuperäisen palvelimen.
  - ErrorCode.CiSApplianceProxy – Tämä ilmaisee HttpStatusCode.ProxyAuthenticationRequired poikkeuksen (HTTP-tilakoodin 407): asiakas ei voinut todentaa välityspalvelimeen. 
  - ErrorCode.CiSApplianceDNSError – Tämä ilmaisee WebExceptionStatus.NameResolutionFailure poikkeus: nimi tulkintatoiminnon palvelua ei voitu selvittää isäntänimi.
  - ErrorCode.CiSApplianceACSError – Tämä ilmaisee, että palvelu palautettu Todennusvirhe, mutta on yhteys.
  
    Jos se ei ole palauttaa web-poikkeusten, tarkista ErrorCode.CiSApplianceSaasServiceError. Tämä ilmaisee StorSimple hallintapalvelu ongelma.
 
6. Tarkista Azure palvelun Bus yhteys. ErrorCode.CiSApplianceServiceBusError ilmaisee, että laite ei voi muodostaa yhteyden palvelun Bus.
 
Lokitiedostot CiSCommandletLog0Curr.errlog ja CiSAgentsvc0Curr.errlog on lisätietoja, kuten poikkeuksen tiedot. 

Lisätietoja siitä, miten cmdlet-komennon, siirry Windows PowerShell- [Testi HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) oppaiden.

> [AZURE.IMPORTANT] Voit suorittaa aktiivisen ja passiivisen ohjauskoneen cmdlet. 
 
Katso tulosteen seuraavan esimerkin `Test-HcsmConnection` cmdlet-komento. 

**Esimerkki tulosteen – onnistuneesti rekisteröityjen laitteessa StorSimple Release (heinäkuussa 2014)**

Ensimmäinen malli on laitteissa, joissa on rekisteröitynyt StorSimple hallintapalvelu ja ei ole yhteysongelmat. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Esimerkki tulosteen – onnistuneesti rekisteröityjen laitteessa StorSimple päivitys 1**

Jos käytössäsi on päivitys 1 StorSimple laitteen sinun ei tarvitse Suorita yksityiskohtainen valitsimella.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Esimerkki tulosteen – offline-tilassa laitteessa StorSimple Release (heinäkuussa 2014)**

Tässä esimerkissä on laite, jonka tila on **offline-tilassa** Azure perinteinen-portaalissa.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Laite ei voi muodostaa yhteyden nykyiset web-välityspalvelimen asetukset. Tämä voi johtua ongelmasta web-välityspalvelimen määritykset tai verkon yhteysongelma. Tässä tapauksessa kannattaa varmistat, että web välityspalvelimen asetukset ovat oikein ja web-välityspalvelimet ovat online-tilassa ja tavoitettavissa. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Vianmääritys ja synkronoi HcsTime cmdlet-komento

Tämä cmdlet-komennon avulla voit näyttää laitteen aika. Jos laitteen aikaa on NTP-palvelimen kanssa siirtymä, valitse voit cmdlet voimassa-ajan synkronoiminen NTP-palvelimen kanssa. Jos välisen laitteen ja NTP-palvelin on suurempi kuin 5 minuuttia, näyttöön tulee varoitus. Jos siirtymä yli 15 minuuttia, laite siirtyy offline-tilassa. Voit silti cmdlet Pakota aika Synkronoi. Kuitenkin Jos siirtymä ylittää 15 tuntia, valitse ei osaat voimassa synkronointiin ajankohta ja virhesanoma näytetään.

**Esimerkki tulosteen – pakotettu ajan synkronointi avulla synkronointi HcsTime**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Vianmääritys ja ota käyttöön HcsPing ja poista käytöstä HcsPing cmdlet-komennot

Näiden cmdlet-komentojen avulla voit varmistaa, että laitteen verkkoliittymät vastata ICMP ping-pyynnöt. Oletusarvoisesti StorSimple verkkoliittymät eivät vastaa ping-pyyntöihin. Käyttämällä tätä cmdlet-komento on helpoin tapa oppia tuntemaan Jos laitteesi ovat verkkoyhteydessä ja tavoitettavissa.  

**Esimerkki tulosteen – Ota käyttöön HcsPing ja poista käytöstä HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Jäljitys-HcsRoute cmdlet-komento ja vianmääritys

Käytä tätä cmdlet-komento reitin jäljitys-työkalua. Lähettää paketit kunkin reitittimen lopulliseen kohteeseen matkalla ajanjakson aikana ja laskee tulokset kunkin kohteen palautettujen pakettien perusteella. Koska cmdlet-komento näkyy pakettien menetys minkä tahansa määritetyn reitittimen tai linkin aste, voit paikantaa mitä reitittimen tai linkit aiheuttavat verkko-ongelmista.

**Esimerkki tulosteen esittää, kuinka paketti, jonka jäljitys HcsRoute reitin jäljitys**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Get-HcsRoutingTable cmdlet-komento ja vianmääritys

Tämä cmdlet-komennon avulla voit tarkastella reititys taulukon StorSimple laitteeseesi. Reititys taulukko on joukko sääntöjä, jotka määrittävät, jossa tiedot pakettien matkustaminen Internet Protocol (IP)-verkossa ohjataan. 

Reititys taulukossa liittymät ja jotka ohjaavat tietojen määritetyn verkkoihin yhdyskäytävän. Voit myös aloittaa reititys arvo, joka on otettava saavuttaa tietyn kohteen polun päätös-maker. Alaosassa reititys metrijärjestelmä, mitä suurempi haluamasi asetuksen. 

Esimerkiksi jos sinulla on 2 verkkoliittymät, tietojen 2 ja 3 tietojen yhteydessä Internetiin. Jos tietojen 2 ja 3 tietojen reititys mittaukset on 15 ja 261, pienet reititys metrijärjestelmä tietojen 2 on ensisijainen käyttöliittymä käyttää muodostaa yhteys Internetiin.

Jos käytössäsi on päivitys 1 StorSimple laitteen tiedot 0 verkko-liittymän on suurin tilaksi cloud-liikenne. Tämä tarkoittaa sitä, vaikka määritettynä on muita cloud käyttävä liittymät, cloud liikenne reitittää tietojen 0. 

Jos suoritat `Get-HcsRoutingTable` cmdlet-komennon määrittämättä parametreja (kuten seuraavassa esimerkissä) cmdlet tulosteen IPv4- ja IPv6 reititys taulukot. Vaihtoehtoisesti voit määrittää `Get-HcsRoutingTable -IPv4` tai `Get-HcsRoutingTable -IPv6` saat haluamasi reititys taulukkoon.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Vaiheittaiset ohjeet StorSimple vianmäärityksen Esimerkki

Seuraavassa esimerkissä StorSimple käyttöönoton vaiheittaiset vianmääritys. Esimerkki skenaariossa laitteen rekisteröinti epäonnistuu virhe, joka ilmaisee, että verkkoasetukset tai DNS-nimi on virheellinen.

Funktio palauttaa virhesanoma on seuraavanlainen:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Tämä virhe voi johtua seuraavista:

- Virheellinen laitteisto asennus
- Viallinen verkon saada
- Virheellinen IP-osoite, aliverkkopeite, yhdyskäytävän, ensisijainen DNS-palvelimeen tai web-välityspalvelin
- Virheellinen rekisteröinnin avain
- Virheellinen palomuuriasetukset

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Voit etsiä ja korjata laitteen rekisteröinti

1. Tarkista laite-määritykset: aktiivinen ohjaimen suorittaa `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Ohjatun määritystoiminnon on suoritettava aktiivinen ohjaimen. Voit varmistaa, että olet muodostanut yhteyden aktiivisen ohjauskoneen, tarkista esitetään serial konsolissa ilmoituspalkki. Nauhan ilmaisee, onko yhteys on muodostettu controller 0 tai 1-ohjain ja onko ohjaimen aktiivisen tai passiivisen. Saat lisätietoja Siirry [Määritä aktiivinen ohjauskoneen, laitteessasi](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Varmista, että laite on kytketty oikein: Tarkista kaapeli laitteeseen takaisin tasossa verkkoon. Kaapeli on laitteen mallista. Saat lisätietoja Siirry [asentaminen StorSimple 8100 laitteen](storsimple-8100-hardware-installation.md) ja [asentaa StorSimple 8600 laitteen](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Jos käytössäsi on 10 GbE verkkoportit, sinun on annettu QSFP SFP sovittimet ja SFP kaapeli. Lisätietoja on artikkelissa [kaapeli, valitsimista ja enintään suosittelemalla Mellanox porteille OEM-toimittaja](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Varmista verkon-liittymän kunto:

   - Tunnistaa verkkoliittymät kunto tietojen 0 Get-NetAdapter cmdlet-komennon avulla. 
   - Jos linkki ei toimi, **ifindex** tila osoittavat, että liittymää ei ole käytettävissä. Sinun on verkkoyhteys portin laitteen ja vaihda tarkistamaan. Sinun on myös virheelliset kaapeli pois. 
   - Jos epäilet, että tiedot 0 porttiin aktiivinen valvonta on epäonnistunut, voit vahvistaa muodostamalla yhteyden tiedot 0 portin ohjaimen 1. Voit vahvistaa tämän verkkokaapelin katkaista laitteen taustalle lähettämät 0, Liitä kaapeli ohjauskoneen 1 ja suorittamalla Get-NetAdapter cmdlet-komennon uudelleen. 
   Jos tiedot 0 porttiin valvonta on epäonnistuu, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) vaiheisiin. Voit joutua muuttamaan korvaa järjestelmän valvonta.
 
4. Tarkista yhteys valitsin:
   - Varmista, että tietojen 0 verkkoliittymät controller 0 ja ensisijainen kehyksen ohjauskoneen 1 on saman aliverkon. 
   - Tarkista keskittimeen tai reitittimeen. Molemmat ohjaimet yhdistäminen on yleensä samaan keskittimeen tai reitittimeen. 
   - Varmista, että yhteys käytetään valitsimet on sama vLAN molemmat ohjaimet tietojen 0.
   
5. Poista käyttäjä-virheitä:

  - Suorita ohjattu määritystoiminto uudelleen (Suorita **Käynnistä HcsSetupWizard**) ja kirjoita haluamasi arvot uudelleen ja varmista, että on virheitä. 
  - Tarkista rekisteröinnin avainta. Saman rekisteröinnin avaimen avulla voidaan muodostaa StorSimple hallintapalvelu useilla eri laitteilla. - [Palvelun rekisteröinti Key-tunnuksen hankkiminen](storsimple-manage-service.md#get-the-service-registration-key) menettelyn avulla voit varmistaa, että käytät oikeaa rekisteröinti-näppäintä.

    > [AZURE.IMPORTANT] Jos sinulla on useita palveluita, sinun on varmistettava, että tarvittavat palvelun rekisteröinti näppäintä käytetään Rekisteröi laite. Jos laite on rekisteröity väärä StorSimple hallinta-palvelun kanssa, sinun on seuraavat vaiheet, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) . Saatat joutua suoritetaan factory-Palauta laitteen (joka voi aiheuttaa tietojen menettämisen) ja valitse Yhdistä se tarkoitettu-palveluun.

6. Testaa yhteys cmdlet-komennon avulla voit varmistaa, että sinulla on yhteys ulkopuolelta verkkoon. Jos haluat lisätietoja, siirry [ja Testaa yhteys cmdlet-komennon vianmääritys](#troubleshoot-with-the-test-connection-cmdlet).

7. Tarkista palomuurin häiriöitä. Jos olet varmistanut, virtual IP (VIP), aliverkon yhdyskäytävän tai DNS-asetukset ovat kaikki oikein, ja näet edelleen yhteysongelmat ja valitse on mahdollista, että palomuuri estää laitteen ja ulkopuolelta verkon välisen. Haluat varmistaa, että portit 80 ja 443 ovat käytettävissä StorSimple laitteessasi, n lähtevän tietoliikenteen. Lisätietoja on artikkelissa [Verkko StorSimple laitteen koskevat vaatimukset](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Tarkista lokit. Siirry [tuki-paketit ja laitteen lokit käytettävissä vianmääritystä varten](#support-packages-and-device-logs-available-for-troubleshooting).

9. Jos edellä kuvatut toimet eivät ratkaise ongelmaa, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) järjestelmänvalvojalta.

## <a name="next-steps"></a>Seuraavat vaiheet
[Opi suorittamaan toiminnallisia laite](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 

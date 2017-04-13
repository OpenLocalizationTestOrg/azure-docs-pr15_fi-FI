<properties
    pageTitle="404 tila, joka palauttaa Azure CDN päätepisteet vianmääritys | Microsoft Azure"
    description="Vianmääritys 404 vastauksen koodit ja Azure CDN päätepisteet."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Vianmääritys CDN päätepisteet palauttaminen 404 tilat

Tämän artikkelin avulla voit vianmääritys [CDN päätepisteet](cdn-create-new-endpoint.md) palauttaminen 404 virheitä.

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN-Azure](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto-keskustelupalstoilla. Voit vaihtoehtoisesti myös jättää Azure tuki tapahtuma. Siirry [Azure tukisivustossa](https://azure.microsoft.com/support/options/) ja valitse sitten **Hae tuki**.

## <a name="symptom"></a>Oire

Olet luonut CDN profiili ja päätepisteen, mutta sisältö ei vaikuta olla käytettävissä CDN.  Käyttäjät, jotka yrittävät tarkastella sisältöä CDN URL-Osoitteen kautta saat HTTP 404 tilakoodin. 

## <a name="cause"></a>Syy

On useita mahdollisia syitä, mukaan lukien:

- Tiedoston alkuperän ei näy CDN
- Päätepisteen on määritetty virheellisesti, aiheuttaa CDN tarkistettava väärään paikkaan
- Isännän hylkää-CDN toimialuenimi
- Päätepisteen ei ole ollut leviäminen CDN koko ajan

## <a name="troubleshooting-steps"></a>Vianmääritysohjeita

> [AZURE.IMPORTANT] Kun olet luonut CDN päätepisteen, se heti eivät käytettäväksi, kuin kuluvaa aikaa rekisteröinnin kautta CDN leviäminen.  <b>Azure-Akamai CDN</b> profiilien välittäminen suorittaa yleensä yhden minuutin kuluessa.  <b>Azure CDN Verizon-</b> profiileista välittäminen yleensä kestää 90 minuutin kuluessa, mutta joissakin tapauksissa saattaa toimia hitaasti.  Jos vaiheiden tässä asiakirjassa ja näyttöön tulee silti 404 vastaukset, voit odottaa muutaman tunnin tarkistamaan uudelleen, ennen kuin avaat tuki lippu.

### <a name="check-the-origin-file"></a>Tarkista origin-tiedosto

Tarkista ensin, että olisi, joka haluamme välimuistissa oleva tiedosto on käytettävissä Microsoftin origin ja on kaikkien käytettävissä.  Nopein tapa tehdä on avata selaimessa olevan yksityisen tai Incognito-istunnon ja tiedosto selaamalla.  Vain Kirjoita tai liitä URL-osoite osoite-kenttään ja tarkistaa, aiheuttaako, odotat tiedostossa.  Tässä esimerkissä valitsen minulla on Azure-tallennustilan tilin käytettävissä osoitteessa tiedoston `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Kuten näet, se kulkee onnistuneesti testi.

![Success!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Kun tämä on nopein ja helpoin tapa vahvistamiseksi tiedostossa on julkisesti saatavilla, organisaation verkon jotkin määritykset voi antaa sinulle vaikutelma, että tiedosto on julkisesti saatavilla, kun se on itse asiassa vain näkyvät käyttäjille verkoston (vaikka sitä isännöidään Azure-tietokannassa).  Jos sinulla ulkoisia selaimessa, joista voit testata, kuten mobiililaitteella, joka ei ole yhteydessä organisaation verkkoon tai virtual konetta Azure-tietokannassa, joka olisi paras.

### <a name="check-the-origin-settings"></a>Origin asetusten tarkistaminen

Nyt, että olet tarkistanut tiedosto on yleisesti saatavilla Internetissä, emme Tarkista Microsoftin origin asetukset.  [Azure-portaalin](https://portal.azure.com)CDN profiiliisi Selaa ja valitse olet vianmääritys päätepiste.  Valitse alkuperän tuloksena **päätepisteen** -sivu.  

![Päätepisteen sivu kanssa origin korostettuna](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Origin** -sivu tulee näkyviin. 

![Origin sivu](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Origin tyyppi ja isäntänimi

Tarkista **Origin tyyppi** on oikea ja varmista, että **Origin isäntänimi**.  Oma esimerkissä `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, URL-Osoitteen hostname (isäntänimi)-osa on `cdndocdemo.blob.core.windows.net`.  Kuten näet näyttökuvan, tämä on oikein.  Azure-tallennustilan, Web App ja pilvipalvelussa alkuperän **Origin isäntänimi** -kentän on avattavasta-, joten ei tarvitse olla huolissaan siitä, että oikeinkirjoitus oikein.  Jos käytät mukautettua origin, on *ehdottoman tärkeitä* oman isäntänimi on kirjoitettu oikein!

#### <a name="http-and-https-ports"></a>HTTP- ja HTTPS-portit

Voit tarkistaa tätä kohdetta on **HTTP** ja **HTTPS-portit**.  Useimmissa tapauksissa 80 ja 443 ovat oikein ja tarvitset muutoksia.  Kuitenkin jos alkuperäisestä palvelimesta seuraa eri porttiin, joka on esitettäväksi tähän.  Jos et ole varma, valitsemisen origin tiedoston URL-osoitteessa.  HTTP- ja HTTPS-määritykset Määritä portit 80 ja 443 oletusarvoiksi. Kirjoita URL-osoite `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`-portin ei määritetä, jotta 443 oletusarvon oletetaan ja Omat asetukset ovat oikein.  

Yammer kuitenkin URL-osoite on peräisin tiedosto, joka testataan aiemmin `http://www.contoso.com:8080/file.txt`.  Huomautus `:8080` hostname segmentin lopussa.  Tämä kertoo selaimen käyttävät porttia `8080` muodostaa WWW-palvelimessa `www.contoso.com`, joten sinun on annettava 8080 **HTTP Port (portti)** -kenttään.  On tärkeää muistaa, että nämä porttiasetukset vaikuttavat vain mitä portin päätepisteen käyttää voit hakea tietoja alkuperän.

> [AZURE.NOTE] **Azure-Akamai CDN** päätepisteet eivät salli alkuperistä koko TCP-porttialue.  Origin portit, jotka eivät ole sallittuja luettelo on artikkelissa [Azure CDN-Akamai sallitut Origin portit](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Päätepisteen asetusten tarkistaminen

Takaisin **päätepisteen** -sivu, valitse **Määritä** -painiketta.

![Päätepisteen sivu määrittäminen-painike korostettuna](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Päätepisteen **määrittäminen** -sivu tulee näkyviin.

![Määritä sivu](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokollat

**Protokollat**Varmista, että käytössä asiakkaat protokolla on valittuna.  Sama protokolla, jota käytetään asiakkaan on yksi käyttää alkuperän, joten on tärkeää on määritetty oikein edellisessä osassa origin portit.  Päätepisteen seuraa vain oletusarvon HTTP- ja HTTPS portit (80 ja 443) origin portit riippumatta.

Palaa japanin hypoteettista esimerkissä kanssa `http://www.contoso.com:8080/file.txt`.  Jonka muistat, kuten Contoso määritetty `8080` kuin niiden HTTP portti, mutta myös oletetaan ne määritetty `44300` niiden HTTPS-porttina.  Jos luomiaan päätepisteen nimeltä `contoso`, niiden CDN päätepisteen hostname olisi `contoso.azureedge.net`.  Pyyntö `http://contoso.azureedge.net/file.txt` on HTTP-pyyntö, joten päätepisteen käyttää HTTP porttiin 8080 noutamiseen alkuperän.  Suojattu pyyntö päälle HTTPS- `https://contoso.azureedge.net/file.txt`, aiheuttaa päätepisteen HTTPS käyttämisestä portin 44300 kun retriving tiedoston alkuperän.

#### <a name="origin-host-header"></a>Origin toimialuenimi

**Origin toimialuenimi** on lähetetty origin sivupyynnön kanssa host otsikon arvo.  Useimmissa tapauksissa tämä on oltava sama kuin **Origin isäntänimi** on tarkistettu aiemmin.  Virheellinen arvo tähän kenttään ei yleensä aiheuttaa 404 tilat, mutta se on todennäköisesti 4xx muut tilat sen mukaan, mitä odottaa alkuperän.

#### <a name="origin-path"></a>Origin polku

Lopuksi on Tarkista Microsoftin **Origin polku**.  Oletusarvoisesti tämä on tyhjä.  Käyttää kentän vain, jos haluat tarkentaa haluat käyttöön CDN origin isännöimä resursseja.  

Esimerkiksi Omat päätepisteen-voin määrittää kaikkien resurssien tilini tallennustila on käytettävissä, jotta voin jättää **Origin polku** tyhjäksi.  Tämä tarkoittaa, että pyyntö `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` tuloksena on oma päätepiste yhteyden `cdndocdemo.core.windows.net` , joka pyytää `/publicblob/lorem.txt`.  Vastaavasti pyynnön `https://cdndocdemo.azureedge.net/donotcache/status.png` johtaa päätepisteen pyytää `/donotcache/status.png` kohteen alkuperäisestä.

Mutta entä jos en halua käyttää Omat origin jokaisen polku CDN?  Sano I haluat näyttää vain `publicblob` polku.  Jos */publicblob* Syötä **Origin polku** -kentän, joka aiheuttaa päätepisteen Lisää */publicblob* jokaisen pyynnön alkuperän tehdään ennen.  Tämä tarkoittaa, että pyyntö `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nyt itse asiassa kestää pyyntö URL-Osoitteen osa `/publicblob/lorem.txt`, ja liitä `/publicblob` alkuun. Tuloksena on pyynnön `/publicblob/publicblob/lorem.txt` kohteen alkuperäisestä.  Jos polku ei vastaa todellinen tiedostoon, alkuperän Palauta 404 tila.  Blogisivun URL-Osoitteen noutaminen lorem.txt tässä esimerkissä olisi `https://cdndocdemo.azureedge.net/lorem.txt`.  Huomaa, että Microsoft ei polku */publicblob* lainkaan, koska pyyntö URL-osoitteen osa on `/lorem.txt` ja päätepisteen Lisää `/publicblob`, tuloksena `/publicblob/lorem.txt` pyynnön siirretään alkuperän.

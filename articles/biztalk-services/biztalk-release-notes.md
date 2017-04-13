<properties
    pageTitle="Julkaisutiedot Azure BizTalk palveluiden | Microsoft Azure BizTalk palvelut"
    description="Luettelo tunnetuista ongelmista Azure BizTalk palveluille" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Azure BizTalk Services julkaisutiedot

Microsoft Azure BizTalk Services julkaisutiedot sisältävät tämän version tunnetut ongelmat.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Mitä uusia ominaisuuksia BizTalk palvelujen marraskuun-päivitys
* Salauksen Rest-palvelussa voi olla käytössä BizTalk palvelut-portaalissa. Katso [käyttöön muiden BizTalk palvelut-portaalissa osoitteessa salausta](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Päivityshistoria

### <a name="october-update"></a>Lokakuussa päivitys

* Organisaation tili tuetaan:  
 * **Skenaario**: rekisteröity BizTalk palvelun käyttöönoton Microsoft-tilillä (kuten user@live.com). Tässä skenaariossa vain Microsoft Account käyttäjät voivat hallita BizTalk palvelun BizTalk palvelut-portaalissa. Organisaation tiliä ei voi käyttää.  
 * **Skenaario**: rekisteröity BizTalk palvelun käyttöönoton organisaatiotiliä käyttämällä Azure Active Directory (kuten user@fabrikam.com tai user@contoso.com). Tässä skenaariossa vain samassa organisaatiossa Azure Active Directory-käyttäjiä voit hallita BizTalk palvelua BizTalk palvelut-portaalissa. Microsoft-tiliä ei voi käyttää.  
* Kun luot BizTalk palvelu Azure perinteinen-portaalissa, olet rekisteröitynyt automaattisesti BizTalk palvelut-portaalissa.
 * **Skenaario**: kirjautua Azure perinteinen-portaaliin, Luo BizTalk palvelu ja valitse sitten **Hallitse** hyvin ensimmäisen kerran. Kun BizTalk-palveluiden portaali avautuu, BizTalk-palvelun Rekisteröi ja on valmiina käyttöönoton automaattisesti.  
 Katso [rekisteröiminen ja päivittäminen BizTalk palvelun käyttöönoton BizTalk palvelut-portaalissa](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Elokuussa 14 päivitys
* Sopimuksen ja silta irtautus – kumppanin sopimusten ja siltoja nyt erillisen BizTalk palvelut-portaalissa. Voit nyt luoda sopimusten ja siltoja erikseen ja suorituksen siltoja ratkaista sopimus Muokkaa viestin arvojen perusteella. Lue [Luominen sopimusten Azure BizTalk Services-palveluissa](https://msdn.microsoft.com/library/azure/hh689908.aspx), [luoda BizTalk palvelut-portaalissa Muokkaa-sillan](https://msdn.microsoft.com/library/azure/dn793986.aspx), [Luo BizTalk palvelut-portaalissa AS2-sillan](https://msdn.microsoft.com/library/azure/dn793993.aspx), ja [miten siltoja ratkaista sopimusten suorituksen?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Voit luoda malleja toimeenpano on lopetettu.  
* Lähetä Include sopimuksen voit määrittää kunkin rakenteen määrittää eri erottimen. Valitse Lähetä sivu sopimuksen protokolla-asetusten tämän määrityksen. Lisätietoja on artikkelissa [Create an X12 sopimuksen Azure BizTalk Services-palveluissa](https://msdn.microsoft.com/library/azure/hh689847.aspx) ja [Luo EDIFACT sopimuksia Azure BizTalk Services-palveluissa](https://msdn.microsoft.com/library/azure/dn606267.aspx). Kaksi uudet kohteet lisätään myös TPM OM Ohjelmointirajapinnan samaan tarkoitukseen. Katso [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) ja [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Vakio XSD-rakenteita, mukaan lukien johdetun eri tukee nyt. Katso [Käytä vakio XSD muodostaa yhteyttä karttoja](https://msdn.microsoft.com/library/azure/dn793987.aspx) ja [Käytä johdetun tyypit yhdistäminen skenaariot ja esimerkkejä](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 tukee uuden viestin allekirjoittamiseen Mikrofoni algoritmit ja uusi salausalgoritmeista. Katso [luoda AS2 sopimuksia Azure BizTalk Services-palveluissa](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Tunnetut ongelmat

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Yhteysongelmat BizTalk Services portaalin päivityksen jälkeen

  Jos Avaa, kun BizTalk-palvelut on päivitetty kokoa muuttuu palvelun BizTalk palveluiden portaali on ehkä yleisölle tarkoitetun yhteysongelmat BizTalk palvelut-portaalissa.  
  Voit kiertää tämän ongelman voi Käynnistä selain uudelleen, Poista selaimen välimuisti tai Käynnistä portaalin yksityinen-tilassa.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE ei löydy Palvelutietojen, jos valitset virhe tai varoitus BizTalk Services projektin
Asenna Visual Studio 2012 päivityksen 3 RC 1 Voit korjata ongelman.  

### <a name="custom-binding-project-reference"></a>Mukautettu sidonta projektiviittaus
Harkitse BizTalk Services projektin Visual Studio ratkaisussa seuraavissa tilanteissa:  
* Visual Studio samalla ratkaisussa on BizTalk Services-projekti ja mukautettu sidonta projektin. BizTalk palvelun projektissa on mukautettu sidonta tämän projektitiedoston viittaus.
* BizTalk palvelun projektissa on mukautettu sidonta/toiminnan DLL viittaus.

'Luodaan-ratkaisun käyttöön Visual Studiossa onnistuneesti. Sitten uudelleen' tai Puhdista-ratkaisun. Sen jälkeen kun uudelleen tai puhdistaa uudelleen seuraava virhesanoma tulee näyttöön:  
  Ei voi kopioida tiedoston <Path to DLL> , "bin\Debug\FileName.dll". Prosessia ei voi käyttää tiedostoa 'bin\Debug\FileName.dll', koska se on toisen prosessin käytössä.  

#### <a name="workaround"></a>Vaihtoehtoinen menetelmä
* Jos [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) on asennettu, sinulla on jompikumpi seuraavista toimista:

  * Käynnistä Visual Studiossa, tai

  * Käynnistä ratkaisu. Suorita sitten vain muodosta ratkaisun.  

* Jos [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) ei ole asennettu, Avaa Tehtävienhallinta, valitse prosessit-välilehti, MSBuild.exe prosessi valitsemalla ja valitse sitten Lopeta prosessi-painiketta.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>BasicHttpRelay päätepisteet reitittämisestä ei tueta siltoja ja BizTalk palveluiden portaali Jos ei voi tulostaa merkkien ylennetyt HTTP otsikoina

Jos käytät ei voi tulostaa merkkien ylemmän tason ominaisuudet osana viestien, välitys kohteisiin, jotka käyttävät BasicHttpRelay sidonta, sitä voi reitittää viestit. Ylemmän tason ominaisuudet, jotka ovat myös käytettävissä kuin seuranta osa BLOB URL-koodatun ja poistamalla koodattu kohteisiin.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN lähetetään asynkronisesti, vaikka Lähetä asynkroninen MDN-vaihtoehto ei ole valittu  
Harkitse Tämä skenaario – Jos olet **Lähetä asynkroninen MDN** -valintaruutu ja määritä URL-Osoitteen lähettäminen asynkroninen MDN vastaanottaja ja poista **Lähetä asynkroninen MDN** -valintaruutu uudelleen MDN edelleen lähetetään määritetyn URL-Osoitteen lähettäminen asynkroninen MDNs ei ole valittuna mutta.  
Voit kiertää tämän ongelman poistamalla määritetyssä URL-osoitteessa ennen valinnan **lähettää asynkroninen MDN** -valintaruutu ja käyttöönotto AS2 sopimuksen.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Lisäksi kelvollinen interchange välilyöntejä tulla keskeytys päätepisteen lähettämisen tyhjä sanoma  
Jos luettelossa on lisäksi IEA-osiossa välilyönnit, disassembler tulkitsee nykyisen interchange loppuun ja tarkastelee Seuraava joukko ylätunnisteiden seuraavan viestin. Tämä ei ole kelvollinen Interchange Format-saattaa huomata, että yhden onnistuneen viesti lähetetään reitin kohteeseen ja yksi tyhjä viesti on lähetetty keskeytys päätepiste.  
### <a name="tracking-in-biztalk-services-portal"></a>Seurannan BizTalk palvelut-portaalissa  
Tapahtumien seuranta välitetyt Muokkaa viestin käsittely- ja minkä tahansa korrelaatio ylöspäin. Jos viestin epäonnistuu protokolla-vaiheen ulkopuolella, seuranta näkyy vain onnistu. Tässä tilanteessa viitata **yhteenvetosarakkeen **Seuranta** -virheiden tiedot** LOG-osiossa.
X12 vastaanottamista ja lähettämistä asetukset ([Create an X12 sopimuksen Azure BizTalk Servicesissä](https://msdn.microsoft.com/library/azure/hh689847.aspx)) Anna protokolla vaiheen tiedot.  

### <a name="update-agreement"></a>Päivitä sopimus  
Voit muokata jäsenyyden valitsin, kun sopimus on määritetty BizTalk palveluiden portaali. Tällöin on epäyhtenäinen ominaisuudet. On esimerkiksi ZZ:1234567 ja ZZ:7654321 valitsinta sopimus. BizTalk palveluiden portaali profiilin-asetusten avulla voit muuttaa ZZ:1234567 on 01:ChangedValue. Voit avata sopimuksen ja 01:ChangedValue näkyy ZZ:1234567 sijaan.
Voit muokata jäsenyyden valitsin poistamalla sopimuksen, Päivitä **Käyttäjätietojen** kumppanin profiilin ja luomalla sopimuksen.  
> AZURE. Tämä ongelma varoitus vaikuttaa X12 ja AS2.  

### <a name="as2-attachments"></a>AS2 liitteet  
Liitteet-viestejä ei tue AS2 lähettää tai vastaanottaa. Tarkemmin sanottuna liitteet äänettömästi ohitetaan ja viestin tekstin käsitellään säännöllisesti AS2 viestinä.  
### <a name="resources-remembering-path"></a>Resurssit: Muistaminen polku  
Kun lisäät **resursseja**, valintaikkuna saattaa muista aiemmin käyttänyt Lisää Resurssin polku. Muistaa aiemmin käyttänyt-polku, yritä BizTalk palveluiden portaali web-sivuston lisääminen **Luotetut sivustot** Internet Explorerissa.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Jos kohteen nimeä sillan ja sulkea projektin tallentamatta muutoksia, kohteen avataan uudelleen aiheuttaa virheen
Harkitse tilanne seuraavassa järjestyksessä:  
* Lisää sillan (esimerkiksi XML One-Way silta) BizTalk-palvelun lisääminen projektiin  

* Anna siltaan määrittämällä kohteen nimi-ominaisuuden arvon. Nimeää uudelleen liittyvän .bridgeconfig tiedoston määrittämäsi nimi.  

* Sulje .bcs-tiedosto (sulkemalla Visual Studio ‑välilehden) tallentamatta muutoksia.  

* Avaa .bcs tiedoston uudelleen ratkaisunhallinnassa.  
Huomaat, että liittyvän .bridgeconfig-tiedostoa ei määrittämäsi uuden nimen, kohteen suunnittelualueella on edelleen vanha nimi. Jos yrität avata sillan määrityksiä kaksoisnapsauttamalla silta-osan, näyttöön tulee seuraava virhe:  
  "<old name>"Kohde on liitetty tiedosto"<old name>.bridgeconfig" ei ole  
Voit välttää tämän skenaarion siten, varmista, että tallennat muutokset, kun olet nimennyt BizTalk palvelun projektin kohteita.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk palvelun project muodostaa onnistuneesti, vaikka Palvelutietojen on jätetty pois Visual Studio-projekti
Harkitse tilanne, jossa voit lisätä Palvelutietojen (esimerkiksi XSD-tiedosto) BizTalk palvelun projektiin, kyseisen Palvelutietojen sisällyttäminen sillan määritys (esimerkiksi määrittämällä sen pyynnön viestin tyypin) ja pois Visual Studio-projekti. Siinä tapauksessa muodostaminen projektin ei avulla jokin virhe, kunhan poistetut Palvelutietojen on samassa paikassa-kohtaa, johon se on sisältyy Visual Studio-projektin levyllä.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>BizTalk palvelun projektin ei tarkista rakenteen käytettävyys siltoja määritettäessä
BizTalk palvelun Projectissa, jos rakenteen, joka on lisätty projektiin Tuo toisesta rakenteen BizTalk palvelun projektin ei tarkista onko tuodun rakenteen on lisätty projektiin. Jos yrität luoda esimerkiksi projektin, saat ei muodosta virheitä.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>XML-vastaus pyyntö sillan vastausviesti on aina merkistö UTF-8
Tässä versiossa XML pyyntö vastaus siltauksesta vastausviesti merkistö on aina määritetty UTF-8.
### <a name="user-defined-datatypes"></a>Käyttäjän määrittämät tietotyypit
BizTalk sovittimen Pack sovittimet BizTalk sovittimen Service-ominaisuuden sisällä voit hyödyntää käyttäjän määrittämät tietotyypit sovittimen toimintoja varten.
Käyttäjän määrittämät tietotyypit käytettäessä kopioida tiedostoja (.dll) asema: \Program BizTalk sovittimen Service\BAServiceRuntime\bin\ tai voit yleiseen kokoonpanon välimuistin (GAC) palvelimessa, jossa BizTalk sovittimen-palvelun. Muussa tapauksessa seuraava virhe voi ilmetä asiakkaan:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] On suositeltavaa asentaminen tiedoston yleinen kokoonpanon välimuistiin GACUtil.exe avulla. GACUtil.exe asiakirjat tämä työkalu ja Visual Studio-asetukset.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>BizTalk sovittimen Service-ohjelman Web-sivuston käynnistäminen uudelleen
Asentaminen **BizTalk sovittimen palvelun suorituksenaikainen** * Luo * *BizTalk sovittimen palvelun* * web-sivusto, joka sisältää IIS * *BAService* * sovelluksen.* *BAService** sovellus käyttää välitys sidonta sisäisesti Laajenna paikallinen Palvelupäätepisteen pilveen saatavilla. Palvelu, jota paikallisen vastaava välitys päätepisteen rekisteröitävä palvelun Bus vain silloin, kun paikallisen-palvelu käynnistyy.  

Jos Lopeta ja Käynnistä jokin sovellus, aloittamisen sovelluksen automaattinen määritys ei käyttää. Niin **BAService** lopetetaan, kun on aina käynnistettävä **BizTalk sovittimen Service** -web-sivuston sijaan. Älä käynnistä tai Pysäytä **BAService** -sovellus.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Erikoismerkit ei voi käyttää osoite ja kohteen nimien LOB osat
Osoite- ja yritys nimien LOB osien Älä käytä erikoismerkkejä. Jos teet näin, näkyviin tulee Virhe otettaessa BizTalk palvelun projektin. Tiettyjä merkkejä, kuten "%", BizTalk sovittimen Service-sivuston voi Siirry pysäytetty vaiheeseen ja sinun tarvitse käynnistää manuaalisesti.
### <a name="test-map-with-get-context-property"></a>Testi-kartan, jossa on Hae konteksti-ominaisuus
Jos muunto sisältää **Hae konteksti-ominaisuuden** kartta-toimintoa, **Testaa kartan** epäonnistuu. Tilapäinen kiertää tämän ongelman tilalle, merkkijonon KETJUTA kartta-toiminto tyhjä tiedot sisältävä **Hae konteksti-ominaisuuden** kartta-toimintoa. Tämä täyttää kohde-rakenne ja Salli testaat Muunna muita toimintoja.
### <a name="test-map-property-does-not-display"></a>Testaa Yhdistä-ominaisuus ei näy
**Testaa kartta** -ominaisuudet eivät näy Visual Studiossa. Näin voi käydä, jos kiinnitetty, **Ominaisuudet** -ikkunassa ja **Ratkaisunhallinnassa** -ikkunassa on ei samanaikaisesti. Voit ratkaista tämän kiinnittää **Ominaisuudet** ja **Ratkaisu Explorer** -ikkunat.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>DateTime alustaa avattavasta näkyy harmaana
Kun DateTime alustaa kartta-toiminto on lisätty suunnitteluosaan ja määritetty, muoto avattavasta luettelosta saattaa harmaana. Näin voi käydä, jos tietokoneen näyttö on määritetty **Keskikokoinen-125 %** tai **Suurenna – 150 prosenttiin**. Voit ratkaista, määritä näytön **Pienennä – 100 %: n (oletus)** seuraavasti:  
1. Avaa **Ohjauspaneeli** ja valitse **Ulkoasu ja mukauttaminen**.
2. Valitse **Näytä**.
3. **Pienennä – 100 %: n (oletus)** ja sitten **Käytä**.

**Muoto** avattavasta luettelosta pitäisi nyt toimi odotetulla tavalla.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Kaksoiskappaleiden sopimuksia BizTalk palvelut-portaalissa
Ota huomioon seuraavat skenaariota:
1. Luo myynti kumppanin hallinta OM Ohjelmointirajapinnan käyttäminen sopimuksia.
2. Avaa sopimuksen BizTalk palveluiden portaali kaksi eri välilehdissä.
3. Ota käyttöön sekä välilehtien sopimuksen.
4. Tämän vuoksi sekä sopimusten tule käyttöön tuloksena on kaksoisarvoja BizTalk palvelut-portaalissa

**Vaihtoehtoinen menetelmä**. Avaa jokin kaksoiskappaleiden sopimuksia BizTalk palvelut-portaalissa ja undeploy.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Päivitetty varmenne Älä käytä siltoja senkin jälkeen, kun varmenne on päivitetty Palvelutietojen säilöön
Kuvitellaan seuraava tilanne:  

**Tapaus 1: Allekirjoitus-pohjaisen varmenteiden käyttäminen suojaamiseen tiedonsiirto sillan palvelupäätepiste**  
Harkitse tilanne, jossa allekirjoitus-pohjaisen varmenteiden käytöstä BizTalk palvelun projektin. Päivitä varmenteen BizTalk palvelut-portaalissa on sama nimi, mutta erilaisia allekirjoitus, mutta eivät päivity BizTalk palvelun projektin vastaavasti. Näiden tilanteessa siltaan voivat edelleen käsitellä viestejä, koska vanhempia varmenteen tietojen saattaa olla edelleen kanavan välimuistin. Tämän jälkeen käsittely epäonnistuu.  

**Vaihtoehtoinen menetelmä**: Päivitä BizTalk palvelun projektin varmenteen ja ota uudelleen projektin.  

**Tapaus 2: Tunnistaa varmenteet suojaaminen tiedonsiirto sillan Palvelupäätepisteen nimi perustuvia toimintamalleja avulla**

Harkitse tilanne, jossa käytät nimi perustuvia toimintamalleja tunnistavan BizTalk palvelun projektin varmenteet. Päivitä varmenteen BizTalk palvelut-portaalissa, mutta eivät päivity BizTalk palvelun projektin vastaavasti. Näiden tilanteessa siltaan voivat edelleen käsitellä viestejä, koska vanhempia varmenteen tietojen saattaa olla edelleen kanavan välimuistin. Tämän jälkeen käsittely epäonnistuu.  

**Vaihtoehtoinen menetelmä**: Päivitä BizTalk palvelun projektin varmenteen ja ota uudelleen projektin.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Siltoja edelleen Käsittele viestejä, vaikka SQL-tietokanta on offline-tilassa
BizTalk Services siltoja jatkaa sanomien käsittelemiseen jonkin aikaa, vaikka Microsoft Azure SQL-tietokantaan (joka sisältää käynnissä tietoja, kuten käyttöön palvelutiedot ja putkistot), on offline-tilassa. Tämä johtuu siitä BizTalk Services käyttää välimuistiin tallennetut tiedot ja sillan määritys.
Jos et halua käsitellä viestejä, kun SQL-tietokanta on offline-tilassa siltoja, voit lopettaa tai keskeyttää BizTalk-palvelun BizTalk palveluiden PowerShell cmdlet-komennot avulla. Katso [Azure BizTalk Service Management otoksen](http://go.microsoft.com/fwlink/p/?LinkID=329019) Windows PowerShellin cmdlet-komennot hallittavan toimintoja.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Lukeminen silta mukautettua koodia osan sisällä XML-viesti sisältää ylimääräisiä Tuoterakenteen-merkki
Harkitse tilanne kohtaa, johon haluat lukea XML-viestin silta mukautetun koodiin. Jos käytät .NET API System.Text.Encoding.UTF8.GetString(bytes) ylimääräisiä Tuoterakenteen-merkin sisältyy tulosteen viestin alussa. Näin on, jos et halua sisällyttää ylimääräisiä Tuoterakenteen merkin tulos, sinun on käytettävä ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Viestien lähettäminen käyttämällä WCF sillan skaalaudu
Käyttämällä WCF sillan lähetetyt viestit skaalaudu. HttpWebRequest kannattaa käyttää sen sijaan, jos haluat skaalattava asiakas.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>PÄIVITYS: Tunnuksen toimittaja-virhe, yleinen käytettävyys (GA) BizTalk Services esikatselu: stä päivittämisen jälkeen
Ei muokkaa tai AS2 sopimuksen aktiivinen erissä kanssa. Kun BizTalk-palvelun päivitetään esikatselusta GA, seuraavat saattaa ilmetä:
* Virhe: Suojaustunnuksen toimittaja ei pysty esittämään suojaustunnus. Palauttaa viestin tunnuksen toimittaja: etätietokoneen nimi ei voi selvittää.

* Erän Tehtävät peruutetaan.

**Vaihtoehtoinen menetelmä**: jälkeen BizTalk palvelun päivitetään, yleinen käytettävyys (GA), ota sopimuksen uudelleen.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>PÄIVITYS: Työkaluryhmän näyttää vanha silta kuvakkeet on BizTalk SDK: N päivittämisen jälkeen
Kun olet päivittänyt BizTalk Services SDK-paketissa, joka oli vanha kuvakkeet siltoja aiemmassa versiossa työkaluryhmän säilyy Näytä siltoja vanha kuvakkeet. Jos lisäät sillan BizTalk palvelun projektin suunnittelutyökalun pinta, pinta näyttää uusi kuvake.  

**Vaihtoehtoinen menetelmä**. Voit ratkaista ongelman poistamalla .tbd-tiedostoja, valitse <system drive>: \Users\<käyttäjän > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>PÄIVITYS: BizTalk Portal päivityksen esikatselusta GA saattavat näyttää virheen, joka ilmaisee, että Muokkaa-ominaisuus ei ole käytettävissä
Jos olet kirjautunut BizTalk palvelut-portaaliin, kun BizTalk-palvelut on päivitetty esikatselusta GA, voit saada seuraavan virheilmoituksen portaalissa:  

Tämä ominaisuus ei ole käytettävissä Microsoft Azure BizTalk Services tähän versioon osana. Voit käyttää seuraavia ominaisuuksia siirtymällä haluamasi versio.  

**Ratkaisu**: lokiin portaalin, Sulje ja Avaa selaimessa, ja valitse portaalissa on kirjauduttava ulos.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>PÄIVITYS: Seuranta uudet tiedot eivät näy jälkeen BizTalk Services päivitetään GA
Oletetaan, joihin on otettu käyttöön BizTalk Services esikatselu tilauksen XML-silta tilanne. Lähettämiseen siltaan ja sitä vastaava tietojen seuranta on käytettävissä BizTalk palveluiden portaali. Nyt, jos BizTalk palveluiden portaali ja BizTalk palvelujen runtime bittien päivitetään GA, viesti lähetetään samaan silta päätepisteen käyttöön aiemmin seurantatietoja ei näy lähetetyt päivityksen jälkeen.  

### <a name="pipelines-vs-bridges"></a>Putkistot v/s siltoja
Tässä asiakirjassa termin "putkistot" ja 'siltoja' käytetään tarkoittavat. Molemmat tarkoittavat olennaisesti samojen, joka on otettu käyttöön BizTalk Services viestin käsittelyn yksikkö.  

### <a name="concepts"></a>Käsitteitä  

[BizTalk palvelut](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

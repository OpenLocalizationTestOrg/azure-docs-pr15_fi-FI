<properties 
    pageTitle="Opettele luomaan AS2 sopimuksia Pack Enterprise-integrointi" 
    description="Opettele luomaan yrityksen integrointi Pack AS2 sopimuksia | Sovelluksen Microsoft Azure-palvelu" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Yrityksen AS2 integrointi

## <a name="create-an-as2-agreement"></a>Luo AS2-sopimus
Jotta voit käyttää yritysominaisuudet logiikan sovelluksissa, sinun on luotava sopimusten. 

### <a name="heres-what-you-need-before-you-get-started"></a>Seuraavassa on tarvittavat ennen aloittamista
- Azure-tilaukseesi määritetty [integrointi tili](./app-service-logic-enterprise-integration-accounts.md)  
- Vähintään kaksi [kumppanien](./app-service-logic-enterprise-integration-partners.md) jo määrittänyt tilin integrointi  

>[AZURE.NOTE]Kun luot sopimuksen, sopimus-tiedoston sisältö on vastattava sopimuksen tyyppi.    


Kun olet [integrointi tilin luoda](./app-service-logic-enterprise-integration-accounts.md) ja [lisätä kumppanit](./app-service-logic-enterprise-integration-partners.md), voit luoda sopimuksia toimimalla seuraavasti:  

### <a name="from-the-azure-portal-home-page"></a>Azure-portaalin kotisivulla

Kun kirjaudut [Azure portal](http://portal.azure.com "Azure portaalissa"):  
1. Valitse vasemmasta valikosta **Selaa** .  

>[AZURE.TIP]**Selaa** -linkki ei ole näkyvissä, jos joudut ehkä Laajenna ensin. Toiminto valitsemalla **Näytä-valikko** -linkki, joka sijaitsee osoitteessa tiivistetyt valikon vasemmasta yläkulmasta.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Kirjoita *integrointi* suodattimen hakuruutuun ja valitse sitten **Integrointi tilit** tulosten luettelosta.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Valitse integration-tili, johon luot sopimuksen, joka avaa **Integrointi tilit** -sivu. Jos et näe integraatio tilien luetteloiden [luomaan ensimmäinen](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Valitse **sopimusten** -ruutu. Jos et näe toimeenpano-ruutu, lisää se ensin.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Valitse **Lisää** -painike toimeenpano-sivu, joka avautuu.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Kirjoita **nimi** sopimuksesi ja valitse sitten **Host kumppani**, **Host tunnistetiedot**, **Vieras kumppani**, **Vieras tunnistetietojen**toimeenpano-sivu, joka avautuu.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Seuraavassa on muutamia tietoja saattaa olla hyödyllistä, kun sopimuksesi asetusten määrittämisestä: 
  
|Ominaisuus|Kuvaus|
|----|----|
|Host (isäntä)-kumppani|Sopimus on Host (isäntä)- ja Vieras kumppani. Host (isäntä)-kumppani edustaa organisaatiossa, jossa on määrittäminen sopimuksen.|
|Host (isäntä)-tunnistetiedot|Host (isäntä)-kumppani tunniste. |
|Vieras kumppani|Sopimus on Host (isäntä)- ja Vieras kumppani. Vieras kumppanin edustaa organisaatiossa, jossa on käy kauppaa host kumppanin kanssa.|
|Vieras tunnistetiedot|Vieras kumppanin tunniste.|
|Vastaanottamisen asetusten muokkaaminen|Nämä ominaisuudet koskee kaikkia viestejä vastaanottanut sopimus|
|Lähetä asetukset|Nämä ominaisuudet koskee kaikkia viestejä, jotka on lähettänyt sopimuksia|  
Oletetaan, että edelleen:  
7. Valitse **Vastaanottoasetuksia** , voit määrittää, miten vastaanotetuista tekstiviesteistä tämän sopimuksen viestit ovat käsitellään.  
 
 - Vaihtoehtoisesti voit ohittaa saapuvan viestin ominaisuudet. Toiminto, valitse **Ohita viestin ominaisuudet** -valintaruutu.
  - Valitse **viesti on allekirjoitettu** -valintaruutu, jos haluat vaatia allekirjoittaa kaikki saapuvat viestit. Jos valitset tämän vaihtoehdon, sinun on myös **varmenne** , jota käytetään viestien allekirjoituksen vahvistaminen.
  - Vaihtoehtoisesti voit edellyttää, että myös salattuja viestejä. Toiminto, valitse **viesti on salattu** -valintaruutu. Voit sitten on **varmenne** , jota käytetään toistaa saapuvat viestit.
  - Voit myös haluta pakataan viestejä. Toiminto, valitse **viesti voi pakata** -valintaruutu.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Katso alla oleva taulukko, jos haluat lisätietoja siitä, mitä vastaanotto asetusten avulla.  

|Ominaisuus|Kuvaus|
|----|----|
|Ohittaa viestin ominaisuudet|Valitse tämä, jos osoittamaan ominaisuudet saapuneille viesteille voi ohittaa |
|Viesti on allekirjoitettu|Tämä edellyttää digitaalisesti allekirjoitettujen viestien ottaminen käyttöön|
|Viesti on salattu|Ottaa tämän edellyttämään salattuja viestejä. Muut salattuja viestejä hylätään.|
|Viestin voi pakata|Ottaa tämän edellyttämään pakataan viestejä. Ei pakata viestit hylätään.|
|MDN teksti|Tämä on oletusarvo MDN voidaan lähettää viestin lähettäjälle|
|Lähetä MDN|Ota tämä, jos haluat sallia lähetettäväksi MDNs.|
|Lähetä allekirjoitettujen MDN|Ottaa tämän edellyttämään MDNs allekirjoitusta.|
|Mikrofoni algoritmin||
|Lähetä asynkroninen MDN|Ottaa tämän edellyttämään viestejä voidaan lähettää asynkronisesti.|
|URL-OSOITE|Tämä on URL-osoite, johon viestit lähetetään.|
Nyt jatkaa oletetaan, että:  
8. Valitse **Lähetä asetuksia** voit määrittää, miten tämän sopimuksen kautta lähetetyt viestit ovat käsitellään.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Katso alla oleva taulukko, jos haluat lisätietoja siitä, mitä Lähetä asetusten avulla.  

|Ominaisuus|Kuvaus|
|----|----|
|Ota käyttöön viestin allekirjoitus|Valitse tämä valintaruutu, jotta kaikki sopimuksen allekirjoittaa lähettämät viestit.|
|Mikrofoni algoritmin|Valitse viestin allekirjoitus käyttäminen algoritmin|
|Varmenne|Valitse viestin allekirjoitus käyttäminen varmenne|
|Ota käyttöön salaus|Valitse tämä valintaruutu, jos haluat salata kaikki tämän sopimuksen lähettämät viestit.|
|Salausalgoritmi|Valitse salausalgoritmi salaus käyttäminen|
|Liukuvat HTTP-otsikot|Valitse tämä valintaruutu liukuvat HTTP-sisältötyypin otsikon yhdelle riville.|
|Pyydä MDN|Ota käyttöön tämä valintaruutu, jos pyytää MDN, kaikki tämän sopimuksen lähettämät viestit|
|Pyyntö allekirjoitettu MDN|Pyydä, että kaikki tämän sopimuksen lähetetään MDNs kirjautunut ottaminen käyttöön|
|Pyydä asynkroninen MDN|Ota käyttöön asynkroninen MDN tämän sopimuksen lähettämisen pyytää|
|URL-OSOITE|URL-osoite, johon MDNs lähetetään|
|Ota käyttöön NRR|Valitse tämä valintaruutu, jos käyttöön ei-hylkäämistä vastaanotto|
Olemme melkein valmis!  
9. Valitse Integration tili-sivu- **sopimusten** -ruutu ja näet juuri lisätyt sopimuksen luettelossa.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)


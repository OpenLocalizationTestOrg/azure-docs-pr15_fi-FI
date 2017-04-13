
<properties 
    pageTitle="Azure RemoteApp ulos monella tavalla | Microsoft Azure" 
    description="Tutustu erilaisiin ulos Azure RemoteApp siirtämistä varten." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Azure RemoteApp ulos monella tavalla

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Jos olet lopettanut käyttämällä Azure RemoteApp [käytöstä poistaminen ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) vuoksi tai koska arviointi on valmis, sinun on siirrettävä Azure RemoteApp luettelosta toinen sovellus-palveluun. Kahdella eri tavalla siirtämistä varten: itse hallitun (kutsutaan usein infrastruktuurin palveluna [IaaS]) käyttöönoton tai täysin hallitun (kutsutaan usein ympäristö palveluna) tai ohjelmiston palveluna [PaaS/SaaS] tarjoaminen. 

Omatoiminen IaaS on kodin pieniin kunnostustöihin käyttöönoton on hallitun, ylläpitämä ja omistamasi näennäiskoneiden (VMs) tai fyysinen järjestelmien suoraan käyttöön. Toisessa päässä täysin hallitun PaaS/SaaS tarjoaminen on lisätietoja, kuten Azure RemoteApp - kumppani sisältää palvelun kerroksen Etäpalvelujen ratkaisun, joka käsittelee toiminnallisia päälle ja ylläpidon, offline-asiakas, toimi joitakin kuvan ja sovelluksen hallinta.

Lisätietoja on esimerkkejä isännöintipalvelu vaihtoehtoihin luettu.    

## <a name="self-managed-iaas-solutions"></a>Itse hallitut (IaaS) ratkaisut

### <a name="rds-on-iaas"></a>**Valitse IaaS RDS** 
Voit ottaa käyttöön alkuperäiset istunnon perustuva Etätyöpöytäpalvelut (kohdassa Windows Server) käyttöönoton käyttämällä RemoteApp tai työpöytiä paikallisen tai isännöityä ympäristössä (like-Azure VMs). RDS-IaaS ominaisuuksissa ovat parhaiten asiakkaille, jotka ovat jo aiemmin mutta joissa RDS ominaisuuksissa aiemmin teknistä osaamistasi. 

> [AZURE.NOTE]
> Tarvitset Volume Licensing kanssa Software Assurance (SA) RDS client access-käyttöoikeuksien käyttöönoton tämän asetuksen.

Käyttöönotto-Azure VMs RDS on helpompaa, kun käytät käyttöönotto- ja korjausta mallit (luku on [Yleiskatsaus](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) ja sitten [Siirry poistaa ne](https://aka.ms/rdautomation)). Saat samat joustavasti skaalauksen mahdollisuudet Azure RemoteApp Azure perinteinen käyttöönoton mallin resurssit (ei Azure resurssin mallin resursseja) kanssa käyttämällä [Automaattinen skaalaus komentosarjan](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), vaikka enemmän mukautuksia ja määrityksiä. Kun otat käyttöön Azure VMs RDS-tuki annetaan kautta [Azure tukea](https://azure.microsoft.com/support/plans/), saman tukihenkilöt, joka tukee Azure RemoteApp. Voit saada kustannuksia arvioiden perusteella aiemmin käyttö ottamalla yhteyttä [Tukipalveluun Azure](https://azure.microsoft.com/support/plans/)ja voi suorittaa laskutoimituksia itse käyttämällä pian on julkaistu kustannukset Laskimen.  Lisäksi N sarjan VMs (olevan yksityisen esikatselu) voit lisätä vGPU - tietää lisää tietoja lisäämällä vGPU ja menettelytapoja [RDS parannuksia Windows Server 2016](https://myignite.microsoft.com/videos/2794) Ignite tässä istunnossa.   

[Windows Server 2012 R2: n](http://aka.ms/rdsonazure) ja [Windows Server 2016](http://aka.ms/rdsonazure2016) käyttöönoton helpottamiseksi vaiheittainen käyttöönoton oppaita on. Tutustu uusimmista [Etätyöpöydän blogiin](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) .
 
### <a name="citrix-on-iaas"></a>**Valitse IaaS Citrix** 
Alkuperäisen Citrix, joka on asennettu istunnon perustuva XenApp tai XenDesktop voidaan ottaa käyttöön paikallisen tai isännöityä ympäristössä (kuten Azure VMs käyttöön). 

Katso vaiheittaiset käyttöönotto-oppaan, [Citrix XA 7.6 Azure-](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), katso lisätietoja. Lisätietoja on artikkelissa [Azure-Citrix](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), mukaan lukien hinta Laskimeen. Voit etsiä myös [Citrix yhteyttä](http://citrix.com/English/contact/index.asp) Keskustele asetusten kanssa.

## <a name="fully-managed-paassaas-offerings"></a>Täysin hallitun (PaaS/SaaS) tarjouksia

### <a name="citrix-cloud"></a>**Citrix pilveen** 
[Citrix aiemman cloud-ratkaisun](https://www.citrix.com/products/citrix-cloud/)architecturally vastaamaan Citrix XenApp Express. Citrix tarjoaa [50 % alennuksen ylentäminen](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) Azure RemoteApp asiakkaille. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (-tech Preview'n)**
[Rekisteröidy niiden tech Preview'n](http://now.citrix.com/remoteapp), ja katsoa käyttäjien [Ignite istunnon](https://myignite.microsoft.com/videos/2792) (aloittaen minuutin 20:30). XenApp Express on sama kuin architecturally Citrix pilveen, lukuun ottamatta sitä, että se sisältää yksinkertaistetun hallinnan Käyttöliittymä ja muut toiminnot ja ominaisuudet, jotka muistuttavat Azure RemoteApp. 

Lisätietoja [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix Service Provider Program** 
Citrix palveluntarjoajan ohjelma on helppo for palveluntarjoajia aikana virtual cloud tietojenkäsittely SMBs, helppokäyttöisyyden niiden he haluavat helppoa, tukiyhteyksiä mallin palvelujen tarjoaminen. Citrix palveluntarjoajien kasvaa Microsoft SPLA niiden yritykset ja laajenna niiden RDS ympäristö sijoitukset laitteen kanssa, missä tahansa, laaja ohjelmien tuki, monipuolisia kokemus, suojauksen ja parantavat skaalattavuus. Puolestaan Citrix palveluntarjoajien herättää Lisää tilaajat, Suurenna asiakkaiden tyytyväisyyttä ja vähentää niiden toiminnallisia kustannuksia. [Lue lisää](http://www.citrix.com/products/service-providers.html) tai [Etsi kumppani](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft isännöidään palveluntarjoaja** 
Isännöintipalvelu kumppanit tarjoavat yleensä täysin hallitun isännöidään Windows-työpöytä ja sovelluspalvelu, jotka sisältävät Azure resursseja, käyttöjärjestelmät, sovellusten ja käyttämällä kumppanin tukipalvelu hallinta on käyttöoikeudet toimeenpano Microsoftin ja muiden tarjoajat ja käytössä palveluntarjoajan käyttöoikeussopimus Salli, jotka jälleenmyyvät tilaajan Access käyttöoikeuden (SAL). Seuraavat tiedot annetaan vinkkejä ja joitakin palveltavat järjestelmänvalvojat voivat, jotka ovat erikoistuneet avustaminen asiakkaille, joilla on niiden Azure RemoteApp-siirron yhteystiedot. Tarkista [isännöidään palveluntarjoajien nykyinen luettelo](http://aka.ms/rdsonazurecertified) , joka on valmis, valitse polku ja arviointi Koulujen IaaS RDS.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) specializes siirtymisessä pilveen valmistajille ja ISV "Haluatko optimoida niiden nykyinen cloud-asetukset. ASPEX on monenlaisia hallittuja palveluja, devops ja konsultointi palvelut.  

Ensisijainen sijainti: Antwerpen, Belgia

Toiminnon alue: Länsi Europe

Kumppanin tila: [Hopea](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoftin palveluntarjoajan: Kyllä

Tarjoavat istunnon perustuva RemoteApp ja ratkaisuja: Kyllä, molemmat

Azure RemoteApp siirron ratkaisuja: Kyllä, [Katso](https://www.aspex.be/en/azure-remote-apps)

**Yhteyshenkilö:**

- Puhelin: +3232202198
- Sähköposti:[info@aspex.be](mailto:info@aspex.be)
- Verkko: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (käyttöympäristön nimi: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) on automaatio-ympäristö, IT yritysten yksinkertaistaa ja optimoi skaalata siirtymis- ja lähettämisen remote työpöytiä, sovellusten ja Microsoft Azure-pilvi-infrastruktuuria. 

MyCloudIT-käyttöympäristö lyhentää käyttöönoton 95 % Azure kustannusten 30 %, ja siirtyy niiden asiakkaan koko IT-infrastruktuurin ohjeisiin ja joitakin keskeisiä piirtojen pilveen. Kumppanit voivat nyt hallita asiakkaiden yksi yleinen-koontinäytössä, palvelun loppukäyttäjät eri puolilla maailmaa valmiuksia ja seurantaa tuottojen lisäämättä suorituskykyä tai laaja Azure koulutusta.  

Ensisijainen sijainti: Dallas, TX, USA

Toiminnon alue: maailmanlaajuisesti

Kumppanin tila: [kulta](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoftin palveluntarjoajan: Kyllä

Tarjoavat istunnon perustuva RemoteApp ja ratkaisuja: Kyllä, molemmat

Azure RemoteApp siirron ratkaisuja: Kyllä, [Katso](https://mycloudit.com/remote-app-microsoft/)

**Yhteyshenkilö:**
- Tarkistajan Garoutte yrityksen kehitysosaston VP

   Puhelin: 972-218-0741

   Sähköposti:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) specializes-isännöityä työpöydän ratkaisuja, jotka tarjoavat koko työpöydän ja sovellusten toimittaminen kokemukset laadittuihin Microsoftin Azure ja oman palvelinkeskusten perus yleinen asiakkaalle.

Ensisijainen sijainti: Lontoo, UK; Singaporen; Houston-TX

Toiminnon alue: maailmanlaajuisesti

Kumppanin tila: Kulta

Microsoftin palveluntarjoajan: Kyllä

Tarjoavat istunnon perustuva RemoteApp ja ratkaisuja: Kyllä, molemmat

Azure RemoteApp siirron ratkaisuja: Kyllä, [Katso](http://www.acuutech.com/ara-migration/)

**Yhteyshenkilö:**

- Iso-Britannia:

  5/6 York talon Langston tien

  Loughton, Essex IG10 3TQ
  
  Puhelin: +44 (0) 20 8502 2155
 
- Singapore:

  100 Cecil Street, #09-02 
  
  Maapallo Singapore 069532
  
  Puhelin: + 65 6709 4933
 
- Pohjois-Amerikka: 

  3601 s Sandman St.
  
  Suite 200 Houston-TX 77098
  
  Puhelin: +1 713 691 0800

## <a name="need-more-help"></a>Tarvitsetko lisää ohjeita?
Edelleen ohjeen valitseminen tai on muita kysymyksiä? Pyydä apua jollakin seuraavista tavoista. 

1.  Sähköpostin osoitteeseen [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Ota [Azure tukevat](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Aloita avaamalla [Azure tukevat kirjainkoko](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3.  Ota meihin yhteyttä. [Etsi paikallinen myynnin numero](https://azure.microsoft.com/overview/sales-number/).

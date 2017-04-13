<properties
    pageTitle="Luomisesta ja käyttämisestä sovelluksen Service-ympäristössä sisäinen kuormituksen | Microsoft Azure"
    description="Luomisesta ja käyttämisestä ILB Irjainkoko"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Sisäinen kuormituksen käyttäminen sovelluksen Service-ympäristössä #

Sovelluksen palvelun Environments(ASE)-ominaisuus on Premium-palvelu-vaihtoehto Azure App palvelun, joka tarjoaa parannetun asetusten-ominaisuus, joka ei ole käytettävissä usean vuokraajan molempia.  Ietokannan-toiminto ottaa käyttöön käytettävä Azure-Virtual Network(VNet) Azure sovelluksen-palvelun.  Sovelluksen palvelun ympäristöissä lukea, [mitä sovelluksen Service-ympäristössä] tarjoamia ominaisuuksia suurempi ymmärtämisen saada[ WhatisASE] ohjeissa.  Jos et tiedä eduista VNet toimivien lukea [Azure Virtual verkon usein kysytyt kysymykset][virtualnetwork].  


## <a name="overview"></a>Yleiskatsaus ##


Ietokannan voidaan ottaa käyttöön internet käytettävissä päätepisteen tai IP-osoitteen lisääminen VNet kanssa.  Jotta voit määrittää IP-osoitetta haluat ottaa yhteyttä Ietokannan sisäinen kuormituksen-Balancer(ILB) kanssa VNet sähköpostiosoitteeseen.  Kun oman Ietokannan on määritetty ILB on:

- oman toimialueen tai alitoimialueen.  Tämän asiakirjan olettaa, jotta helposti alitoimialueen, mutta voit määrittää sen molemmilla tavoilla.  
- HTTPS sertifikaatti
- Oman alitoimialueen DNS-hallintaa.  


Voit tehdä asioita seuraavasti:

- Host intranet-sovellukset, kuten yrityssovellussarjan suojatusti pilvipalvelussa, jossa voit käyttää sivustoa tai ExpressRoute VPN-sivuston kautta
- Host (isäntä)-sovelluksia pilveen, jotka eivät näy julkisen DNS-palvelimet
- eristetty internet-taustatietokannan sovellusten joka edusta-sovellukset voivat turvallisesti integroida luominen


#### <a name="disabled-functionality"></a>Käytöstä poistetut toiminnot ####

On joitakin toimintoja, jotka voi tehdä ILB Ietokannan käytettäessä.  Nämä ovat seuraavat:

- IPSSL käyttäminen
- IP-osoitteiden määrittäminen tietyn sovellukset
- ostamalla ja sertifikaatilla, jonka sovelluksen portaalin kautta.  Voit hankkia varmenteet kanssa suoraan varmenteen myöntäjä kurssin edelleen ja käyttäminen sovelluksia, vain palvelun Azure-portaalissa.


## <a name="creating-an-ilb-ase"></a>ILB Ietokannan luominen ##

ILB Ietokannan luominen ei ole paljon eri luomasta Ietokannan normaalisti.  Jos tarkempaa keskustelun luomisesta Ietokannan Lue, [miten voit luoda sovelluksen palvelun ympäristön][HowtoCreateASE].  Luodaan ILB Ietokannan on sama Luo VNet Ietokannan luotaessa tai olemassa VNet välillä.  Voit luoda ILB Ietokannan seuraavasti: 

1.  Azure-portaalissa Valitse **Uusi -> Web + Mobile -> App Service-ympäristössä**
2.  Valitse tilaus
3.  Valitse tai luo resurssiryhmä
4.  Valitse tai luo VNet
5.  Luo aliverkon, jos VNet valitseminen
6.  **Virtuaalinen verkkosijainti-> VNet määritys** ja määritä sisäinen VIP-tyyppi
7.  Anna alitoimialueen nimi (tämä on lisäämäsi alitoimialue, sovellukset on luotu tämän Ietokannan käytettävä)
8.  Valitse Ok ja luo sitten


![][1]


Sisällä VPN-sivu on VNet määritysvaihtoehto.  Näillä oikeuksilla voit valita ulkoisen VIP tai sisäinen VIP välillä.  Oletusarvo on ulkoinen.  Jos olet sen arvoksi ulkoinen oman Ietokannan käyttää käytettävissä internet-VIP.  Jos valitset sisäinen, että Ietokannan määritetään ILB-että VNet IP-osoitteen.  


Valittuasi sisäinen mahdollista lisätä Lisää IP-osoitteiden oman Ietokannan poistetaan ja sen sijaan sinun on annettava Ietokannan alitoimialueen.  Ietokannan kanssa ulkoisen VIP Ietokannan nimeä käytetään alitoimialueen kyseisen Ietokannan luodaan-sovellusten.  
Jos yhteyttä Ietokannan on kutsuttu ***contosotest*** ja sovelluksen siten, että Ietokannan kutsuttiin ***mytest*** sitten alitoimialueen olisivat muoto ***contosotest.p.azurewebsites.net*** ja kyseisen sovelluksen URL-osoite on ***mytest.contosotest.p.azurewebsites.net***.  
Jos määrität VIP tyyppi sisäinen, Ietokannan nimi on ei käytetä Ietokannan alitoimialueen.  Voit määrittää alitoimialueen erikseen.  Jos yhteyttä alitoimialueen oli ***contoso.corp.net*** ja tekemäsi sovelluksen siten, että Ietokannan nimeltä ***timereporting*** URL-osoite, sovelluksen olisi ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>ILB Ietokannan sovellukset ##

Sovelluksen luominen ILB Ietokannan on sama kuin Ietokannan-sovelluksen luominen normaalisti.  

1. Valitse Azure portaalissa **Uusi Web -> + Mobile -> Web** tai **Matkapuhelin** tai **API-sovellus**
2. Kirjoita sovelluksen nimi
2. Valitse tilaus
3. Valitse tai luo resurssiryhmä
4. Valitse tai Luo sovelluksen palvelun Plan(ASP).  Jos luodaan uusi ASP Valitse että Ietokannan sijainniksi, ja valitse haluat luoda ASP työntekijä vähäisen.  Kun luot ASP valitaan oman Ietokannan sijainti ja työntekijä resurssivarantoon.  Kun määrität sovelluksen nimeä näet, että sovelluksen kohdalta alitoimialueen korvataan alitoimialueen oman Ietokannan varten.   
5. Valitse Luo.  Sinun on valittava **raporttinäkymät-ikkunan kiinnittäminen** -valintaruutu, jos haluat sovelluksen näkyvän koontinäyttöön.  

![][2]


Valitse sovelluksen nimen alitoimialueen nimen saa päivittää oman Ietokannan alitoimialueen.  


## <a name="post-ilb-ase-creation-validation"></a>Kirjaa ILB Ietokannan luomisen vahvistus ##

ILB Ietokannan on hieman erilainen kuin ei - ILB Ietokannan.  Kuten jo merkille haluat hallita omia DNS- ja käytössä on myös antaa omat varmenteen HTTPS-yhteyksien.  


Kun olet luonut oman Ietokannan huomaat alitoimialueen näkyy alitoimialueen määrittämäsi ja uuden kohteen on **nimeltään **ILB varmenteen**asetusvalikko** .  Ietokannan luodaan itse allekirjoitettua varmennetta, joka on helppo testata HTTPS.  Portaalin selviää, jotka haluat antaa oman varmenteen HTTPS, mutta tämä on asema voi olla oman alitoimialueen antamaasi käytettävä varmenne.  

![][3]


Jos yksinkertaisesti rooliin kohteita ja tiedä, miten varmenteen, voit itse allekirjoitetun varmenteen luominen IIS MMC console-sovelluksen avulla.  Sen luomisen jälkeen voit viedä .pfx-tiedostona ja lataa se sitten ILB varmenteen käyttöliittymässä. Kun käytät sivustoa, joka on suojattu itse allekirjoitettua varmennetta, selaimen antaa varoituksen, että käytät käyttöoikeuspalvelinta sivuston ei ole suojattu vuoksi ei voi vahvistaa sertifikaatin.  Jos et halua, että varoitus on oikein allekirjoitettua varmennetta, joka vastaa omaa alitoimialueen ja on komentoketju, jonka luottamusta, tunnista selaimella.

![][6]

Jos haluat kokeilla kulun oman todistuksia ja Testaa yhteyttä Ietokannan HTTP- ja HTTPS pääsy:

1.  Siirry Ietokannan Käyttöliittymän Ietokannan luomisen jälkeen **Ietokannan -> Asetukset -> ILB varmenteiden**
2.  Määritä ILB varmenteen valitsemalla pfx sertifikaattitiedosto ja Anna salasana.  Tämä vaihe kestää hetken käsittelemään ja näytetään viesti, joka skaalauksen toiminto on käynnissä.
3.  ILB-osoitteen hankkiminen oman Ietokannan (**Ietokannan -> ominaisuudet -> Virtual IP-osoite**)
4.  Web-sovelluksen luominen Ietokannan luomisen jälkeen 
5.  Luo AM, jos sinulla ei ole sellainen, että VNET (eivät sisälly saman aliverkon kuin Ietokannan tai kohteita tauko)
6.  Voit määrittää oman alitoimialueen DNS.  Voit käyttää yleismerkkiä oman alitoimialueen DNS- tai jos haluat tehdä kaikkia yksinkertainen testejä Muokkaa yhteyttä AM määritetään web-sovelluksen nimi VIP IP-osoite isännät-tiedosto.  Jos yhteyttä Ietokannan alitoimialueen nimen. ilbase.com ja voit tehdä web app-mytestapp niin, että se osoitettu osoitteessa mytestapp.ilbase.com sitten asettaa isännät-tiedostossa.  (Windows isännät-tiedosto on osoitteessa C:\Windows\System32\drivers\etc\)
7.  Selaimella, AM ja siirry http://mytestapp.ilbase.com (tai joista web-sovelluksen nimi on kohdassa alitoimialue)
8.  Selaimella, AM ja siirry https://mytestapp.ilbase.com on Hyväksy suojauksen puuttuminen, jos itse allekirjoitettua varmennetta.  


Oman ILB IP-osoite näkyy ominaisuuksista kuin Virtual IP-osoite

![][4]


## <a name="using-an-ilb-ase"></a>Käyttämällä ILB Irjainkoko ##

#### <a name="network-security-groups"></a>Verkon käyttöoikeusryhmät ####

ILB Ietokannan mahdollistaa verkon eristystaso sovelluksia varten sovellukset eivät ole käytettävissä tai jopa tunnetut internet mukaan.  Tämä on erinomainen isännöimiseen intranet-sivustoja, kuten yrityssovellussarjan.  Kun haluat rajoittaa myös muita edelleen voit verkon suojauksen Groups(NSGs) käytönvalvonta verkon tasolla. 


Jos haluat käyttää NSGs edelleen rajoittamisesta sinun on Varmista, että ei saa tapahtua tietoliikenne, joka Ietokannan tarvitsee toimivat.  Vaikka HTTP/HTTPS-käyttöoikeus on vain Ietokannan käyttämä ILB kautta Ietokannan määräytyy resurssin ulkopuolella VNet edelleen.  Voit tarkastella tietoja verkkokäyttö tarvitaan edelleen etsiminen tiedot asiakirjan [Saapuvan liikenteen hallinta App Service-ympäristössä] [ ControlInbound] ja [Verkon määritysten tiedot sovelluksen palvelun ympäristöissä, joissa ExpressRoute]asiakirjan[ExpressRoute].  


Jos haluat määrittää, että sinun tarvitsee tietää IP-osoite, jota käytetään Azure hallita oman Ietokannan NSGs.  Kyseisen IP-osoite on myös lähtevät IP-osoite-kohdassa Ietokannan, jos se on internet-pyynnöt.  Etsi IP address Siirry **Asetukset -> ominaisuudet** ja Etsi **Lähtevät IP-osoite**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Yleinen ILB Ietokannan hallinta ####

ILB Ietokannan hallinta on pääosin sama kuin hallinta Ietokannan normaalisti.  Tarvitset voit työntekijöiden-jakavat isännöidä ASP esiintymiä ja laajentaa edusta-palvelimet käsittelemään parantavat määriä HTTP/HTTPS-liikenne.  Yleistietoja Ietokannan määritysten hallinta lukea asiakirjan [määrittäminen sovelluksen Service-ympäristössä],[ASEConfig].  


Muita-kohteet ovat varmenteen hallinta ja DNS-hallinta.  Haluat saada ja Lataa sertifikaatti käytettävä HTTPS ILB Ietokannan luonnin jälkeen ja korvaa se ennen kuin se päättyy.  Koska Azure omistaa perus toimialueen olemme koonneet varmenteet ohjeet ASEs ulkoisen VIP kanssa.  Koska ILB Ietokannan käyttämä alitoimialueen voi olla mikä tahansa, sinun täytyy antaa oman varmenteen HTTPS. 


#### <a name="dns-configuration"></a>DNS-määritys ####

Kun käytät ulkoisia VIP DNS hallitaan Azure.  Minkä tahansa sovelluksen luotu oman Ietokannan lisätään automaattisesti Azure DNS, joka on julkinen DNS.  ILB Ietokannan sinulla omia DNS-hallinta.  Tietyn alitoimialue, kuten contoso.corp.net varten on luotava DNS A tietueet, jotka osoittavat ILB osoitteeseen:

    * 
    *.SCM ftp julkaiseminen 


## <a name="getting-started"></a>Käytön aloittaminen
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Aloita sovelluksen palvelun ympäristöissä, Lue lisää [ohjeaiheista johdanto App palvelun ympäristöissä][WhatisASE]

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/

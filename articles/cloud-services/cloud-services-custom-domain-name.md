<properties
    pageTitle="Mukautetun toimialuenimen määrittäminen pilvipalveluihin | Microsoft Azure"
    description="Lue, miten voit näyttää Azure sovelluksen tai tiedot mukautetun toimialueen DNS-asetusten määrittäminen."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Azure pilvipalvelussa mukautetun toimialuenimen määrittäminen

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-custom-domain-name-portal.md)
- [Azure perinteinen portal](cloud-services-custom-domain-name.md)


Kun luot pilvipalveluun, Azure määrittää sen alitoimialuetta cloudapp.net. Esimerkiksi jos pilvipalvelussa sijaitsevaan nimi on "contoso", käyttäjien on käyttää sovelluksen URL-osoitetta, kuten http://contoso.cloudapp.net. Azure määrittää myös virtual IP-osoite.

Voit myös näyttää sovelluksen käyttöön oman toimialuenimen, esimerkiksi contoso.com. Tässä artikkelissa kerrotaan, miten voit varata tai mukautettua toimialuenimeä pilvipalvelussa web roolien määrittäminen.

Haluatko jo undestand, mitä A ja CNAME-tietueet on? [Siirry aiempia explaination](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Hanki siirtymällä nopeammin! Käytä Azure [käytön vaiheittainen kuvaus](http://support.microsoft.com/kb/2990804). Se on liittämisestä mukautettua toimialuenimeä ja suojaaminen yhteyttä (SSL) Azure pilvipalveluihin tai Azure sivustoja nopeasti.

<p/>

> [AZURE.NOTE]
> Tämän tehtävän ohjeet koskevat Azure pilvipalveluihin. Katso sovelluksen palveluita [tämän](../app-service-web/web-sites-custom-domain-name.md). Tallennustilan-tileissä on [Tässä](../storage/storage-custom-domain-name.md).


## <a name="understand-cname-and-a-records"></a>Tietoja A ja CNAME-tietueet

CNAME-TIETUE (tai alias-tietueet) ja molemmat tietueet avulla voit yhdistää tiettyyn palvelimeen toimialuenimi (tai palvelun tässä tapauksessa) kuitenkin ne toimivat eri tavalla. Saatavilla on myös tiettyjä seikkoja käytettäessä tietueisiin Azure pilvipalveluihin, ota huomioon ennen valita käytettävää.

### <a name="cname-or-alias-record"></a>CNAME-tai Alias

CNAME-tietue yhdistää *tietyn* toimialueen, kuten **contoso.com** tai **www.contoso.com**canonical toimialuenimi. Tässä tapauksessa canonical toimialuenimi on **[myapp] .cloudapp .net** oman Azure toimialuenimi isännöidään sovelluksen. Luomiasi CNAME-TIETUE Luo tunnusnimen **[myapp] .cloudapp .net**. CNAME-merkinnän ratkaisee IP-osoitteen lisääminen **[myapp] .cloudapp .net** palvelun automaattisesti, joten cloud-palvelun IP-osoite muuttuu, jos sinun ei tarvitse tehdä mitään.

> [AZURE.NOTE]
> Jotkin toimialuerekisteröijät Salli vain voidaan yhdistää alitoimialueita, kun käytät CNAME-tietue, kuten www.contoso.com ja ei pääkansion nimiä, esimerkiksi contoso.com. Lisätietoja CNAME-tietueet on rekisteröintipalvelustasi, [CNAME-tietue Wikipedia-tapahtuma](http://en.wikipedia.org/wiki/CNAME_record)tai [IETF toimialuenimet - käyttöönotto ja määritys](http://tools.ietf.org/html/rfc1035) -asiakirjan ohjeissa.

### <a name="a-record"></a>Tietueen

A-tietue yhdistää toimialueen, kuten **contoso.com** tai **www.contoso.com**, *tai yleismerkkien toimialueen* ** \*. contoso.com**, IP-osoitteeseen. Jos kyseessä Azure pilvipalvelussa, virtual IP-palvelun. Jotta päälle CNAME-tietue A-tietue tärkeimmät etuna on se, että sinulla on yksi merkintä, joka käyttää yleismerkkejä, kuten \* **. contoso.com**, jossa käsitellä useita alitoimialueisiin, kuten **mail.contoso.com**, **login.contoso.com**tai **www.contso.com**pyynnöt.

> [AZURE.NOTE]
> Koska A-tietue on nyt yhdistetty staattinen IP-osoite, muutoksia ei voi selvittää Cloud-palvelun IP-osoitteen automaattisesti. Cloud-palvelun käyttämä IP-osoite on varattu ensimmäistä kertaa, voit ottaa käyttöön tyhjä paikka (tuotannon tai väliaikaisen.) Jos poistat vapaan käyttöönoton, IP-osoite on julkaissut Azure ja minkä tahansa tulevien käyttöönotoissa paikka, voidaan antaa uusi IP-osoite.
>
> Tietyn käyttöönoton paikka (tuotannon tai väliaikaisesta) IP-osoite on samanlainen helposti, kun väliaikaisesta ja tuotannon ominaisuuksissa tai suorittamiseen paikallaan tapahtuva päivitys aiemmin käyttöönoton välillä vaihtaminen. Saat lisätietoja näiden toimintojen tekemistä [hallinnasta pilvipalveluihin](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Mukautetun toimialueen CNAME-tietueen lisääminen

CNAME-tietueen luomiseen on lisättävä uusi merkintä DNS-taulukon mukautetun toimialueen rekisteröintipalvelustasi työkalujen avulla. Kunkin rekisteröintipalvelu on samanlainen, mutta se on hieman erilainen tapa CNAME-tietue, joka määrittää, mutta käsitteitä ovat samat.

1. Etsi jokin näistä tavoista avulla **. cloudapp.net** pilvipalvelussa sijaitsevaan toimialuenimi.

    * Kirjaudu sisään [Azure perinteinen portal], valitse pilvipalveluun, valitse **raporttinäkymät-ikkuna**ja Etsi **Sivuston URL-osoite** tapahtuma **quick glance** -osassa.
    
        ![Quick glance osa, jossa sivuston URL-osoite][csurl]
    
        **TAI**  
    
    * Asenna ja määritä [PowerShellin Azure](../powershell-install-configure.md)ja sitten seuraavalla komennolla:
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Tallenna joko menetelmän palauttama URL-osoitteen toimialuenimen, kun tarvitset sitä CNAME-tietuetta luotaessa.

1.  Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Etsi linkkejä tai **Toimialuenimi**, **DNS**- tai **Name Server Management**merkintä sivuston alueille.

2.  Etsi, jossa voit valita tai kirjoittaa henkilön CNAME. Saatat joutua Valitse tietuetyyppi avattavasta alaspäin tai valitsemalla Lisäasetukset-sivulla. Pitäisi näyttää sanoja **CNAME**- **Alias**tai **alitoimialueita**.

3.  On myös määritettävä toimialue tai alitoimialue alias CNAME-TIETUE, kuten **WWW-tunnistetta** , jos haluat luoda tunnusnimen **www.customdomain.com**. Jos haluat luoda tunnuksen päätoimialueen, sitä voidaan luettelossa nimellä '**@**' käyttämäsi Rekisteröintipalvelun DNS-työkalujen symboli.

4. Valitse sinun on määritettävä canonical isäntänimi, joka on sovelluksen **cloudapp.net** toimialueen tällöin.

Esimerkiksi seuraavat CNAME-tietueet välittää kaikki liikenne- **www.contoso.com** **contoso.cloudapp.net**-sovelluksen käyttöön mukautettua toimialuenimeä:

| Alias tai Host nimi/alitoimialueen | Kanoninen toimialueen     |
| ------------------------- | -------------------- |
| WWW-tunnistetta                       | contoso.cloudapp.NET |

**Www.contoso.com** , käyttäjä ei koskaan näe tosi host (contoso.cloudapp.net), niin edelleenlähetys-prosessi on näkymätön käyttäjälle.

> [AZURE.NOTE]
> **Www** -alitoimialueen liikenteen koskee vain yllä olevassa esimerkissä. Koska et voi käyttää yleismerkkejä CNAME-tietueet, sinun on luotava yksi CNAME-TIETUE kunkin toimialueen/alitoimialueen varten. Jos haluat liikenne voidaan ohjata osoitteesta alitoimialueita, kuten \*. contoso.com-cloudapp.net-osoitteeseen, voit määrittää **URL-Osoitteen ohjata** tai **URL-Osoitteen eteenpäin** tapahtuma DNS-asetukset, tai luo A-tietue.


## <a name="add-an-a-record-for-your-custom-domain"></a>Mukautetun toimialueen A-tietueen lisääminen

A-tietueen luomiseen on ensin etsittävä pilvipalvelussa sijaitsevaan virtual IP-osoite. Valitse Lisää uusi merkintä mukautetun toimialueen DNS-taulukon rekisteröintipalvelustasi työkalujen avulla. Kunkin rekisteröintipalvelu on samanlainen, mutta hieman eri keinoja niiden A-tietue, joka määrittää, mutta käsitteitä ovat samat.

1. Käytä jollakin seuraavista tavoista saat cloud-palvelun IP-osoite.
    
    * Kirjaudu sisään [Azure perinteinen portal], valitse pilvipalveluun, valitse **raporttinäkymät-ikkuna**ja Etsi **julkisen Virtual IP (VIP) osoite** tapahtuma **quick glance** -osassa.
    
        ![Quick glance osassa näkyy VIP][vip]
    
        **TAI**  
    
    * Asenna ja määritä [PowerShellin Azure](../powershell-install-configure.md)ja käytä sitten seuraava komento:
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Jos sinulla on useita päätepisteet cloud-palveluun liitetyn, näyttöön tulee IP-osoitetta, joka sisältää useita viivoja, mutta kaikki pitäisi näkyä on sama osoite.
    
    Tallentavat IP-osoite on pystyttävä A-tietuetta luotaessa.

1.  Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Etsi linkkejä tai **Toimialuenimi**, **DNS**- tai **Name Server Management**merkintä sivuston alueille.

2.  Etsi, jossa voit valita tai kirjoittaa tietueen. Saatat joutua Valitse tietuetyyppi avattavasta alaspäin tai valitsemalla Lisäasetukset-sivulla.

3. Valitse tai kirjoita toimialueen tai alitoimialueen, joka käyttää A-tietue. Valitse esimerkiksi **www** , jos haluat luoda tunnusnimen **www.customdomain.com**. Jos haluat luoda kaikki alitoimialueet yleismerkkien tekstin, kirjoita '__*__". Tämä kattaa kaikki alitoimialueisiin, kuten **mail.customdomain.com**, **login.customdomain.com**ja **www.customdomain.com**.

    Jos haluat luoda päätoimialueen A-tietue, sitä voidaan luettelossa nimellä '**@**' käyttämäsi Rekisteröintipalvelun DNS-työkalujen symboli.

4. Kirjoita määritetty kentän cloud-palvelun IP-osoite. Tämä liittää toimialueen tapahtuma käytetään tietueessa cloud palvelun käyttöönoton IP-osoitteeseen.

Esimerkiksi tietueen välittää kaikki liikenne **contoso.com** - **137.135.70.239**, IP-osoite on otettu käyttöön sovelluksesi seuraavasti:

| Host name/alitoimialueen | IP-osoite     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



Tässä esimerkissä näytetään luominen päätoimialueen A-tietue. Jos haluat kattaa kaikki alitoimialueet yleismerkkien tekstin luominen, syötä "__*__' alitoimialueen nimellä.

>[AZURE.WARNING]
>IP-osoitteiden Azure ovat dynaamisia oletusarvoisesti. Voit haluat [varattu IP-osoitteen](../virtual-network/virtual-networks-reserved-public-ip.md) avulla voit varmistaa, että IP-osoite ei muutu.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Voit hallita pilvipalveluihin](cloud-services-how-to-manage.md)
* [Voit yhdistää CDN sisältöä mukautetulla toimialueella](../cdn/cdn-map-content-to-custom-domain.md)
* [Yleisten määritysten cloud-palvelun](cloud-services-how-to-configure.md).
* Lisätietoja käyttöönottamisesta [pilvipalveluun](cloud-services-how-to-create-deploy.md).
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate.md).




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure perinteinen portal]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 
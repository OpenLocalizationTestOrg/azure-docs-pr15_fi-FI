<properties
    pageTitle="Vianmääritys heikentynyt tila Azure liikenteen hallinta"
    description="Liikenteen hallinta-profiileista vianmääritys, kun se on heikentynyt tila."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Vianmääritys heikentynyt tilan Azure liikenteen hallinta

Tässä artikkelissa kuvataan Azure liikenteen hallinta-profiilia, joka on näkyvissä eivät toimi oikein tila. Tässä skenaariossa Ota huomioon, että olet määrittänyt valitsemalla isännöidään cloudapp.net palvelujen liikenteen hallinta profiilin. Kun liikenteen hallinta kuntotietojen tarkistus, näet, että tila on heikentynyt.

![eivät toimi oikein tila](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Jos siirryt profiilin päätepisteet-välilehteen, näet yhden tai useamman päätepisteet Offline-tilassa:

![offline-tilassa](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Tietoja liikenteen hallinta keräysputkien

- Liikenteen hallinta katsoo päätepisteen on online-TILASSA vain silloin, kun näytteenottimen vastaanottaa HTTP 200-vastauksen takaisin näytteenottimen polku. 200 –-vastauksen on virhe.
- 30 x uudelleenohjaus epäonnistuu, vaikka uudelleenohjatun URL-osoite palauttaa 200.
- HTTPs-keräysputkien varmenteen virheitä oteta huomioon.
- Näytteenottimen polun asiakirjan sisältöä ei ole väliä, kunhan 200 palautetaan. Tutkitaan URL-osoite, joitakin staattiseksi sisällöksi "/ favicon.ico" on yleisiä tekniikka. Dynaamisen sisällön, kuten ASP-sivuja ei voi palauttaa 200 aina, myös silloin, kun sovellus on kunnossa.
- Paras käytäntö on määritetty näytteenottimen polku, jossa on tarpeeksi logiikan määrittää, että sivusto on ylös tai alas. Edellisessä esimerkissä, määrittämällä polku "/ favicon.ico", vain kyseisen w3wp.exe testaaminen vastaa. Tämä sondi saattaa tarkoita, että web-sovelluksen on kunnossa. Parempi vaihtoehto on määritetty polku kuten "/ Probe.aspx", joka sisältää sivuston terveydentilasta logiikan. Voit esimerkiksi käyttää suorituskyvyn laskureita suorittimen käyttö tai mittaa epäonnistuneiden pyyntöjen määrä. Tai voit yrittää käyttää tietokannan resurssit tai varmista, että web-sovellus toimii istunnon tila.
- Jos kaikki päätepisteet profiilissa on heikentynyt, valitse liikenteen hallinta käsittelee kaikki kunnossa kuin päätepisteet ja liikenne ohjautuu sen vuoksi kaikki päätepisteiden. Näin varmistat, että kokeillut järjestelmän ongelmia eivät aiheuta valmis käyttökatkosta palvelun.

## <a name="troubleshooting"></a>Vianmääritys

Vianmääritys näytteenottimen epäonnistui, tarvitset työkalua, joka näyttää HTTP-tilakoodin palautuksen näytteenottimen URL-osoitteesta. On useita työkaluja, jotka näyttävät raaka HTTP-vastaus.

* [Fiddler](http://www.telerik.com/fiddler)
* [Kääntö](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Lisäksi voit tehdä F12 virheenkorjaus Työkalut Verkko-välilehdessä Internet Explorerissa HTTP-vastausten tarkasteleminen.

Tässä esimerkissä haluat nähdä Microsoftin näytteenottimen URL-Osoitteen vastaus: http://watestsdp2008r2.cloudapp.net:80/näytteenottimen. Seuraavassa PowerShell-esimerkissä havainnollistetaan ongelma.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Esimerkki tulos:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Ilmoitus on vastaanotettu uudelleenohjaus vastausta. Ilmoitetulla aiemmin, minkä tahansa StatusCode muu kuin 200 pidetään epäonnistui. Liikenteen hallinta muuttuu päätepisteen tila offline-tilassa. Voit ratkaista ongelman ottamalla Tarkista sivuston määritykset varmistaa, että oikea StatusCode palauttaa näytteenottimen polku. Määritä uudelleen siten, että polku, joka palauttaa 200 keräysputken liikenteen hallinta.

Jos yhteyttä näytteenottimen käyttää HTTPS-protokollan, joudut ehkä käytöstä varmenteiden tarkistusta voit välttää virheet SSL/TLS testauksessa aikana. Seuraavat PowerShell-lausekkeet käytöstä varmenteen vahvistus PowerShell-istunnon:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Seuraavat vaiheet

[Tietoja liikenteen hallinta liikenne reititys menetelmät](traffic-manager-routing-methods.md)

[Mikä on liikenteen hallinta](traffic-manager-overview.md)

[Pilvipalveluihin](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps-sovelluksista](https://azure.microsoft.com/documentation/services/app-service/web/)

[Toiminnot-liikenteen hallinta (REST API hakemisto)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure liikenteen hallinnan cmdlet-komennot][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx

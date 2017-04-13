<properties
    pageTitle="Käyttää avainta säilö palomuurin takana | Microsoft Azure"
    description="Opettele käyttämään avaimen säilö sovelluksesta palomuurin takana"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Käytettäessä palomuurin takana avaimen säilö
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>Kysymys avaimen säilö asiakassovellus on oltava palomuurin takana, mitä portit ja isännät/IP-osoitteita kannattaa avata käyttämiseksi avaimen säilö?

Voit käyttää keskeisiä säilö avaimen säilö asiakassovellus on voi käyttää useita merkitsemisperusteet eri toimintoja varten.

- Todennus (joko Azure Active Directory)
- Avaimen säilö (joka sisältää luominen, lukeminen ja Päivitä/poistaminen ja asetus käytäntöjen) – Azure Resurssienhallinta hallinta
- Käyttäminen ja hallinta (näppäimet ja tietoja) tallennettujen objektien avaimen säilö itse, käy läpi avaimen säilö tietyn loppupisteen-merkki (esimerkiksi [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Eri määritys ja ympäristössä on joitakin variaatiot.   

## <a name="ports"></a>Portit

Kaikki liikenne paikalliseen avaimen säilö, kaikki 3 Funktiot (todennus, hallinnasta ja tietoja-tasossa käytön) esitellään HTTPS: porttiin 443. Kuitenkin luettelon varten on joskus HTTP (80 portti)-liikenne. Asiakkaat, jotka tukevat OCSP ei kannata saavuttaa luettelon, mutta voi toisinaan saavuttaa [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Todennus

Avaimen säilö asiakassovellus on käyttää Azure Active Directory-päätepisteet todennusta varten. Käyttää päätepisteen määräytyy AAD vuokraajan määritykset ja lyhennys--käyttäjätunnus, palvelun lyhennys ja tilillä, kuten Microsoft Account tai Org-ID tyyppi tyyppi  

| Lyhennys tyyppi | Päätepisteen: portti |
|----------------|---------------|
| Käyttämällä Microsoft-Account käyttäjä<br> (kutenuser@hotmail.com) | **Yleistä:**<br> Login.microsoftonline.com:443<br><br> **Azure Kiinan:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US Government:**<br> Kirjaudu sisään us.microsoftonline.com:443<br><br>**Azure Saksa:**<br> Login.microsoftonline.de:443<br><br> ja <br>Login.live.com:443   |
| Käyttäjäpalvelu/lyhennys Organisaatiotunnus käyttäminen AAD (kutenuser@contoso.com) | **Yleistä:**<br> Login.microsoftonline.com:443<br><br> **Azure Kiinan:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US Government:**<br> Kirjaudu sisään us.microsoftonline.com:443<br><br>**Azure Saksa:**<br> Login.microsoftonline.de:443 |
| Käyttäjäpalvelu/lyhennys organisaatiokaavion tunnus + ADFS tai muita liitetyt päätepisteen avulla (kutenuser@contoso.com) | Yllä päätepisteet Organisaatiotunnus sekä ADFS tai muita liitetyt päätepisteet |

Muut mahdolliset monimutkaisia tilanteita, joissa on. Katso [Azure Active Directory käyttöoikeuksien Flow](/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory-integrointi sovelluksiin](/documentation/articles/active-directory-integrating-applications/) ja [Active Directory käyttöoikeuksien protokollat](https://msdn.microsoft.com/library/azure/dn151124.aspx) lisätietoja.  

## <a name="key-vault-management"></a>Avaimen säilö hallinta

Avaimen säilö hallinta (CRUD ja access-käytännön asetus) avaimen säilöön-asiakassovellukseen on käyttää Azure Resurssienhallinta päätepiste.  

| Toiminnon tyyppi | Päätepisteen: portti |
|----------------|---------------|
| Avaimen säilö ohjausobjektin tasolla toimintoja<br> kautta Azure resurssien hallinta | **Yleistä:**<br> Management.Azure.com:443<br><br> **Azure Kiinan:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US Government:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Saksa:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory-kaavion Ohjelmointirajapinta | **Yleistä:**<br> Graph.Windows.NET:443<br><br> **Azure Kiinan:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure US Government:**<br> Graph.Windows.NET:443<br><br> **Azure Saksa:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Avaimen säilö toiminnot

Kaikki avaimen säilö objektin (näppäimet ja tietoja) hallinta ja salauksen toimintojen avaimen säilö asiakas on käyttää avaimen säilö loppupisteen merkki. Avaimen säilö sijainnin mukaan eroaa päätepisteen DNS-liitettä. Avaimen säilö loppupisteen merkki on muoto: < säilö nimi >. < alue--dns--liitettä > alla olevassa taulukossa kuvatulla tavalla.  

| Toiminnon tyyppi | Päätepisteen: portti |
|----------------|---------------|
| Avaimen säilö toimintojen salauksen toimenpiteet näppäimet-luotu, lue ja Päivitä/poistaminen näppäimet ja tietoja, kuten määrittäminen ja Hae tunnisteet ja määritteet avaimen säilö objektien (näppäimet/tietoja)     | **Yleistä:**<br> &lt;säilö nimi&gt;. vault.azure.net:443<br><br> **Azure Kiinan:**<br> &lt;säilö nimi&gt;. vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;säilö nimi&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Saksa:**<br> &lt;säilö nimi&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-osoitealueet

Avaimen säilö palvelun puolestaan käyttää muita Azure resursseja, kuten PaaS infrastruktuuri, näin ollen ei ole mahdollista on määritetty IP-osoitteet, jotka on avaimen säilö päätepisteiden annetun milloin tahansa. Kuitenkin jos palomuurin tukee vain alueiden sitten Lue [Microsoft Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653) asiakirjan IP-osoite.   Todennuksen ja tunnistetiedot (Azure Active Directory)-sovelluksen on voi yhdistää päätepisteet kuvattu [todennuksen ja tunnistetiedot osoitteet](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos sinulla on kysyttävää avaimen säilö, tutustu [Azure avaimen säilö keskustelupalstat](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

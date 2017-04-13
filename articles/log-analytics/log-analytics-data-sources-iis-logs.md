<properties
   pageTitle="IIS Kirjaa lokin Analytics | Microsoft Azure"
   description="Internet Information Services (IIS) tallennetaan käyttäjän lokitiedostot, joka voi kerätä Log Analytics niitä.  Tässä artikkelissa käsitellään määrittäminen kokoelma IIS-lokit ja he luovat OMS säilössä tietueiden tietoja."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="iis-logs-in-log-analytics"></a>IIS Kirjaa lokin Analytics
Internet Information Services (IIS) tallennetaan käyttäjän lokitiedostot, joka voi kerätä Log Analytics niitä.  

![IIS-lokit](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>IIS: N määrittäminen lokit
Lokitiedoston Analytics Kerää hakusanat lokitiedostoja luodaan IIS-, joten sinun täytyy [määrittää kirjaaminen IIS](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics IIS-lokitiedostoja W3C-muodossa tallennettu tukee vain ja ei tue mukautettuja kenttiä tai IIS-Advanced kirjaaminen.  
Log Analytics kerää lokit alkuperäisen NCSA tai IIS-muodossa.

Määritä IIS-lokeja Log Analytics [Lokiasetukset Analytics-tiedot-valikko](log-analytics-data-sources.md#configuring-data-sources).  Ei määrityksiä vaaditaan vain valitsemalla **Muotoile kerääminen W3C IIS lokitiedostot**.

On suositeltavaa, että kun otat IIS log sivustokokoelman, kannattaa määrittää IIS log palauttaminen-asetusta jokaisessa palvelimessa.


## <a name="data-collection"></a>Tietojen kerääminen

Lokitiedoston Analytics kerää IIS lokimerkintöjä kunkin yhdistetyn lähde noin 15 minuuttia.  Agentti kirjataan jokaisen tapahtumalokin, se kerää sijaintia.  Jos agentti offline-tilassa, valitse loki Analytics kerää tapahtumat, mihin se viimeksi jäi, vaikka tapahtumat on luotu agentti ollessa offline-tilassa.


## <a name="iis-log-record-properties"></a>IIS log tietueen ominaisuudet-valintaikkunassa

IIS log tietueiden tyyppi on **W3CIISLog** ja sen ominaisuudet ovat seuraavassa taulukossa:

| Ominaisuus | Kuvaus |
|:--|:--|
| Tietokoneen | Tietokoneeseen, jossa tapahtuma on kerätty nimi. |
| cIP | Asiakkaan IP-osoite. |
| csMethod | Menetelmä, kuten GET tai POST pyynnön. |
| csReferer | Sivuston käyttäjän Seuratut linkin nykyiseen sivustoon. |
| csUserAgent | Asiakkaan selaimen tyyppi. |
| csUserName | Todennetun käyttäjän, joka käyttää palvelimen nimi. Anonyymit käyttäjät on merkitty yhdysmerkin. |
| csUriStem | Kohde on esimerkiksi verkkosivulle pyynnön. |
| csUriQuery | Kyselyn, jos asiakas yritti suorittaa. |
| ManagementGroupName | Operations Manager tekijöiden hallinta-ryhmän nimi.  Muiden tekijöiden tämä on AOI -\<työtilan tunnus\> |
| RemoteIPCountry | Asiakkaan IP-osoitteen maa. |
| RemoteIPLatitude | Asiakkaan IP-osoite leveyttä. |
| RemoteIPLongitude | Asiakkaan IP-osoite pituutta. |
| scStatus | HTTP-tilakoodin. |
| scSubStatus | Alitila virhekoodi. |
| scWin32Status | Windows-tilakoodi. |
| sIP | Verkkopalvelin IP-osoite. |
| SourceSystem  | OpsMgr |
| Urheilu | Asiakas-palvelimen porttiin yhteydessä. |
| sSiteName | IIS-sivuston nimi. |
| TimeGenerated | Päivämäärä ja kellonaika, tapahtuma on kirjautunut. |
| TimeTaken | Ajanjakso, pyynnön millisekunteina. |

## <a name="log-searches-with-iis-logs"></a>Lokitiedoston hakujen kanssa IIS-lokit

Seuraavassa taulukossa on eri esimerkkejä log kyselyitä, jotka hakevat IIS log tietueita.

| Kyselyn | Kuvaus |
|:--|:--|
| Tyyppi = IISLog | IIS-loki tietueet. |
| Tyyppi = IISLog EventLevelName = virhe | Kaikkien Windowsin tapahtumien virheen vakavuus. |
| Tyyppi = W3CIISLog & #124; Mittaa count() cIP mukaan | IIS Laske Kirjaudu tapahtumat asiakkaan IP-osoite. |
| Tyyppi = W3CIISLog csHost = "www.contoso.com" & #124; Mittaa count() csUriStem mukaan | IIS Laske Kirjaudu URL-osoitteen merkintöjen host www.contoso.com. |
| Tyyppi = W3CIISLog & #124; Mittaa Sum(csBytes) tietokoneen & #124; ensimmäiset 500000| Tavujen vastaanottanut IIS-tietokoneissa. |

## <a name="next-steps"></a>Seuraavat vaiheet

- Määritä lokin Analytics kerätä analyysi muista [tietolähteistä](log-analytics-data-sources.md) .
- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten.
- Ilmoitusten määrittäminen ilmoittamaan, kun muutoksista tärkeitä ehdoista löydy IIS-lokeja Analytics loki.

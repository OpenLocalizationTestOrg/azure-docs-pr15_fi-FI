

<properties
   pageTitle="Kunnon seuranta yleiskatsaus Azure sovelluksen Gatewayn | Microsoft Azure"
   description="Tietoja Azure sovelluksen yhdyskäytävän seurantaa ohjelman"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>Sovelluksen yhdyskäytävän kunnon valvonta yleiskatsaus

Azure sovelluksen yhdyskäytävän oletusarvoisesti valvoo kaikkien resurssien taustatietokantaan sen sarjassa kuntoa ja poistaa automaattisesti kaikki resurssin pidettäviä perusasemassa olevilla. Sovelluksen yhdyskäytävän jatkuu seurannassa perusasemassa esiintymät, ja lisää ne takaisin kunnossa taustatietokantaan resurssivarantoon, kun ne ovat käytettävissä ja vastata kunto keräysputkien. Sovelluksen yhdyskäytävän lähettää kuntotietojen probes samaan porttiin, joka on määritetty taustatietokantaan HTTP-asetuksia. Näin varmistat, että näytteenottimen Testaa sama portti, asiakkaat olisi käyttäen muodostaa taustaan.

![sovelluksen yhdyskäytävän näytteenottimen Esimerkki][1]

Lisäksi oletusarvon terveyden näytteenottimen seurantaan, voit mukauttaa kunto keräysputken sovelluksen vaatimukset sopivaksi. Tässä artikkelissa käsitellään oletusarvoiset ja mukautetut kunto keräysputkien.

## <a name="default-health-probe"></a>Oletusarvoinen kunto näytteenottimen

Sovelluksen yhdyskäytävän määrittää oletusarvon kunto-näytteenottimen automaattisesti, kun määrität ei mukautetun näytteenottimen kokoonpanoa. Seurannan toiminnan toimii tekemällä HTTP-pyynnön IP-osoitteiden taustatietokantaan ryhmää varten määritettyä. Oletusarvoinen keräysputkien, jos Taustajärjestelmä http-asetukset on määritetty HTTPS-näytteenottimen käyttämällä https sekä testata kunto backends.

Esimerkki: Voit määrittää sovelluksen-yhdyskäytävän käyttämään taustatietokantaan palvelimia A, B ja C vastaanottamaan HTTP verkkoliikennettä porttiin 80. Kolme palvelinta oletusarvon kunnon valvonta Testaa kunnossa HTTP-vastaus 30 sekunnin välein. Kunnossa HTTP-vastaus on [tilakoodin](https://msdn.microsoft.com/library/aa287675.aspx) 200 – 399.

Jos oletusarvoinen näytteenottimen tarkistus epäonnistuu palvelimen A, sovelluksen yhdyskäytävän poistaa taustatietokantaan sen varannon ja verkkoliikenteen lopettaa juoksutus tähän palvelimeen. Oletus-keräysputken yhä tarkistaa palvelimen 30 sekunnin välein. Kun palvelimeen vastaa onnistuneesti yhden pyynnön oletusarvon kunto-näytteenotin, se lisätään takaisin kunnossa taustatietokantaan resurssivarantoon ja liikenne alkaa juoksutus palvelimeen uudelleen.

### <a name="default-health-probe-settings"></a>Kuntotietojen näytteenottimen oletusasetukset

|Tutkia ominaisuus | Arvo | Kuvaus|
|---|---|---|
| Tutkia URL-osoite| http://127.0.0.1:\<portti\>/ | URL-polku |
| Väli | 30 | Aikavälin tutkia sekunnin kuluttua |
| Aikakatkaisu  | 30 | Tutkia aikakatkaisu sekunnin kuluttua |
| Perusasemassa kynnysarvo | 3 | Tutkia Laske uudelleen. Taustatietokannan palvelimen on merkitty, kun peräkkäiset näytteenottimen virheen Laske saavuttaa perusasemassa raja-arvon. |

> [AZURE.NOTE] Portti on aina sama portti taustatietokantaan HTTP-asetukset.

Oletusarvoinen näytteenottimen tarkastelee vain http://127.0.0.1:\<portin\> Selvitä kunto tila. Jos haluat määrittää kunto keräysputken voit siirtyä mukautettua URL-Osoitetta tai muuttaa muita asetuksia, sinun on käytettävä mukautettu keräysputkien kuvatulla tavalla seuraavien ohjeiden mukaisesti.

## <a name="custom-health-probe"></a>Mukautetun kunto näytteenottimen

Mukautetun keräysputkien mahdollistavat eritellympiä päättää kunnon valvonta. Mukautetun keräysputkien käytettäessä voit määrittää näytteenottimen aikaväli, URL-osoite ja testaa polku ja kuinka monta epäonnistui vastaukset hyväksyä, ennen kuin taustatietokantaan resurssivarantoon esiintymän merkitään virheelliseksi merkitsemisen.

### <a name="custom-health-probe-settings"></a>Mukautetun kunto näytteenottimen asetukset

Seuraavassa taulukossa on mukautettu kunto näytteenottimen ominaisuuksien määritykset.

|Tutkia ominaisuus| Kuvaus|
|---|---|
| Nimi | Näytteenottimen nimi. Tätä nimeä käytetään viittaamaan keräysputken taustatietokantaan HTTP asetuksissa. |
| Protokolla | Lähetä näytteenottimen protokolla. Näytteenottimen käyttävät taustatietokantaa HTTP-asetuksissa määritetyn mukaisesti protokolla |
| Host (isäntä) |  Lähetä näytteenottimen isäntänimi. Käytettävissä vain, kun usean sivuston on määritetty sovelluksen yhdyskäytävässä, muuten käyttää "127.0.0.1". Tämä on sama kuin AM isäntänimi. |
| Polku | Suhteellinen polku näytteenottimen. Virheellinen polun alkaa "/". |
| Väli | Tutkia välin sekunteina. Tämä on kaksi peräkkäistä keräysputkien aikavälin.|
| Aikakatkaisu | Tutkia aikakatkaisun sekunteina. Näytteenottimen on merkitty ei onnistunut, jos vastausta ei vastaanoteta tämän aikakatkaisuajan kuluessa. |
| Perusasemassa kynnysarvo | Tutkia Laske uudelleen. Taustatietokannan palvelimen on merkitty, kun peräkkäiset näytteenottimen virheen Laske saavuttaa perusasemassa raja-arvon. |

> [AZURE.IMPORTANT] Jos sovelluksen yhdyskäytävä on määritetty yksittäisen sivuston, oletusarvoisesti isännän nimi on määritettävä "127.0.0.1", ellei muussa määritetty mukautettuja näytteenottimen.
Viittauksen mukautettu näytteenottimen lähetetään \<protokolla\>://\<host\>:\<portin\>\<polku\>. Portin on sama portti taustatietokantaan HTTP-asetuksissa määritetyn mukaisesti.

## <a name="next-steps"></a>Seuraavat vaiheet

Liittyviä sovelluksen yhdyskäytävän kunnon valvonta jälkeen voit määrittää [mukautetun kunto näytteenottimen](application-gateway-create-probe-portal.md) Azure portaalin tai [mukautetun kunto näytteenottimen](application-gateway-create-probe-ps.md) PowerShell ja Azure resurssien hallinnan käyttöönottomalli.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
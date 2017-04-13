<properties
   pageTitle="SSL-käytäntö ja pääty SSL-sovelluksen yhdyskäytävän | Microsoft Azure"
   description="Tällä sivulla on yleiskuvaus pääty SSL tukea sovelluksen yhdyskäytävän."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>SSL-käytäntö ja pääty SSL-sovelluksen yhdyskäytävän ottaminen käyttöön

## <a name="overview"></a>Yleiskatsaus

Sovelluksen gateway tukee SSL-tilauksen yhdyskäytävässä, kun liikenne yleensä jatkuu salaamattomana Taustajärjestelmä-palvelimiin. Tällöin on unburdened kallista salauksen katseltavan-web-palvelimiin. Kuitenkin joitakin asiakkaiden salaamaton tietoliikenteen taustaan-palvelimiin vaihtoehto ei ole oikein. Tämä voi johtua suojaus-ja yhteensopivuusvaatimukset tai sovelluksen voivat hyväksyä vain suojattua yhteyttä. Näiden sovellusten sovelluksen gateway tukee nyt pääty SSL-salausta.

Lopeta loppuun SSL avulla voit välittää turvallisesti luottamuksellisia tietoja kerroksen 7 kuormituksen ominaisuuksien edut Taustajärjestelmä salattuja edelleen ottaen hyödyntää mitä sovelluksen gateway tukee, eväste affiniteetti, kuten URL-pohjaiset reititys, reititys perustuu sivustoja tai mahdollisuus lisätä X-välitetyt-* otsikot.

Kun määritetty pääty SSL viestintä-tilassa, sovelluksen yhdyskäytävä katkaisee käyttäjän SSL-istuntojen yhdyskäytävässä ja purkaa käyttäjien tietoliikenne. Voit valita haluamasi Taustajärjestelmä resurssivarantoon esiintymä, reitti-liikenne paikalliseen määritetyn säännöt käytetään. Sovelluksen yhdyskäytävän sitten aloittaa uuden SSL-yhteys palvelimeen ja salaa uudelleen käyttämällä Taustajärjestelmä palvelimen julkisen avaimen sertifikaatti ennen lähettävän taustaan pyyntö tiedot. End, jos haluat lopettaa SSL on käytössä määrittämällä protokollan BackendHTTPSetting Https, jota käytetään sitten Taustajärjestelmä resurssivarantoon. Kunkin palvelimeen Taustajärjestelmä pääty SSL käytössä sarjassa on oltava määritettynä sertifikaatilla Salli suojattu tietoliikenne.

![lopusta loppuun ssl-skenaario][1]

Tässä esimerkissä pyyntöjen TLS1.2 reititetään Taustajärjestelmä palvelinten Pool1 pääty SSL-yhteys.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Lopeta SSL- ja whitelisting varmenteiden lopettaminen

Sovelluksen yhdyskäytävän yhteydessä vain tunnetut Taustajärjestelmä esiintymät, joissa on whitelisted niiden sovelluksen yhdyskäytävän varmenne. Varmenteiden whitelisting käyttöön täytyy ladata Taustajärjestelmä palvelimen sertifikaatit julkisella avaimella sovelluksen Gateway (ei pääkansion varmenne). Valitse vain tunnetut ja whitelisted backends yhteydet sallitaan. Yhdyskäytävän virhe jäljellä olevan backends tulokset. Itse allekirjoitetun varmenteet ovat vain ja ei suositella tuotannon työmääriä testaus. Todistusten on oltava myös whitelisted application Gatewayn kuvatulla tavalla edellä kuvatut toimet ennen kuin niitä voidaan käyttää.

## <a name="application-gateway-ssl-policy"></a>Sovelluksen yhdyskäytävän SSL-käytäntö

Sovelluksen gateway tukee määritettäviä SSL neuvottelun käyttäjäkäytännöt, jotka mahdollistavat asiakkaiden tarkemmin SSL-yhteyksien sovelluksen yhdyskäytävässä.

1. SSL 2.0 ja 3.0 oletusarvoisesti käytöstä kaikki sovelluksen yhdyskäytävät. Niitä ei voi muuttaa lainkaan.
2. SSL-käytännön määritys antaa asetus käytöstä jollakin seuraavista 3 protokollista - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Jos ei ole SSL-käytäntö määritetään kolme (TLSv1\_0, TLSv1\_1, TLSv1_2) ovat käytössä.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet tietoa end SSL ja SSL-käytäntö, siirry [pääty SSL: n käyttöön sovelluksen yhdyskäytävän](application-gateway-end-to-end-ssl-powershell.md) luomalla sovelluksen yhdyskäytävän lähettämistä liikenne backends salattuja lomakkeessa.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
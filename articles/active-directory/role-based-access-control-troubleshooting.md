<properties
    pageTitle="Roolipohjainen käytön hallinta vianmääritys | Microsoft Azure"
    description="Ohjeita seurantakohteita tai roolin perusteella käytönvalvonta resurssien koskeviin kysymyksiin."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Roolipohjainen käyttöoikeuksien valvonta vianmääritys

## <a name="introduction"></a>Johdanto

[Roolipohjainen käyttöoikeuksien valvonta](role-based-access-control-configure.md) on tehokas ominaisuus, jonka avulla voit delegoida Azure resurssien tarkasti rajattuja käyttöoikeuksia. Tällöin voit tuntuu varma myöntämistä tietyn henkilön oikeus käyttää täsmälleen tarvitsemansa ja enää. Kuitenkin joskus resurssin mallin Azure resurssien voi olla monimutkainen ja voi olla vaikeaa ymmärtää täsmälleen mitä myönnät käyttöoikeudet.

Tämän asiakirjan ilmoittaa, kun avulla roolit Azure-portaalissa toiminta. Nämä kolme roolia kattaa kaikki resurssityypit:

- Omistaja  
- Avustaja  
- Lukija  

Omistajat ja osallistujat on täydet oikeudet hallintatoimintojen, mutta osallistujan et voi antaa muille käyttäjille tai ryhmille. Asioita käyttöösi hieman mielenkiintoisemmaksi lukijaroolin kanssa niin, joka on, jossa on aikaa jonkin aikaa. Katso lisätietoja [Roolipohjainen käyttöoikeuksien valvonta get aloittaminen-artikkelissa](role-based-access-control-configure.md) käyttöoikeus.

## <a name="app-service-workloads"></a>Sovelluksen palvelun toiminnoista

### <a name="write-access-capabilities"></a>Kirjoita käyttömahdollisuudet

Jos annat käyttäjälle vain luku-tilassa yksittäisen web App-sovellukseen, jotkin toiminnot on poistettu käytöstä, että voisi olettaa ei. Seuraavat hallintaominaisuuksien edellyttävät **kirjoittaminen** access web App (osallistuja tai omistaja) ja ei voi käyttää vain luku-tilanne.

- Komentojen (esimerkiksi aloitus, Lopeta jne.)
- Yleisten määritysten, mittakaava-asetuksia, varmuuskopiointiasetuksia ja seurannan asetukset kuten asetusten muuttaminen
- Julkaisun tunnistetiedot ja muita tietoja, kuten sovelluksen asetusten ja yhteysmerkkijonon käyttäminen
- Streaming lokit
- Vianmäärityslokit määritys
- Console (komentokehote)
- Aktiivinen ja viimeisimmät käyttöönotoissa (paikallinen git jatkuva käyttöönottoa) varten
- Arvioitu käyttävät
- Web-testiä
- Virtuaalinen verkon (jotka vain jos virtual verkko on määritetty aiemmin käyttäjä, jolla kirjoitusoikeudet lukuohjelmaa).

Jos voi käyttää näitä ruudut, sinun on pyydettävä järjestelmänvalvojan avustaja access web App-sovellukseen.

### <a name="dealing-with-related-resources"></a>Aiheeseen liittyvät resurssit käsittely

Web Apps-sovellukset ovat monimutkaiselta muutamalla eri resurssien avulla vuorovaikutus tavoitettavuuden mukaan. Tässä on muutama sivustojen tyypillinen resurssiryhmä:

![Web app-resurssiryhmä](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Tuloksena, kun myönnät access vain web-sovellukseen, paljon Azure-portaalissa sivuston-sivu-toiminnot eivät ole käytettävissä.

Nämä kohteet tarvitaan, joka vastaa sivuston **App palvelusopimus** **kirjoittaa** access:  

- Web-sovelluksen tarkasteleminen on hinnat taso (vapaa tai vakio)  
- Skaalaa määritys (määrä esiintymät, virtuaalikoneen koon Automaattinen skaalaus-asetukset)  
- Kiintiön (tallennustilaa, kaistanleveyden-Suoritin)  

Nämä kohteet tarvitaan koko **resurssiryhmä** , joka sisältää sivuston **kirjoittaminen** access:  

- SSL-varmenteita ja sidontojen (tämä johtuu siitä SSL-varmenteita voidaan jakaa saman resurssiryhmä-sivustoissa ja geo sijainti)  
- Ilmoitusten säännöt  
- Automaattinen skaalaus-asetukset  
- Sovelluksen tiedot-osat  
- Web-testiä  

## <a name="virtual-machine-workloads"></a>Virtuaalikoneen toiminnoista

Paljon kuten web Apps-sovellusten kanssa virtuaalikoneen-sivu, jotkin ominaisuudet vaativat kirjoitusoikeudet virtuaalikoneen tai muita resursseja resurssiryhmän.

Näennäiskoneiden liittyvät toimialueen nimet, virtual verkkojen, tallennustilan tilit ja ilmoitusten säännöt.

Nämä kohteet tarvitaan **virtuaalikoneen** **kirjoittaminen** access:

- Päätepisteet  
- IP-osoitteet  
- Levyjen  
- Tunnisteet  

Nämä edellytä Accessia **kirjoittaa** **virtuaalikoneen**sekä **resurssiryhmä** (ja toimialuenimi), joka on:  

- Käytettävyys määrittäminen  
- Lataa tasapainoinen  
- Ilmoitusten säännöt  

Jos et voi käyttää mitä tahansa näistä, tarvitset Kysy järjestelmänvalvojaltasi resurssiryhmän avustaja-yhteyden.

## <a name="see-more"></a>Lisätietoja on ohjeaiheessa
- [Roolin perusteella käyttöoikeuksien valvonta](role-based-access-control-configure.md): Aloita RBAC Azure-portaalissa.
- [Valmiin rooleja](role-based-access-built-in-roles.md): roolit, joilla on tullut RBAC-tietoja.
- [Mukautettu roolien Azure RBAC](role-based-access-control-custom-roles.md): Lue, miten voit luoda mukautettuja rooleja access tarpeitasi.
- [Luo access muuttaa historia-kaavioraportin](role-based-access-control-access-change-history-report.md): seurantaan RBAC muuttaminen roolimäärityksiä.

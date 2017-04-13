
<properties
    pageTitle="Tallenna koskaan Azure RemoteApp luottamuksellisia tietoja mukautetun kuvista | Microsoft Azure"
    description="Lue lisää profiilikuva tiedot tallennetaan mukautetut Azure RemoteApp kuvat"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="never-store-sensitive-data-on-custom-images"></a>Älä tallenna arkaluontoisia tietoja mukautetun kuvista

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Jos isännöit omia Azure RemoteApp-sovelluksen, ensimmäiseksi kannattaa luoda mukautetun kuvan. Mukautetun näköistiedoston avulla luoda AM esiintymät, jotka vastaavat sovelluksia käyttäjille. Mukautetun kuvan pitäisi olla vain sovellukset ja koskaan luottamuksellisia tietoja, jotka voidaan menettää, kuten SQL-tietokantoja, henkilöstön tiedostoja tai erityinen datatiedostojen esimerkiksi QuickBooks yrityksen tiedostot. Luottamuksellisia tietoja tulisi sijaita ulkopuolisten Azure RemoteApp tiedostopalvelimessa, toinen Azure AM tai SQL Azure-tietokannassa. Kuva on vain ylläpitää sovellus, joka muodostaa yhteyden tietolähteeseen ja esittää tiedot. Tarkastele [Azure RemoteApp kuvien](remoteapp-imagereqs.md) lisätietoja. 

Selvittääksesi, miksi olisi Tallenna luottamuksellisia tietoja, joudut ymmärtää Azure RemoteApp toiminta. Kun kokoelma luodaan tai päivitetään taustalla useita kloonit tai kuvan luodaan. Kaikki nämä AM esiintymät ovat täsmälleen replikoita mukautetun kuvan; Kun käyttäjät käynnistää sovelluksia ne on yhdistetty johonkin näistä AM esiintymät. Mutta samassa esiintymässä ei välttämättä ja olisi ole merkitystä, koska ne ovat tilapäisten. AM esiintymät, isännöinnin sovellukset ovat tilapäisten ja voidaan hävitetään tai poistaa mukaan, esimerkiksi sivustokokoelman päivityksen aikana. 

Kun kokoelma on valmisteltu ja käyttäjät voivat aloittaa VMs muodostamisesta, käyttäjätiedot on jatkuva ja suojattu, koska se on tallennettu eri säilössä sisällä Näennäiskiintolevyn on kutsu [käyttäjäprofiilin levyn (UPD)](remoteapp-upd.md), joka on c:\users käyttäjäprofiilin\<userprofile >. Kun sovellus käynnistyy UPD on otettu käyttöön ja käsitellään tavoin paikallisen käyttäjäprofiilin käyttöjärjestelmä. Lue lisää siitä, miten [Azure RemoteApp tallentaa käyttäjätiedot ja asetukset](remoteapp-upd.md).

Esimerkkitiedot, jotka kannattaa ole kuvassa:

- Jaettuja tietoja, jotta käyttäjät voivat käyttää
- SQL-DB- tai QuickBooks DB
- D:\ tiedot

Esimerkkitiedot, jotka voivat sijaita oletusprofiilin, jos haluat kopioida kaikki käyttäjät UPD:

- Tiedostojen käyttäjää kohden
- Komentosarjoja, jotka käyttäjät on niiden UPD säilyvät

Yhteenveto:

- Älä koskaan kaupan luottamuksellisia tietoja, jotka voivat kadota kuva, kun luot mukautetun kuvan.
- Luottamuksellisten tietojen pitäisi aina sijaitsevat erilliseen tiedostoon-palvelimella, erillisessä Azure AM pilvipalveluun, ja aina ulkoisen isännöinnin sovellustesi sisällä Azure RemoteApp AM esiintymissä. 
- Käyttäjätiedot on tallennettu, ja se toistuu käyttäjän profiiliin levyn (UPD)



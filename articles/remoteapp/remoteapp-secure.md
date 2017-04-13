
<properties
    pageTitle="Suojattu sovellukset ja resurssien Azure RemoteApp | Microsoft Azure"
    description="Lue, miten voit lukita sovellukset ja Azure RemoteApp resurssit"
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



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Suojatun sovellukset ja Azure RemoteApp resurssit

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp tarjoaa käyttäjille käyttämään keskitetysti hallittuja Windows-sovelluksia, jolloin voit päättää, mitä käyttäjien voi tehdä.  Tämä on erityisen hyödyllinen, kun käyttäjä muodostaa ei-hallitun laitteesta (kuten niiden Omat Macbook) ja käytön hallinta tai kohdata.

Esimerkiksi jos käytät Active Directory käyttöoikeuksien ja haluat estää käyttäjiä kopioimasta tiedot sovelluksen ulos, voit määrittää Remote Desktop Ryhmäkäytäntö estä käyttäjiä kopioimasta tietoja.

Toinen esimerkki on, jos haluat estää kunkin sovelluksen kokoelmasta internet-yhteyttä. Voit luoda Windowsin palomuuri-sääntö, joka estää käyttöoikeudet, kun luot kuvan kokoelmasta.

## <a name="implementation-options"></a>Toteutusvaihtoehdot

  Seuraavassa on avaimen Toteutusvaihtoehdot, joita voidaan käyttää yksitellen tai Yritystietopalveluiden tarpeen mukaan:

1.  Jos RemoteApp-sivustokokoelman on liitetty voit pakottaa kaikki [Ryhmäkäytäntö](https://technet.microsoft.com/library/cc725828.aspx) (lukuun ottamatta vapaa- ja Katkaise yhteys aikakatkaisu käytännöt kuvattu [seuraavassa](../azure-subscription-service-limits.md)).
2.  Vaihtoehtoinen Ryhmäkäytäntö (Jos kokoelmaa ei ole liitetty toimialue tai ei ole siihen tarvittavat oikeudet AD) voit määrittää [Paikalliset käytännöt](https://technet.microsoft.com/library/cc775702.aspx) suunnittelumallin kuva kyselyjä.  Huomautus kyseisen ryhmän käytännöt valtti Paikalliset käytännöt, kun kyseessä on ristiriita.
3.  Käyttöjärjestelmä/app joitakin asetuksia ei voi määrittää käytännön kautta, mutta se voi olla kautta rekisteriavain määritettäessä suunnittelumallin kuva [RegEdit-työkalun](./remoteapp-hybridtrouble.md) avulla.
4.  [Windowsin palomuuri](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) avulla voit hallita verkossa, ja tietokoneesta, jossa sovellus on käynnissä. Varmista juuri ei estä URL- ja määritetty seuraavat portit.
5.  Voit määrittää, mitkä sovellukset ja tiedostoja käyttäjät voidaan suorittaa [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) . Esimerkiksi joulunajan käyttäjät voivat selvittää suorittaminen sovellusten ei julkaista mutta, jotka ovat käytettävissä kuvan kokoelman luomiseen käytetyt - AppLocker voi estää.

## <a name="detailed-information"></a>Yksityiskohtaiset tiedot

- Seuraavia RDS käytäntöjä on todennäköisesti hyötyä:
    - [Laitteen ja resurssin uudelleenohjaus](https://technet.microsoft.com/library/ee791794.aspx)
    - [Tulostimen uudelleenohjaus](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profiilit](https://technet.microsoft.com/library/ee791865.aspx).
- Huomaa, että määrittäminen uudelleenohjausta RemoteApp PowerShellin kautta moduuli (kuten näkyy [tähän](./remoteapp-redirection.md)) riippuvainen voidaan valvoa käytäntö, joten jos suojausta ensisijaisen tavoitteen kannattaa Pakota käytännön mallin kuva paikallisen käytännön kautta tai ryhmäkäytännön asiakaskoneeseen.
- [Windows Server 2012 R2 käytännöt](https://technet.microsoft.com/library/hh831791.aspx).
- [Office 2013: n käytännöt](https://technet.microsoft.com/library/cc178969.aspx) (mukaan lukien [mukauttamisesta Office-painiketta](https://technet.microsoft.com/library/cc179143.aspx)).

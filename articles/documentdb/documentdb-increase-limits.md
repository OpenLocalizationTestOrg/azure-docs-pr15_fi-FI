<properties
    pageTitle="Pyyntö entistä DocumentDB tilin kiintiön | Microsoft Azure"
    description="Lue, miten pyytää DocumentDB tietokannan kiintiöitä, kuten asiakirjojen säilyttäminen ja siirtonopeuden kokoelman muutos."
    services="documentdb"
    authors="AndrewHoh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter=""/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="anhoh"/>

# <a name="request-increased-documentdb-account-limits"></a>Pyyntö entistä DocumentDB rajoitukset

[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) on oletusarvo, joka voidaan säätää ottamalla yhteyttä tukipalveluun Azure kiintiöiden.  Tässä artikkelissa kerrotaan, miten pyytää kiintiön Suurenna.

Luettuasi tämän artikkelin pystyt seuraaviin kysymyksiin:  

-   Mitä DocumentDB tietokannan kiintiön voidaan säätää ottamalla yhteyttä tukipalveluun Azure?
-   Miten DocumentDB tilin kiintiö on muutos voit pyytää?

##<a id="Quotas"></a>DocumentDB tilin kiintiön

Seuraavassa taulukossa on kuvattu DocumentDB kiintiön. Tähdellä (*) merkittyjä kiintiön voidaan säätää ottamalla yhteyttä tukipalveluun Azure:

[AZURE.INCLUDE [azure-documentdb-limits](../../includes/azure-documentdb-limits.md)]


##<a id="RequestQuotaIncrease"></a>Kiintiön muutoksen pyytäminen
Seuraavissa vaiheissa kuvataan pyytämisestä kiintiön muutokset.

1. [Azure portal](https://portal.azure.com)valitsemalla **Lisää palveluja**ja valitse sitten **Ohje + tuki**.

    ![Näyttökuva avaamista Ohje ja tuki](media/documentdb-increase-limits/helpsupport.png)

2. Valitse **Uusi tue pyyntöä** **Ohjeen + tuki** -sivu.

    ![Näyttökuva tuki lippu luominen](media/documentdb-increase-limits/getsupport.png)

3. Valitse **Uusi tue pyyntöä** , sivu **perusteet**. Seuraava, Määritä **ongelman tyyppi** **kiintiön**, **tilauksen** tilaukseen, joka isännöi oman DocumentDB tilin **DocumentDB** **kiintiön tyyppi** ja **Kiintiön tuki - levy** **tuki suunnitelma** . Valitse sitten **Seuraava**.

    ![Näyttökuva tuki lipun pyynnön tyyppi](media/documentdb-increase-limits/supportrequest1.png)

4. Valitse **ongelma** -sivu vakavuus ja Sisällytä tietoja kiintiön Suurenna **tiedot**. Valitse **Seuraava**.

    ![Näyttökuva tuki lipun tilauksen valitsin](media/documentdb-increase-limits/supportrequest2.png)

5. Lopuksi täytä yhteystietoja **Yhteystiedot** -sivu ja valitse **Luo**.

Kun tuki-lippu on luotu, näyttöön tulee tuki pyynnön numero sähköpostitse.  Voit tarkastella tukipyynnön myös valitsemalla **Hallitse tuki pyytää** **apua + tuki** -sivu.

![Näyttökuva tuki pyytää sivu](media/documentdb-increase-limits/supportrequest4.png)


##<a name="NextSteps"></a>Seuraavat vaiheet
- Saat lisätietoja DocumentDB napsauttamalla [tätä](http://azure.com/docdb).

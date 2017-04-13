<properties 
    pageTitle="Ammattimainen Azure Redis välimuistin | Microsoft Azure"
    description="Lue, miten voit suorittaa hallintatoimia, kuten uudelleen ja aikataulun päivitykset Azure Redis välimuisti"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Azure Redis välimuistin hallinnasta

Tässä ohjeaiheessa kerrotaan, miten voit tehdä hallintatehtäviä, kuten uudelleenkäynnistyksen ja päivitysten Azure Redis välimuistin esiintymien ajoittaminen.

>[AZURE.IMPORTANT] Tässä artikkelissa kuvattuja ominaisuuksia ja asetukset ovat käytettävissä vain Premium taso tallentaa.


## <a name="administration-settings"></a>Hallinta-asetukset

Azure Redis välimuistin **hallinnan** asetusten avulla voit suorittaa seuraavat hallintatehtävät premium välimuistin. Voit käyttää hallinta-asetukset valitsemalla **tai **kaikkia asetuksia** ** Redis välimuistin sivu ja vieritä kohtaan **hallinnan** **asetukset** -sivu.

![Hallinta](./media/cache-administration/redis-cache-administration.png)

-   [Käynnistä tietokone uudelleen](#reboot)
-   [Päivitysten ajoittaminen](#schedule-updates)

## <a name="reboot"></a>Käynnistä tietokone uudelleen

**Käynnistä uudelleen** -sivu antaa vähintään yksi solmut välimuistin uudelleen. Voit testata vikasietoisuudelle, jos sovellus.

![Käynnistä tietokone uudelleen](./media/cache-administration/redis-cache-reboot.png)

Jos sinulla on premium-välimuistin asentaminen käytössä, voit valita mitkä shards välimuistin käynnistää uudelleen.

![Käynnistä tietokone uudelleen](./media/cache-administration/redis-cache-reboot-cluster.png)

Käynnistämään vähintään yksi solmut välimuistin Valitse haluamasi solmut ja sitten **uudelleen**. Jos sinulla on premium-välimuistin asentaminen otettu käyttöön, valitse shard(s) Käynnistä uudelleen ja valitse sitten **uudelleen**. Muutaman minuutin kuluttua valitun solmut uudelleen ja ovat takaisin online-muutama minuutti myöhemmin.

Asiakassovellukset vaikutus vaihtelee sen mukaan, kun käynnistät solmut.

-   **Perustyyli** : kun perusmuodon solmu käynnistetään Azure Redis välimuistin ei onnistu replikan solmu päällä, ja edistää perustyyliin. Tämä automaattisesti aikana saattaa olla lyhyt aikaväli, joka saattaa epäonnistua yhteydet välimuistiin.
-   **Toissijaisen** – kun toissijaisen solmun käynnistetään, ei yleensä ole vaikutusta välimuistin asiakkaille.
-   **Perustyylisivut sekä toissijaisen** - välimuistin molemmat solmut on käynnistettävä, kaikki tiedot ole käytettävissä välimuistin ja yhteyksiä välimuistin epäonnistuvat, kunnes ensisijainen solmu on online-tilaan. Jos olet määrittänyt [tietojen pysyvyyttä](cache-how-to-premium-persistence.md), uusin varmuuskopio palautetaan välimuistin tullessa online-tilaan. Huomaa, että kaikki välimuistin kirjoituksia viimeisimmän varmuuskopioinnin jälkeen on tapahtunut menetetään.
-   **Solmut premium-välimuistin asentaminen käytössä** – kun käynnistät premium-välimuistin solmut asentaminen käytössä toiminta on sama kuin, kun käynnistät solmut ei ole liitetty välimuistin.


>[AZURE.IMPORTANT] Käynnistä on käytettävissä vain Premium taso tallentaa.

## <a name="reboot-faq"></a>Käynnistä tietokone uudelleen usein kysytyt kysymykset

-   [Solmun Testaa sovelluksessa olisi uudelleen?](#which-node-should-i-reboot-to-test-my-application)
-   [Poista asiakasyhteydet välimuistin uudelleen](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Voin tiedot menetetään Omat välimuistista jos minulla uudelleen?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Oma välimuistin PowerShell, CLI tai muiden hallintatyökalujen käyttäminen uudelleen](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Mitä hinnoittelua tasoa voi käyttää uudelleen-toiminto?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Solmun Testaa sovelluksessa olisi uudelleen?

Testaa vikasietoisuudelle välimuistin ensisijainen solmu mukaan vika sovelluksesi uudelleen **perustyyli** -solmu. Voit esikatsella sovelluksen mukaan vika toissijaisen solmun vikasietoisuudelle-uudelleen **toissijaisen** solmun. Voit esikatsella sovelluksesi vastaan yhteensä Virhe välimuistin vikasietoisuudelle-uudelleen **molemmat** solmut.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Poista asiakasyhteydet välimuistin uudelleen

Kyllä, jos käynnistät välimuistin kaikki asiakasyhteydet ole valittuina. Näin voit hyötyä kaikki asiakasyhteydet käytettyjen, kuten logiikkavirhe tai asiakassovelluksessa ohjelmavirhe kirjoitusmuotoon. Hinnoittelu kunkin tason on eri [asiakkaan yhteysrajoitukset](cache-configure.md#default-redis-server-configuration) eri kokoja, ja kun nämä raja-arvot saavutetaan, ei lisää yhteyksien hyväksytään. Tapa poistaa kaikki asiakasyhteydet uudelleenkäynnistyksen välimuistin on.

>[AZURE.IMPORTANT] Jos asiakasyhteyksien käytetään vuoksi logiikkavirhe tai asiakas-koodisi ohjelmavirhe, Huomaa, että StackExchange.Redis automaattisesti uudelleen, kun Redis.txt-solmu on online-tilaan. Jos pohjana ongelma ei ratkennut, asiakas-yhteydet säilyvät voidaan käyttää loppuun.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Voin tiedot menetetään Omat välimuistista jos minulla uudelleen?

Jos käynnistät **pää** - ja **toissijaisen** solmujen välimuistin (tai, jos käytössäsi on premium-välimuistin asentaminen käytössä shard) kaikki tiedot menetetään. Jos olet määrittänyt [tietojen pysyvyyttä](cache-how-to-premium-persistence.md), uusin varmuuskopio palautetaan välimuistin tullessa online-tilaan. Huomaa, että kaikki välimuistin kirjoituksia, joka on tapahtunut sen jälkeen, kun varmuuskopio menetetään.

Jos käynnistät vain yksi solmut-tiedot eivät ole yleensä kadonneen, mutta se edelleen saattaa olla. Jos esimerkiksi jos perusmuodon solmu käynnistetään ja välimuistin kirjoitus on käynnissä, välimuistin kirjoittaa tiedot menetetään. Tietojen menetyksen toiseen skenaario olisi, jos käynnistät yksi solmu ja toisen solmun tapahtuu samanaikaisesti virheen vuoksi taaksepäin. Saat lisätietoja mahdollisista syistä tietojen menettämisen [mitä on tapahtunut Omat tiedot Redis.txt?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Oma välimuistin PowerShell, CLI tai muiden hallintatyökalujen käyttäminen uudelleen

Kyllä, PowerShell ohjeita ohjeaiheessa [Redis.txt välimuistin uudelleen](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Mitä hinnat tasoa voi käyttää uudelleen-toiminto?

Käynnistä on käytettävissä vain premium hinnat taso.

## <a name="schedule-updates"></a>Päivitysten ajoittaminen

Voit määrittää ylläpito-ikkuna, jossa välimuistin **päivitysten ajoittaminen** -sivu. Kun ylläpito-ikkuna on määritetty, Redis.txt palvelimen päivityksiä aikaiset tässä ikkunassa. Huomaa, että ylläpitovalintaikkunassa koskee vain Redis palvelimen päivitykset- ja minkä tahansa Azure ei voi päivittää tai päivitykset, jotka isännöivät välimuistin VMs käyttöjärjestelmä.

![Päivitysten ajoittaminen](./media/cache-administration/redis-schedule-updates.png)

Voit määrittää ylläpito-ikkuna, tarkista haluamasi päivän ja määritä kunkin ylläpito ikkunan aloituspäivämäärä ja valitse **OK**. Huomaa, että ylläpito ikkunan aika on UTC-muodossa. 

>[AZURE.NOTE] Päivitykset oletusarvon ylläpito-ikkuna on viiden tunnin. Tämä arvo ei ole määritettäviä Azure-portaalista, mutta voit määrittää sen PowerShellin avulla `MaintenanceWindow` [Uusi AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) -cmdlet-parametri. Lisätietoja on artikkelissa [voin hallita PowerShell sekä CLI ja muut hallintatyökalut ajoitetut päivitykset?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Ajoita päivitykset usein kysytyt kysymykset

-   [Kun päivitykset ilmenee jos käytän ei aikataulun päivitykset-toiminto?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Minkä tyyppisiä päivitykset tehdään suunniteltu ylläpitovalintaikkunassa aikana?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Voinko hallitun PowerShell sekä CLI ja muut hallintatyökalut ajoitetut päivitykset?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Mitä hinnat tasoa, voit käyttää aikataulun päivitykset-toimintoa?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Kun päivitykset ilmenee jos käytän ei aikataulun päivitykset-toiminto?

Jos et määritä ylläpito-ikkuna, päivitykset voidaan tehdä milloin tahansa.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Minkä tyyppisiä päivitykset tehdään suunniteltu ylläpitovalintaikkunassa aikana?

Vain Redis palvelimen päivitykset tehdään aikana suunniteltu ylläpito-ikkuna. Ylläpito-ikkuna ei koske Azure päivitykset tai päivitykset AM-käyttöjärjestelmään.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Voinko hallitun PowerShell sekä CLI ja muut hallintatyökalut ajoitetut päivitykset?

Voit hallita ajoitetun Päivityksesi käyttämällä PowerShell cmdlet-komentoja.

-   [Hae AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Uusi AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Uusi AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Poista AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Mitä hinnat tasoa, voit käyttää aikataulun päivitykset-toimintoa?

Ajoita päivitykset on käytettävissä vain premium hinnat taso.

## <a name="next-steps"></a>Seuraavat vaiheet

-   Tutustu muihin [Azure Redis välimuistin premium taso](cache-premium-tier-intro.md) -ominaisuuksiin.






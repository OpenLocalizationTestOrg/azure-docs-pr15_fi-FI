<properties
    pageTitle="Järjestelmän päivityksen arviointi ratkaisu Log Analytics | Microsoft Azure"
    description="Voit Log Analytics päivityksiä järjestelmä-ratkaisun auttaa puuttuvien päivitysten infrastruktuurin palvelimiin."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>Järjestelmän päivityksen arviointi ratkaisu Log Analytics

Voit Log Analytics päivityksiä järjestelmä-ratkaisun auttaa puuttuvien päivitysten infrastruktuurin palvelimiin. Kun olet asentanut ratkaisun, voit tarkastella päivitykset, joita ei ole valvottu palvelimien **Järjestelmässä päivitys arviointi** -ruutua käyttämällä OMS **Yleiskatsaus** -sivulla.

Jos puuttuvat päivitykset löytyy, tiedot näytetään **päivitykset** Raporttinäkymät-ikkunan. Voit **päivittää** Raporttinäkymät-ikkunan käyttäminen puuttuvat päivitykset ja suunnitteleminen koske palvelimissa, jotka tarvitset niitä.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Lisää järjestelmässä päivitys arviointi-ratkaisun OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.

## <a name="system-update-data-collection-details"></a>Järjestelmän tiedot sivustokokoelman päivityksestä

Järjestelmässä päivitys arviointi kerää metatiedot- ja tilan tietoja, jotka on otettu käyttöön tekijöiden avulla.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten tiedot kerätään järjestelmässä päivitys arviointia varten.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Kyllä](./media/log-analytics-system-update/oms-bullet-green.png)|![Kyllä](./media/log-analytics-system-update/oms-bullet-green.png)|![Ei](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Ei](./media/log-analytics-system-update/oms-bullet-red.png)|![Kyllä](./media/log-analytics-system-update/oms-bullet-green.png)| Vähintään 2 kerrotaan luvulla per päivä- ja 15 minuutin päivityksen asentamisen jälkeen|

Seuraavassa taulukossa on esimerkkejä järjestelmässä päivitys arviointi keräämiä tietotyypit:

|**Tietotyyppi**|**Kentät**|
|---|---|
|Metatietojen|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP-osoite, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-osoite, NetbiosDomainName, LogicalProcessors, DNSName, näyttönimi, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Tila|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekstissa, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Päivitykset-käyttöä varten

1. Valitse **sivulla** Napsauta **Järjestelmässä päivitys arviointi** -ruutua.  
    ![Järjestelmässä päivitys arviointi-ruutu](./media/log-analytics-system-update/sys-update-tile.png)
2. Voit tarkastella **päivitykset** Raporttinäkymät-ikkunan päivityksen luokat.  
    ![Päivitykset Raporttinäkymät-ikkunan](./media/log-analytics-system-update/sys-updates02.png)
3. Vieritä oikealle sivun tarkasteleminen **Windows kriittinen/turvapäivitykset** -sivu ja valitse sitten **luokittelu**, valitse **Päivitykset**.  
    ![Päivitykset](./media/log-analytics-system-update/sys-updates03.png)
4. Log-hakusivun erilaisia tietoja näkyy suojaus, löytynyt ole palvelinten infrastruktuurin päivityksistä. Valitse **luettelosta** lisätietojen päivitykset.  
    ![Hakutulokset - luettelossa](./media/log-analytics-system-update/sys-updates04.png)
5. Log-haku-sivulla näkyy jokaisen päivityksen yksityiskohtaisia tietoja. Viereen KBID valitsemalla **Näytä** Näytä vastaavan on artikkelissa Microsoftin tukisivustossa.  
    ![Kirjaudu hakutulokset - näkymän kt](./media/log-analytics-system-update/sys-updates05.png)
6. Uudessa välilehdessä päivityksen Microsoftin Web-sivu avautuu selaimeen. Tarkastele päivityksen, joka ei ole tietoja.  
    ![Microsoft Support web-sivu](./media/log-analytics-system-update/sys-updates06.png)
7. Käyttämällä käyttämällä tietoja olet löytänyt, voit luoda manuaalisesti puuttuvat päivityksen suunnitelma tai voit jatkaa annettuja ohjeita automaattisesti päivityksen jälkeen.
8. Jos haluat käyttää automaattisesti puuttuu päivitys, palaa **päivitykset** Raporttinäkymät-ikkunan ja valitse sitten **Päivitä suoritetaan**, valitse **napsauttamalla voit ajoittaa Suorita päivitys**.  
    ![Päivitä suoritetaan - ajoitetun välilehti](./media/log-analytics-system-update/sys-updates07.png)
9. Valitse **ajoitettu** -välilehden **Päivitä suoritetaan** -sivulla **Lisää** voit luoda uuden päivityksen suorittaminen.  
    ![Ajoitettu välilehti - Lisää](./media/log-analytics-system-update/sys-updates08.png)
10. **Uusi Suorita päivitys** sivulla-ruutuun Suorita päivitys nimi, Lisää yksittäiset tietokoneet tai tietokoneryhmät, Määritä aikataulu ja valitse sitten **Tallenna**.  
    ![Uuden päivityksen suorittaminen](./media/log-analytics-system-update/sys-updates09.png)
11. Suorita uusi päivitys, joka **Päivittää suoritetaan** -sivulla näkyy **ajoitettu** -välilehden voit olet varattu.  
    ![Aikataulun mukainen välilehti](./media/log-analytics-system-update/sys-updates10.png)
12. Suorita päivitys käynnistyessä, näet sen tiedot **käynnissä** -välilehdessä.  
    ![Käynnissä olevan välilehden](./media/log-analytics-system-update/sys-updates11.png)
13. Kun Suorita päivitys on valmis, **Valmis** -välilehdessä näkyy tila.
14. Jos päivityksiä on käytetty Update, suorita **Windows kriittinen/turvapäivitykset** -sivu tulee päivitysten määrä vähenee.  
    ![Windows kriittinen/turvapäivitykset sivu - päivityksen Laske rajoitetun](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Seuraavat vaiheet

- [Etsi lokit](log-analytics-log-searches.md) järjestelmän yksityiskohtaisia Päivitä tiedot.

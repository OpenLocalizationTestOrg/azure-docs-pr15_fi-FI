<properties
    pageTitle="Määritysten hallinta yhdistäminen Log Analytics | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan vaihe vaiheelta, miten hallintatoiminto yhdistäminen Log Analytics ja Aloita tietojen analysoiminen."
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
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Yhteyden muodostaminen Log Analytics määritysten hallinta

System Center määritysten hallinta voit muodostaa Log Analytics-OMS synkronoinnin laitteen sivustokokoelman tietoihin. Näin tietoja määritysten hallinta-käyttöönoton OMS käytettävissä.

On useita hallintatoiminto muodostaa OMS, joten tämä on nopea rundown yleistä prosessin vaiheita:

1. Azure hallinta-portaalissa Rekisteröi määritysten hallinnan Web-sovelluksen ja/tai verkko-Ohjelmointirajapinnan sovellus ja varmista, että Asiakastunnus ja asiakkaan salausavaimen rekisteröinnin Azure Active Directorysta. Katso tämän vaiheen suorittamiseen yksityiskohtaisia tietoja [Active Directory-sovelluksen ja palvelun pääasiallista, joka käyttää resursseja portaalin käyttöä](../resource-group-create-service-principal-portal.md) .
2. Azure hallinta-portaalissa, [Anna hallintatoiminnon (rekisteröity web Appissa) sekä OMS käyttöoikeudet](#provide-configuration-manager-with-permissions-to-oms).
3. Toimi määritysten hallinnassa [Lisää käyttämällä ohjattua OMS yhteyden Lisää yhteys](#add-an-oms-connection-to-configuration-manager).
4. Määritysten hallinta voit [päivittää yhteyden ominaisuudet](#update-oms-connection-properties) Jos salasana tai asiakkaan salausavaimen koskaan vanhenee tai menetetään.
5. Tiedot OMS-portaalista, [Lataa ja asenna Microsoft Agent seuranta](#download-and-install-the-agent) tietokoneessa käynnissä määritysten hallinta-palvelun yhteyden osoita sivuston järjestelmän rooli. Agentti lähettää hallintatoiminto OMS.
6. Valitse OMS, tietokoneen ryhminä [Tuo sivustokokoelmat määritysten hallinta](#import-collections) .
7. Tarkastele tietoja määritysten hallinta OMS, [tietokoneen](log-analytics-computer-groups.md)ryhminä.

Voit lukea lisää tietoja yhteyden muodostamisesta hallintatoiminto OMS tietojen [synkronointi-määritysten hallinta ohjelmistopakettiin Microsoft toimintojen hallinta](https://technet.microsoft.com/library/mt757374.aspx).



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Anna määritysten hallinta käyttöoikeuksien avulla OMS

Seuraavassa on Azure hallinta-portaalin kanssa OMS käyttöoikeudet. Tarkemmin sanottuna *osallistujan rooli* on myönnettävä resurssiryhmän käyttäjille. Puolestaan, joka sallii hallintatoiminto muodostaa OMS Azure hallinta-portaalin.

>[AZURE.NOTE] Määritä oikeudet OMS määritysten hallinta. Muussa tapauksessa tulee virhesanoma, kun käytät ohjattu määritystoiminto määritysten hallinta.


1. Avaa [Azure-portaaliin](https://portal.azure.com/) ja valitse **Selaa** > **Lokin Analytics (OMS)** Avaa loki Analytics (OMS)-sivu.  
2. Valitse **Log Analytics (OMS)** -sivu, **Lisää** Avaa **OMS työtila** -sivu.  
  ![OMS-sivu](./media/log-analytics-sccm/sccm-azure01.png)
3. **OMS työtila** -sivu, anna seuraavat tiedot ja valitse sitten **OK**.
  - **OMS työtila**
  - **Tilauksen**
  - **Resurssiryhmä**
  - **Sijainti**
  - **Hinnat taso**  
    ![OMS-sivu](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Edellä oleva esimerkkilauseke Luo uusi resurssiryhmä. Resurssiryhmä käytetään vain antaa hallintatoiminto OMS työtilan tässä esimerkissä käyttöoikeuksia.

4. Valitse **Selaa** > **resurssiryhmät** Avaa **resurssiryhmät** -sivu.
5. Valitse **resurssiryhmät** -sivu, jonka loit edellä Avaa resurssiryhmä &lt;resurssiryhmän nimi&gt; asetukset-sivu.  
  ![resurssin ryhmän asetukset-sivu](./media/log-analytics-sccm/sccm-azure03.png)
6. - &lt;Resurssiryhmän nimi&gt; asetukset-sivu, valitse käytönvalvonta (IAM), voit avata &lt;resurssiryhmän nimi&gt; käyttäjät-sivu.  
  ![resurssin ryhmän käyttäjät-sivu](./media/log-analytics-sccm/sccm-azure04.png)  
7. Valitse &lt;resurssiryhmän nimi&gt; käyttäjät-sivu valitsemalla **Lisää** Avaa **Lisää access** -sivu.
8. Valitse **Lisää access** -sivu valitsemalla **Valitse rooli**ja valitse sitten **osallistujan** rooli.  
  ![Valitse rooli](./media/log-analytics-sccm/sccm-azure05.png)  
9. Valitse **Lisää käyttäjiä**, valitse määritysten hallinta-käyttäjä, **Valitse**ja valitse sitten **OK**.  
  ![käyttäjien lisääminen](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Lisää OMS-yhteys määritysten hallinta

Jotta voit lisätä OMS-yhteyden, määritysten hallinta-ympäristösi on oltava määritetty online-tilassa [service yhteyspistettä](https://technet.microsoft.com/library/mt627781.aspx) .

1. Valitse **hallinta** -työtilan määritysten hallinta **OMS yhdistin**. **Lisää OMS ohjattu tietoyhteyden muodostaminen**avautuu. Valitse **Seuraava**.

2. **Yleiset** näytössä Varmista, että olet tehnyt seuraavat toimet ja, että olet kunkin kohteen tiedot ja valitse sitten **Seuraava**.
  1. Azure hallinta-portaalissa rekisteröitymisen määritysten hallinnan Web-sovelluksen ja/tai verkko-Ohjelmointirajapinnan sovelluksen ja että sinulla on [Asiakastunnus-rekisteröinnin](../active-directory/active-directory-integrating-applications.md).
  2. Olet luonut sovelluksen-salausavaimen, rekisteröidyn sovelluksen Azure Active Directoryn Azure hallinta-portaalissa.  
  3. Azure hallinta-portaalissa määritetty rekisteröity web Appin sekä OMS käyttöoikeudet.  
  ![Yhteyden OMS ohjattu Yleiset-sivu](./media/log-analytics-sccm/sccm-console-general01.png)

3. Siirry **Azure Active Directory** -näytössä määrittää yhteysasetukset OMS antamalla **vuokraajan** ja **Asiakastunnus** ja **Asiakkaan salainen avain** ja valitse sitten **Seuraava**.  
  ![Yhteyden OMS ohjattu Azure Active Directory-sivulle](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Jos kaikki muut menettelyt toteuttaa onnistuneesti, valitse **OMS yhteysmäärityksen** näytön tiedot tulevat automaattisesti näkyviin tällä sivulla. Lisätietoja yhteysasetukset pitäisi näkyä **Azure tilauksen** , **Azure resurssiryhmä** ja **Toimintojen hallinta Suite-työtilassa**.  
  ![Yhteyden OMS ohjattu OMS yhteyden sivulla](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. Ohjattu toiminto yhdistää OMS-palvelu on syötetty tiedot. Valitse laitteen kokoelmien, jonka haluat OMS kanssa, ja valitse sitten **Lisää**.  
  ![Valitse sivustokokoelmat](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Tarkista yhteysasetukset **Yhteenveto** -näytössä ja valitse sitten **Seuraava**. Näytön **edistyminen** näkyy yhteyden tila ja valitse olisi **Valmis**.

>[AZURE.NOTE] Sinun on muodostettava OMS Keskitaso sivustossa oman hierarkian. Jos OMS yhdistäminen erillinen ensisijaisessa ja lisää sitten keskitetyn hallinnan sivuston ympäristön, sinun on Poista ja luo uusi hierarkiassa OMS-yhteys.

Kun olet linkittänyt hallintatoiminto OMS, voit lisätä tai poistaa sivustokokoelmat ja OMS yhteyden ominaisuuksien tarkasteleminen.

## <a name="update-oms-connection-properties"></a>Päivitä OMS yhteyden ominaisuudet

Jos salasana tai asiakkaan salausavaimen vanhenee koskaan tai menetetään, tarvitset manuaalinen päivittäminen OMS yhteyden ominaisuudet.

1. Määritysten hallinta **Pilvipalveluihin** Siirry ja valitse sitten Avaa **OMS yhteyden ominaisuudet** -sivun **OMS yhdistin** .
2. Valitse tällä sivulla voit tarkastella **vuokraajan**, **Asiakastunnus**- **asiakasohjelman salaisen avaimen vanheneminen** **Azure Active Directory** -välilehdessä. **Tarkista** **asiakkaan salausavaimen** jos se on vanhentunut.


## <a name="download-and-install-the-agent"></a>Lataa ja asenna agentti

1. Valitse OMS-portaalissa [Lataa OMS-agentti asennustiedosto](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Asennetaan ja määritetään agentti tietokoneessa käynnissä hallintatoiminto palvelun yhteyden pisteen sivuston järjestelmän rooli jollakin seuraavista tavoista:
  - [Asenna agent-vaihtoehdon avulla](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Asenna komentoriviltä agentti](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Asenna DSC käyttäminen Azure automaatio agentti](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Tuo sivustokokoelmat

Kun olet lisännyt OMS yhteyden määritysten hallinta ja asennettu agentti tietokoneessa käynnissä määritysten hallinta-palvelun yhteyden kohta sivuston järjestelmän rooli, seuraava vaihe on tuotava sivustokokoelmat-määritysten hallinta-OMS tietokoneen ryhminä.

Kun tuonti on otettu käyttöön, sivustokokoelman jäsentietojen haetaan 3 tunnin välein tasalla sivustokokoelman jäsenyyksiä. Voit poistaa käytöstä milloin tahansa tuonti.

1. Valitse OMS-portaalin **asetuksia**.
2. Valitse **Tietokoneryhmät** -välilehti ja valitse sitten **SCCM** -välilehti.
3. Valitse **Tuo hallintatoiminto sivustokokoelman jäsenyydet** ja valitse sitten **Tallenna**.  
  ![Tietokoneryhmät - SCCM-välilehti](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Tietojen tarkasteleminen määritysten hallinta

Kun olet lisännyt OMS yhteyden määritysten hallinta ja agentti asennettu tietokoneeseen, jossa hallintatoiminto palvelun yhteyden pisteen sivuston järjestelmän rooli-agentti tiedot lähetetään OMS. OMS-määritysten hallinta-sivustokokoelmat näkyvät [tietokoneen](log-analytics-computer-groups.md)ryhminä. Voit tarkastella ryhmät **Tietokoneryhmät** kohdasta **Määritysten hallinta** -sivulla **asetukset**.

Tuodut kokoelmien näet, kuinka monta tietokoneissa, joissa sivustokokoelman jäsenyydet on havaittu. Voit myös tarkastella kokoelmien, joka on tuotu numeroa.

![Tietokoneryhmät - SCCM-välilehti](./media/log-analytics-sccm/sccm-computer-groups02.png)

Kun napsautat joko yhdestä, Etsi avautuu ja näyttää joko kaikki tuodut ryhmien tai kaikissa tietokoneissa, joihin kuuluvat mihinkin ryhmään. [Log-haun](log-analytics-log-searches.md)avulla voit aloittaa Perusteellisessa analyysissa hallintatoiminto tiedoille.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Log-haun](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia tietoja määritysten hallinta tietojen.

<properties
   pageTitle="Azure Active Directory-raportointi - esikatselu | Microsoft Azure"
   description="Näyttää luettelon eri raporttien Azure Active Directory-esikatselu"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory-raportointi - esikatselu

> [AZURE.SELECTOR]
- [Azure portal](active-directory-reporting-azure-portal.md)
- [Azure perinteinen portal](active-directory-reporting-guide.md)

*Näissä ohjeissa on osa [Azure Active Directory raportoinnin Guide](active-directory-reporting-guide.md).*

Raportoinnin Azure Active Directory-esikatselu-saat kaikki haluamasi tiedot, sinun on määritettävä, miten ympäristön jokaisen. [Mikä on esikatselussa?](active-directory-preview-explainer.md)

On kaksi pääaluetta raportoinnista:

- **Kirjaudu sisään toimintoja** – tietoja hallittuja sovelluksia ja käyttäjän kirjautumisnimi toiminnot käyttö

- **Valvontalokien** - toimintojen järjestelmän käyttäjät ja ryhmän hallinta, hallittuja sovelluksia ja directory toimintoja tiedot

Sen mukaan, kun etsit tietoja aluetta voit käyttää näiden raporttien joko valitsemalla **käyttäjät ja ryhmät** - tai **Enterprise-sovellusten** [Azure portal](https://portal.azure.com)-palvelut-luettelosta.

## <a name="sign-in-activities"></a>Kirjaudu sisään toiminnot

### <a name="user-sign-in-activities"></a>Käyttäjän toiminnot-kirjautuminen

Käyttäjäraportti-kirjautuminen myöntämä tiedoilla löydät vastauksia kysymyksiin kuten:

- Mikä on käyttäjän kirjautumisnimi kuvion?
- Kuinka monta käyttäjää on käyttäjiä kirjautuneena viikon ajan?
- Mikä on nämä Kirjaudu apuohjelmien tila?

Merkintä-kohdan näitä tietoja on **käyttäjät ja ryhmät**-kohdassa **Yleistä** -osan kaavio käyttäjän kirjautumisnimi.

 ![Raportointi] (./media/active-directory-reporting-azure-portal/05.png "Raportointi")

Käyttäjän kirjautuminen kaavio näyttää Kirjaudu viikoittain koosteet ins kaikille käyttäjille tiettynä aikajaksona. Aika-asteikon oletusarvo on 30 päivää.

![Raportointi] (./media/active-directory-reporting-azure-portal/02.png "Raportointi")

Kun napsautat kirjautumisen Graphiin päivänä, saat tarkat Kirjaudu sisään-toimintoja.

![Raportointi] (./media/active-directory-reporting-azure-portal/03.png "Raportointi")

Kullekin riville kirjautumisen Tehtävät-luettelon avulla voit valitun kirjautumisnimi yksityiskohtaisia tietoja seuraavasti:

- Kuka on kirjautunut sisään?

- Mikä oli liittyvät UPN?

- Mitä sovellus ei kirjautuminen-kohteen?

- Mikä kirjautuminen IP-osoite?

- Mikä oli kirjautuminen-tilaa?

### <a name="usage-of-managed-applications"></a>Hallittujen sovellusten käyttö

Kirjaudu sisään tietojen sovelluksen keskitettyä View voit vastata kysymyksiin kuten:

- Kuka käyttää Omat sovellukset?

- Mitkä ovat organisaation ylimmän 3 sovelluksia?

- Minulla on viimeksi esiin sovelluksen. Miten se tekee?


Merkintä-kohdan näitä tietoja on organisaation viimeisen 30 päivän raportissa **Yleistä** -osan kohdassa **yrityksen**yläreunan 3 sovellukset.

 ![Raportointi] (./media/active-directory-reporting-azure-portal/06.png "Raportointi")


-Sovelluksen käyttö graph viikoittain koosteet, kirjaudu ins ensimmäiset 3-sovellusten tiettynä aikajaksona. Aika-asteikon oletusarvo on 30 päivää.

![Raportointi] (./media/active-directory-reporting-azure-portal/78.png "Raportointi")

Jos haluat, voit määrittää kohdistuksen tietylle sovellukselle.

![Raportointi] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Raportointi")


Kun napsautat app käyttö kaaviossa päivänä, saat tarkat Kirjaudu sisään-toimintoja.


![Raportointi] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Raportointi")



**Kirjautumiset** -vaihtoehdon avulla voit kaikki kirjautumisen tapahtumat täydellinen yleiskuva sovellukset.

![Raportointi] (./media/active-directory-reporting-azure-portal/85.png "Raportointi")

Sarakkeen valitseminen käyttämällä voit valita näytettävien tietojen kentät.

![Raportointi] (./media/active-directory-reporting-azure-portal/column_chooser.png "Raportointi")



### <a name="filtering-sign-ins"></a>Kirjaudu apuohjelmien suodattaminen

Voit suodattaa kirjautumiset aikavälin rajoittaa näytettävien tietojen määrän mukaan.

![Raportointi] (./media/active-directory-reporting-azure-portal/927.png "Raportointi")


Toinen tapa suodattaa tapahtumia kirjautumisen toiminnot on etsiä merkinnät.
Hakumenetelmä mahdollistaa laajuus oman merkki-laajennukset tietyille **käyttäjille**tai **ryhmille** **sovellusten**ympärille.


![Raportointi] (./media/active-directory-reporting-azure-portal/84.png "Raportointi")

## <a name="audit-logs"></a>Valvontalokien

Azure Active Directory valvonta lokit sisältävät tietueet järjestelmän toiminnan yhteensopivuuden.

On kolme tärkeimmät luokkaa tarkistettaviksi aktiviteetit Azure-portaalissa:

- Käyttäjät ja ryhmät   

- Sovellukset

- Hakemisto   


Valvonta raportin toiminnan kattavaan luetteloon Tutustu [valvonta raportin tapahtumien luetteloon](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Merkintä-kohdan kaikki valvonta tiedot on **valvontalokien** **Azure Active Directory** **toiminta** -osassa.


![Valvonta] (./media/active-directory-reporting-azure-portal/61.png "Valvonta")


Valvontalokin on luettelonäkymän, jossa näkyy toimijat (jotka) toiminnot (mitä) ja kohteet.


![Valvonta] (./media/active-directory-reporting-azure-portal/345.png "Valvonta")


Saat lisätietoja napsauttamalla kohteen luettelonäkymässä.

![Valvonta] (./media/active-directory-reporting-azure-portal/873.png "Valvonta")




### <a name="users-and-groups-audit-logs"></a>Käyttäjien ja ryhmien valvontalokit


Käyttäjän ja ryhmän perustuva valvonta-kaavioraporteista saat vastauksia kysymyksiin kuten:

- Päivityksen tyyppi on käytetty käyttäjille?

- Kuinka monta käyttäjää on muuttunut?

- Kuinka monta salasanat on muuttunut?

- Mikä on järjestelmänvalvojan valmis-kansioon?

- Mitä, jotka on lisätty ryhmät ovat?

- Onko ryhmien jäsenyyden muutokset?

- Muutettu omistajat-ryhmän?

- Käyttöoikeudet on määritetty ryhmälle tai käyttäjälle?


Jos haluat tarkastella valvonta käyttäjille ja ryhmille liittyvät tiedot, löytyvät kohdasta **valvontalokien** suodatettujen näkymien **tehtävän** osan **käyttäjät ja ryhmät**.


![Valvonta] (./media/active-directory-reporting-azure-portal/93.png "Valvonta")


### <a name="application-audit-logs"></a>Sovelluksen valvontalokien

Kanssa sovelluksen perustuva valvonta raportteja, voit saada vastauksia kysymyksiin, kuten:

- Mitä sovelluksia, jotka on lisätty tai päivitetty?

- Mitä sovelluksia, jotka on poistettu?

- Vaihtuu palvelun tarpeen, sovelluksen?

- Muutettu sovellusten nimet

- Kuka on antanut suostumuksen sovellukseen?


Jos haluat tarkastella valvonta-sovellukset liittyvät tiedot, löytyvät kohdasta **valvontalokien** suodatettujen näkymien **yrityksen** **tehtävän** -osassa.


![Valvonta] (./media/active-directory-reporting-azure-portal/134.png "Valvonta")


### <a name="filtering-audit-logs"></a>Suodatus valvontalokien

Voit suodattaa raportti aikavälin rajoittaa näytettävien tietojen määrän mukaan.

![Valvonta] (./media/active-directory-reporting-azure-portal/324.png "Valvonta")

Toinen tapa suodattaa valvontalokin tapahtumien on Etsi merkinnät.

![Valvonta] (./media/active-directory-reporting-azure-portal/237.png "Valvonta")

## <a name="next-steps"></a>Seuraavat vaiheet

Katso [Azure Active Directory raportoinnin opas](active-directory-reporting-guide.md).

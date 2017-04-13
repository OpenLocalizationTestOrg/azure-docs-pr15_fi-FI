<properties
    pageTitle="Azure AD Connect synkronointi: synkronoinnin palvelun hallinta Käyttöliittymän | Microsoft Azure"
    description="Ymmärtäminen Azure AD Connect synkronoinnin palvelun hallinta-kohdassa Toiminnot-välilehti."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect synkronointi: synkronoinnin palvelun hallinta

[Toiminnot](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Yhdistimet](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse suunnittelu](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse haku](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Toiminnot-välilehti sisältää viimeisimmän saadut tulokset. Tässä välilehdessä on tietoja ja vianmääritys.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Olevien tietojen näkyvissä olevat toiminnot-välilehti
Yläosassa näkyy kaikki lohkot pitkän aikavälin järjestyksessä. Oletusarvon mukaan toimintojen lokin säilyttää tietoja viimeisten seitsemän päivän aikana, mutta tämä asetus voidaan muuttaa [ajoitus](active-directory-aadconnectsync-feature-scheduler.md). Haluat nähdä: n asennuksen, joka ei näy onnistui-tila. Voit muuttaa lajittelu napsauttamalla otsikot.

**Tila** -sarakkeessa on tärkeitä tietoja ja näyttää eniten vakavia ongelmaa, suorita. Seuraavassa on lyhyt yhteenveto yleisimmistä tiloista tutkia järjestystä (jossa * osoittaa useita mahdollista virhemerkkijonot).

Tila | Kommentti
--- | ---
pysäytetty-* | Suorita ei onnistunut. Jos esimerkiksi Etäjärjestelmä ei ole käytettävissä, ja se voi ottaa yhteyttä.
pysäytetty-virhe-rajoita | On enemmän kuin 5 000 virheitä. Suorita automaattisesti keskeytettiin paljon virheiden vuoksi.
valmis -\*-virheet | Suorita valmis, mutta on virheitä (enintään 5 000), joka on selvitettävä.
valmis -\*-varoitukset | Suorita valmis, mutta tietoja ei ole odotettu tilassa. Jos sinulla on virheitä, viesti on yleensä vain Oire. Ennen kuin on osoitettu virheet, varoitukset ei tarkista.
onnistumista | Ei ole ongelmia.

Kun valitset rivin, alaosassa päivittää Näytä tiedot, jotka suoritetaan. Vasempaan reunaan alareunan sinun on ehkä ajattelevat **Vaihe #**luettelon. Tämä luettelo on näkyvissä vain, jos sinulla on useita toimialueita oman metsää, joissa kunkin toimialueen edustaa vaihetta. Toimialuenimi löytyy **osio**-otsikon alla. **Synkronoinnin tilastotiedot**löydät lisätietoja käsiteltiin muutosten määrää. Voit valita linkit muutetut objektit luettelo. Jos sinulla on objektit, joissa on virheitä, kyseiset virheet näkyvät **Synkronointivirheet**-kohdassa.

## <a name="troubleshoot-errors-in-operations-tab"></a>Vianmääritys Toiminnot-välilehti
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Jos sinulla on virheitä, sekä objektin virhe ja virheen itse on linkkejä, joka sisältää enemmän tietoa.

Aloita napsauttamalla virhe merkkijono (**Synkronoi-sääntö-virhe-funktio – saatu** kuvassa). Näyttöön tulee ensin objektin yleiskatsaus. Todellinen virheen näkyviin valitsemalla **Pinon jäljitys**. Tämä jäljitys on virheenkorjaus ylätason tietoja virheen.

**Vihje:** Voit **kutsua pinon tiedot** -ruudussa hiiren kakkospainikkeella, valitse **Valitse**ja **Kopioi**. Voit kopioida pino ja tarkastella virheen tuttuja editorin, esimerkiksi Muistiossa.

- Jos virhe on **SyncRulesEngine**, puhelu pinon tiedot on ensin kaikki määritteet luettelo objektin. Vieritä alaspäin, kunnes otsikko näkyy **InnerException-kohdassa = >**.  
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
Kun olet rivillä on virhe. Yllä olevassa kuvassa virhe on mukautettu synkronointi säännön Fabrikam luotu.

Jos virheen itse ei anna tarpeeksi tietoa, on aika tietoja itse. Voit valita linkin objektitunnus ja [Noudata objektin ja sen tietojen järjestelmän kautta](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

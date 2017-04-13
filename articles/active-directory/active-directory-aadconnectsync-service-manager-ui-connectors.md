<properties
    pageTitle="Azure AD Connect synkronointi: synkronoinnin palvelun hallinta Käyttöliittymän | Microsoft Azure"
    description="Ymmärtäminen Azure AD Connect yhdysviivat-välilehdestä synkronoinnin palvelun hallinta."
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

![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Yhdysviivat-välilehteä käytetään hallinta kaikki järjestelmät synkronointi-ohjelma on yhdistetty.

## <a name="connector-actions"></a>Yhdistimen toiminnot

Toiminto | Kommentti
--- | ---
Luo | Älä käytä. Muodostamisesta muita AD metsien käyttämällä ohjattua asennusta.
Ominaisuudet: | Käytettävän toimialueen ja OU suodatus.
[Poista](#delete) | Käyttää poistamista joko connector-tilan tai poistaa yhteyden metsää.
[Suorita-profiileista määrittäminen](#configure-run-profiles) | Toimialueita lukuun ottamatta suodattaminen, mitään tässä. Tämän toiminnon avulla voit on jo määritetty Suorita-profiileista.
Suorita | Käyttää käynnistämiseen kertakäyttöisen suorituksen profiilia.
Seis | Pysäyttää käynnissä profiilin yhdistin.
Vie yhdistin | Älä käytä.
Tuo-yhdistin | Älä käytä.
Päivitä yhdistin | Älä käytä.
Päivitä rakenne | Päivittää välimuistissa rakenne. Se on ensisijainen käyttämään asetusta ohjatun asennuksen sen sijaan, koska päivitykset synkronoida myös säännöt.
[Etsi yhdistimen tilaa](#search-connector-space) | Objektien etsiminen käytettävä ja [seuraamaan objektin ja sen tietojen järjestelmän kautta](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Poista
Poista-toimintoa käytetään kahdella eri tavalla.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

**Poistaa vain yhdistimen tilaa** vaihtoehto poistaa kaikki tiedot, mutta säilyttää määritykset.

**Poista yhdistimen** ja yhdistimen tilaa poistaa tiedot ja määritykset. Tätä asetusta käytetään, kun et halua enää muodostaa yhteyden metsää.

Molemmat vaihtoehdot kaikkien objektien synkronointiin ja Päivitä metaverse objekteja. Tämä toiminto on pitkä käynnissä toiminto.

### <a name="configure-run-profiles"></a>Suorita-profiileista määrittäminen
Tämän asetuksen avulla voit on määritetty yhdistimen Suorita profiilit.

![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Etsi yhdistimen tilaa
Etsi yhdistin tila-toiminto on hyötyä objektien etsiminen ja tietojen ongelmien vianmääritys.

![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Aloita valitsemalla **laajuus**. Voit etsiä tietojen perusteella (suhteellista erillistä nimeä DN-ankkuri-aliraportti puun), tai objektin (kaikki muut asetukset).  
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Jos teet esimerkiksi aliraportti puun haun, saat kaikki objektit yhteen OU.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) Tässä ruudukossa voit valita-objekti, valitse **Ominaisuudet**ja [sen perään](#follow-an-object-and-its-data-through-the-system) lähde-yhdistin tilasta, metaverse- ja kohde connector-kohtaan.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Noudata objektin ja sen tietojen järjestelmän kautta
Ongelman tiedot ovat vianmääritys, kun seuraa objektin lähde connector-tilasta metaverse ja kohteeseen yhdistimen tila on avaimen toimintosarja ymmärtämään, miksi tietoja ei ole odotettu arvot.

### <a name="connector-space-object-properties"></a>Yhdistimen tilaa objektin ominaisuudet
**Tuo**  
Kun avaat cs-objekti, on useita välilehtiä yläreunassa. **Tuo** -välilehdessä näkyvät tiedot, jotka on vaiheistettu tuonnin jälkeen.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) **Vanha arvo** näyttää tällä hetkellä on tallennettu järjestelmän ja **Uusi arvo** , mikä on vastaanotettu lähde-järjestelmästä ja ei ole vielä kohdistettu. Tässä tapauksessa koska synkronointivirhe, muutos ei voi käyttää.

**Virhe**  
Virhesivun näkyy vain, jos objekti ongelma. Tarkastella lisätietoja siitä, miten voit [tehdä synkronoinnin vianmäärityksen](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab)toiminnot-sivulla.

**Hierarkiatasolla**  
Hierarkiatasolla-välilehdessä näkyy, miten yhdistimen tilaa objektin liittyvät metaverse-objekti. Voit tarkastella yhdistimen tuonti viimeksi muutoksen yhdistetyn järjestelmä- ja minkä säännöt käyttöön metaverse tietojen täyttämiseen.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) **toiminto** -sarakkeessa näkyy on yksi **saapuvien** synkronointi sääntö **säännöstä**-toiminnolla. Joka ilmaisee, että kun yhdistimen tilaa objekti ei sisällä tietoja, metaverse objekti säilyy. Jos Synkronoi Sääntöluettelo näkyy sen sijaan ** **Lähtevä** suunta ja**Synkronoi sääntö, se osoittaa, että tämä objekti poistetaan, kun metaverse objekti poistetaan.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) näet myös **PasswordSync** sarakkeen saapuvien yhdistin tilaa voivat osallistua salasana muutokset koska yksi synkronointi sääntö on arvo **Tosi**. Salasana lähetetään sitten Azure AD lähtevän säännön avulla.

Hierarkiatasolla-välilehdessä voit siirtyä metaverse valitsemalla [Metaverse objektin ominaisuudet](#metaverse-object-properties).

Kaikkien välilehtien alareunassa on kaksi painiketta: **Esikatselu** ja **Kirjaudu**.

**Esikatselu**  
Esikatselusivulla sivun käytetään synkronoimaan yhdessä yhtenä objektina. Se on hyödyllinen, jos vianmääritystä asiakkaan synkronointi sääntöjä ja haluat nähdä muutoksen vaikutus yhtenä objektina. Voit valita **Koko synkronointi** ja **Delta synkronointi**välillä. Voit myös valita **Luo esikatselu**, joka säilyttää muutoksen vain muistiin, ja **Vahvista esikatselu**, kaikki vaiheet muutoksista kohde yhdistimen välilyöntejä.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) voit tarkastaa objekti ja mitä sääntöä käytetään tietyn määritteen kulkuun.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Lokitiedoston**  
Log-sivun käytetään on salasana synkronoinnin tila-ja historia-lisätietoja [vianmääritys salasanojen synkronoinnin](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) .

### <a name="metaverse-object-properties"></a>Metaverse objektin ominaisuudet
**Määritteet**  
Määritteet-välilehdessä näet arvot ja mitkä yhdistin on lähettänyt sen.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**yhdistimet**  
Yhdysviivat-välilehdessä näkyvät kaikki yhdistimen välilyönnit, joissa on objektin kuvaus.
![Synkronoinnin palvelun hallinta](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) tämän välilehden avulla voit siirtyä [yhdistimen tilaa objektin](#connector-space-object-properties)myös.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

<properties 
    pageTitle="Ilmoitukset ja sähköpostimalleja määrittämisestä Azure API hallinta" 
    description="Opettele ilmoitusten määrittäminen ja sähköpostin mallit Azure API hallinta." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Ilmoitukset ja sähköpostimalleja määrittämisestä Azure API hallinta

API hallinta avulla voit määrittää tietyn liittyviä tapahtumailmoituksia ja sähköpostimalleja, joiden avulla järjestelmänvalvojat sekä kehittäjät API hallinta-esiintymän määrittäminen. Tässä ohjeaiheessa kerrotaan, miten voit määrittää käytettävissä liittyviä tapahtumailmoituksia ja esitetään yleiskatsaus määrittäminen käyttää näitä tapahtumia sähköpostimalleja.

## <a name="publisher-notifications"> </a>Publisher-ilmoitusten määrittäminen

Määritä ilmoitukset, valitse Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin.

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Valitse **ilmoitukset** vasemmalle, jos haluat tarkastella käytettävissä ilmoitukset **API hallinta** -valikon.

![Publisher-ilmoitukset][api-management-publisher-notifications]

Seuraavassa luettelossa tapahtumien voi määrittää ilmoituksia.

-   **Tilauksen pyynnöt (pakollista hyväksymistä)** - määritetyn sähköpostin vastaanottajien ja käyttäjät saavat sähköposti-ilmoitusten tietoja tilauksen pyynnöt API tuotteiden pakollista hyväksymistä.
-   **Uusissa tilauksissa** - määritetyn sähköpostin vastaanottajien ja käyttäjät saavat sähköposti-ilmoitusten tietoja uusi API tuotteen tilaukset.
-   **Sovelluksen valikoima pyynnöt** - määritetyn sähköpostin vastaanottajien ja käyttäjät saavat sähköposti-ilmoitusten lähetettäessä uusien sovellusten sovelluksen valikoimaan.
-   **Piilokopio** - määritetyn sähköpostin vastaanottajien ja käyttäjät saavat sähköpostin sokeat viestistä kopion kaikki sähköpostit lähetetään kehittäjät.
-   **Uusi seurantakohde tai kommentti** - määritetyn sähköpostin vastaanottajien ja käyttäjät saavat sähköposti-ilmoitukset, kun uusi seurantakohde tai kommentti lähetetään developer-portaalissa.
-   **Sulje tili viestin** - määritetyn sähköpostin vastaanottajien ja käyttäjät saavat sähköposti-ilmoitukset, kun tili on suljettu.
-   **Approaching Tilauksen kiintiö** - seuraavat sähköpostin vastaanottajien ja käyttäjät saavat sähköposti-ilmoitukset, kun tilauksen käyttö syötetään lähellä käyttökiintiö.

Voit määrittää sähköpostin vastaanottajat, jotka käyttävät sähköpostiosoite teksti kuhunkin tapahtumaan tai voit valita käyttäjien luettelo.

Voit määrittää ilmoitusten sähköpostiosoitteet kirjoittamalla ne Kirjoita sähköpostiosoite teksti. Jos sinulla on useita sähköpostiosoitteita, erota ne pilkulla.

![Ilmoituksen vastaanottajat][api-management-email-addresses]

Voit määrittää ilmoitusten käyttäjät, valitse **Lisää vastaanottaja**, käyttäjien ilmoitusten vieressä oleva valintaruutu ja valitse **OK**.

>Huomaa, että vain järjestelmänvalvojat näkyvät luettelossa.

Määrittämisen ilmoituksen vastaanottajat jälkeen valitsemalla **Tallenna** käyttää päivitettyä ilmoituksen vastaanottajat.

>Jos siirryt poispäin **Publisher ilmoitukset** -välilehti publisher-portaalin varoittaa, jos on tallentamattomia muutoksia.

## <a name="email-templates"> </a>Määrittäminen sähköpostimalleja

API hallinta sisältää sähköpostimalleja sähköpostiviestien, jotka lähetetään hallinta ja-palvelun avulla. Seuraavat sähköpostimalleja toimitetaan.

-   Sovelluksen valikoima lähetyksen hyväksytty
-   Kehittäjän farewell kirjain
-   Kehitystyökalut-kiintiön lähellä ilmoitus
-   Kutsu käyttäjä
-   Ongelma lisätään uusi kommentti
-   Uusi seurantakohde vastaanotettu
-   Uusi tilaus aktivoitu
-   Uusia tilauksen vahvistus
-   Tilauksen pyynnön kiellä
-   Tilauksen pyyntö on vastaanotettu

Nämä malleja voi muokata vain haluamasi.

Voit tarkastella ja sähköpostimalleja API hallinta-esiintymän määrittäminen, valitse **ilmoitukset** **API hallinta** -valikosta vasemmalla puolella ja valitse **Sähköpostimalleja** -välilehti.

![Sähköpostimalleja][api-management-email-templates]

Jos haluat tarkastella tai muokata mallia, valitse se **Mallit** -avattavasta luettelosta.

![Sähköpostin mallit-luettelosta][api-management-email-templates-list]

Kunkin sähköpostimallin on aihe vain teksti- ja leipätekstin määritelmän HTML-muodossa. Kunkin kohteen muokattavissa kuin haluamasi.

![Mallin sähköpostieditori.][api-management-email-template]

**Parametrit** -luettelo sisältää parametreja, joka lisätään aiheessa tai tekstissä, on korvattu määritetyn arvon, kun sähköposti lähetetään. Jos haluat lisätä parametrin, Aseta kohdistin kohtaan, johon haluat siirtyä parametrin ja parametrin nimen vasemmalla puolella olevaa nuolta.

Valitse **Esikatselu** tai **Lähetä testi** nähdäksesi, kuinka sähköposti sisällytetään kohde tai Lähetä jokin Testisähköpostiviesti.

>Huomaa, että parametrit korvataan ei todellisia arvoja, kun esikatselun tai lähettämällä testi.
>
>Voit tallentaa muutokset sähköpostimalli valitsemalla **Tallenna**tai Peruuta muutokset valitsemalla **Peruuta**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
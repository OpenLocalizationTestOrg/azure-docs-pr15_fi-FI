<properties
    pageTitle="Azure Active Directory-ryhmien hallinta | Microsoft Azure"
    description="Voit luoda ja hallita ryhmiä Azure Azure Active Directoryn avulla käyttäjien hallinta."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Azure Active Directory-ryhmien hallinta

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Azure perinteinen portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShellin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Jokin Azure Active Directory (Azure AD) Käyttäjähallinta toimintoja ei voi luoda käyttäjäryhmiä. Ryhmän avulla voit suorittaa hallintatehtäviä, kuten käyttöoikeuksien tai käyttöoikeuksien määrittäminen samalla kertaa usealle käyttäjälle. Ryhmien avulla voit myös määrittää oikeudet

- Resurssit, kuten hakemiston objektien
- Resurssien ulkoisen kansioon, kuten SaaS sovellukset, Azure palveluihin tai SharePoint-sivustojen tai paikallisen resurssit

Lisäksi resurssien omistajan voit määrittää access resurssin toisen henkilön omistamaan Azure AD-ryhmään. Tämän tehtävän antaa resurssin käyttöoikeutta kyseisen ryhmän jäsenet. Valitse ryhmän omistaja hallitsee ryhmän jäsenyyden. Tehokkaasti resurssien omistajan Delegoi ryhmän omistajalle oikeuden määrittää käyttäjille heidän resurssille.

## <a name="how-do-i-create-a-group"></a>Miten voin luoda ryhmän?

Sen mukaan, johon organisaatiosi on tilannut palveluja voit luoda ryhmän puolesta jompikumpi seuraavista:
- Azure perinteinen portal
- Office 365-tili-portaalissa
- Windows Intune-tili-portaalissa

Tehtävien olemme kuvaus, kuten suorittaa Azure perinteinen-portaalissa. Saat lisätietoja-Azure portaaleihin avulla voit hallita Azure Active directory- [Azure Active Directoryn hallinta](active-directory-administer.md).

1. [Azure perinteinen portal](https://manage.windowsazure.com)Valitse **Active Directory**ja valitse sitten kansion nimi organisaatiollesi.

2. Valitse **ryhmät** -välilehti.

3. Valitse **Lisää-ryhmä**.

4. Valitse **Lisää ryhmä** -ikkunassa Määritä nimi ja ryhmän kuvaus.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Miten lisääminen tai poistaminen yksittäisten käyttäjien käyttöoikeusryhmän?

**Yksittäisten käyttäjien lisääminen ryhmään**

1. [Azure perinteinen portal](https://manage.windowsazure.com)Valitse **Active Directory**ja valitse sitten kansion nimi organisaatiollesi.

2. Valitse **ryhmät** -välilehti.

3. Avaa ryhmä, johon haluat lisätä jäseniä. Avaa valitun ryhmän **jäsenet** -välilehti, jos se ei ole jo näyttäminen.

4. Valitse **Lisää jäseniä**.

5. Valitse **Lisää jäseniä** -sivulla käyttäjä tai ryhmä, johon haluat lisätä tämän ryhmän jäsenen nimi. Varmista, että nimi on lisätty **valittu** -ruutuun.


**Jos haluat yksittäisten käyttäjien poistaminen ryhmästä**

1. [Azure perinteinen portal](https://manage.windowsazure.com)Valitse **Active Directory**ja valitse sitten kansion nimi organisaatiollesi.

2. Valitse **ryhmät** -välilehti.

3. Avaa ryhmä, josta haluat poistaa jäseniä.

4. Valitse **jäsenet** -välilehti, valitse jäsenen, jonka haluat poistaa tämän ryhmän nimi ja valitse sitten **Poista**.

6. Vahvista tulee näkyviin, jos haluat poistaa jäsenen ryhmästä.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Miten voin hallita ryhmän jäsenyyden dynaamisesti?

Azure AD-voit hyvin helposti määrittää yksinkertaisen säännön määrittää käyttäjät, jotka on oltava ryhmän jäsenet. Yksinkertainen sääntö on sellainen, joka on vain yksi vertailu. Esimerkiksi jos ryhmä on määritetty SaaS-sovelluksen, voit määrittää säännön Lisää käyttäjät, joilla on työnimike "Myyntiedustaja". Tämä sääntö antaa sitten kaikki käyttäjät, joilla on kyseisen tehtävänimike hakemistossa SaaS tämän sovelluksen käyttöoikeudet.

Kun kaikki käyttäjän muutoksen määritteitä, järjestelmä ilmoittaa kaikki kansiossa, voit tarkastella, jos määritteen muutoksen käyttäjän käynnistää jonkin ryhmän dynaaminen ryhmän säännöt Lisää tai poistaa. Jos käyttäjä täyttää säännön ryhmälle, ne lisätään jäseneksi kyseiseen ryhmään. Jos hän ei enää täytä ryhmän jäsen niitä säännön, ne poistetaan jäseniksi kyseiseen ryhmään.

> [AZURE.NOTE] Voit määrittää dynaamisen käyttöoikeusryhmät tai Office 365-ryhmien jäsenyyden säännön. Sisäkkäisen ryhmäjäsenyyksiä ei ole tällä hetkellä tueta sovellusten ryhmän perustuva varauksen.
>
> Dynaaminen ryhmien jäsenyyksien edellytä liitetään Azure AD Premium-käyttöoikeutta
>
> - Järjestelmänvalvoja, joka hallitsee sääntö-ryhmä
> - Kaikki ryhmän jäsenet

**Jos haluat ottaa käyttöön dynaaminen ryhmän jäsenyys**

1. [Azure perinteinen portal](https://manage.windowsazure.com)Valitse **Active Directory**ja valitse sitten kansion nimi organisaatiollesi.

2. Valitse **ryhmät** -välilehti ja Avaa ryhmän, jota haluat muokata.

3. Valitse **määritys** -välilehti ja aseta arvoksi **Kyllä** **Käyttöön dynaaminen jäsenyyksiä** .

4. Määritä ryhmä, johon haluat määrittää, kuinka dynaamisen jäsenyyden tämän ryhmäfunktiot yksinkertainen yksittäisen säännön. Varmista, että **käyttäjien lisääminen mihin** vaihtoehto on valittuna ja valitse sitten luettelosta (esimerkiksi osasto, asema jne.), user-ominaisuus

5. Valitse seuraavaksi ehtoon (ei vastaa arvoa, yhtä suuri kuin, ei käynnistyy, käynnistää kanssa, ei sisällä, sisältää, ei vastaa, vastaa).

6. Määritä valitun käyttäjän ominaisuudelle vertailuarvo.

Lisätietoja dynaaminen Ryhmäjäsenyyden *lisäsääntöjä (säännöt, jotka voivat sisältää useita vertailuja)* luomisesta on artikkelissa [käyttäminen määritteitä, jos haluat luoda Monitasoisempia sääntöjä](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Lisätietoja

Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [Resurssien käytön hallinta ja Azure Active Directory-ryhmä](active-directory-manage-groups.md)

* [Ryhmän asetusten määrittämiseen Azure Active Directory cmdlet-komennot](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)

* [Azure Active Directory-ominaisuudet](active-directory-whatis.md)

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

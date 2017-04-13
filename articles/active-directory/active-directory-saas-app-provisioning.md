<properties
    pageTitle="Automaattinen SaaS sovelluksen käyttäjän valmistelu Azure AD | Microsoft Azure"
    description="Johdanto käyttämisestä Azure AD automaattisesti valmistelu varaustiedoista valmistelu ja käyttäjätilien päivittyä kolmannen osapuolen SaaS lisäsovellukset jatkuvasti."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisoida käyttäjän valmistelu ja valmistelun poistaminen SaaS sovellusten Azure Active Directory-hakemistosta

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Mikä on automaattinen käyttäjän valmistelu SaaS sovelluksia?

Azure Active Directory (Azure AD) avulla voit automatisoida luominen, ylläpito ja käyttäjätietojen cloud ([SaaS](https://azure.microsoft.com/overview/what-is-saas/))-sovelluksissa, kuten Dropbox, Salesforce tai ServiceNow poisto.

**Alla on joitakin esimerkkejä siitä, mitä tämän ominaisuuden avulla voit tehdä:**

- Luo automaattisesti uusi tili oikean SaaS-sovelluksissa uusi henkilöille, kun he liittyä ryhmän.
- Aktivoinnin tilit SaaS sovelluksista automaattisesti, kun väistämättä jättävät ryhmän.
- Varmistaa, että SaaS-sovelluksissa käyttäjätietojen ovat ajan tasalla muutosten hakemistossa.
- Valmistele muiden kuin käyttäjäkohtaisten objektit, kuten ryhmiä, SaaS sovellukset, jotka tukevat niitä.

**Automaattinen käyttäjän valmistelu myös seuraavia toimintoja:**

- Mahdollisuus välillä Azure AD käyttäjätietoja vastaavat ja SaaS sovellukset.
- Mukautusasetukset Azure AD-ohjeessa Sovita SaaS sovellukset, jotka organisaatiosi on käytössä nykyisen määrityksiä.
- Valinnainen sähköposti-ilmoituksia, valmisteluvirheitä.
- Raportointi ja toimintojen lokitiedot auttamaan valvonta ja vianmääritys.

##<a name="why-use-automated-provisioning"></a>Mitä hyötyä automaattinen valmistelu?

Joitakin yleisiä motivaatioita ohjatun toiminnon ovat seuraavat:

- Voit välttää kustannukset, tehottomuudet ja ihmisten manuaalinen valmistelu prosessit liittyvän virheen.
- Jos haluat suojata organisaation poistamalla käyttäjien käyttäjätietojen avaimen SaaS sovelluksista heti, kun ne jättää organisaation.
- Voit helposti tuoda samalla kertaa usealle käyttäjälle tietyn SaaS-sovellukseen.
- Jotta voit hyödyntää, ottaa Suorita sama sovelluksen käytön käytännöt, jotka on määritetty, Azure AD kertakirjautumisen luettelosta valmistelu ratkaisun helpottamiseksi.

##<a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

**Kuinka usein Azure AD kirjoittaa kansion muutokset SaaS-sovellukseen?**

Azure AD tarkistaa muutokset 5-10 minuutin välein. Jos SaaS sovellus palauttaa useita virheitä (kuten virheellinen järjestelmänvalvojan tunnistetietoja kirjainkokoa), valitse Azure AD vähitellen hidastaa sen korkojakso ja kerran päivässä, ennen kuin virheet on korjattu ylöspäin.

**Kuinka kauan valmistella aiemmin käyttäjien kestää?**

Vaiheittainen muutokset tapahtuu lähes välittömästi, mutta jos yrität valmistella useimmat hakemistossa, valitse se on riippuvainen käyttäjät ja ryhmät, joihin sinulla on määrä. Pieni kansioiden kestää vain muutaman minuutin, keskikokoiset kansioiden voi kestää useita minuutteja ja suuri kansioiden voi kestää useita tunteja.

**Miten nykyisen valmistelu työn edistymistä voi seurata?**

Voit tarkastella tilin valmistelu raportin hakemistossa raportit-osassa. Toinen vaihtoehto on käy, jotka ovat valmisteleminen ja Etsi sivun alareunassa "integrointitila"-osassa SaaS sovelluksen koontinäyttö-välilehti.

**Miten tietää, jos käyttäjät eivät Hae valmisteltu oikein?**

Ohjattu toiminto asetuksena valmistelu määritysten lopussa voit tilata sähköposti-ilmoitukset valmistelu virheet. Voit myös tarkistaa raportin virheet valmistelu, jos haluat nähdä käyttäjät, jotka epäonnistui valmisteltava ja miksi.

**Azure AD kirjoittaa muutokset SaaS sovelluksen takaisin hakemiston?**

Useimmat SaaS sovellusten valmistelu on lähtevä vain, mikä tarkoittaa, että käyttäjät kirjoitetaan hakemiston sovelluksen ja sovelluksen muutoksia ei voi kirjoittaa takaisin hakemiston. [Työpäivä](https://msdn.microsoft.com/library/azure/dn762434.aspx)-valmistelu on kuitenkin saapuva ‑sääntöjä, mikä tarkoittaa, että, käyttäjien on tuotu työpäivä-kansioon ja vastaavasti hakemistossa muutoksia ei Hae kirjoittaa takaisin ne työpäivä.

**Miten voivat lähettää palautetta suunnitteluryhmät ryhmälle?**

Ota yhteyttä kautta [Azure Active Directory palautteen keskustelupalstalle](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Miten automaattinen valmistelu työn?

Azure AD valmistelee SaaS sovellusten käyttäjien muodostamalla yhteyden valmistelu kunkin sovelluksen toimittajan tarjoamia päätepisteet. Nämä päätepisteet Salli Azure AD ohjelmallisesti luoda, päivittää ja poistaa käyttäjiä. Alla on eri vaiheet, jotka voit automatisoida valmistelu kestää Azure AD lyhyesti.

1. Kun otat valmistelu sovelluksen ensimmäisen kerran, seuraavat toiminnot suoritetaan:
 - Azure AD yrittää vastaa mitä tahansa nykyisille käyttäjille SaaS sovelluksessa vastaavan henkilöllisyyttä hakemistossa. Kun käyttäjä on määritetty, ne eivät *ole* automaattisesti otettu käyttöön kertakirjautumisen. Järjestyksessä sovellus käyttää käyttäjän ne nimenomaisesti määritettävä sovellukseen Azure AD-joko suoraan tai ryhmäjäsenyyden kautta.
 - Jos olet jo määrittänyt käyttäjät, jotka määritetään sovellukseen ja Azure AD ei löydä aiemmin luotujen tilien kyseisille käyttäjille, Azure AD valmistella käyttäjätilejä niiden-sovelluksessa.
2. Kun ensimmäinen synkronointi on suoritettu, yllä olevien ohjeiden mukaisesti, Azure AD Tarkista 10 minuutin välein, ennen kuin seuraavat muutokset:
 - Jos uudet käyttäjät on myönnetty sovellukseen (joko suoraan tai Ryhmäjäsenyyden avulla), valitse ne ovat valmisteltu uuden tilin SaaS-sovelluksessa.
 - Käyttöoikeuksien poistaminen on poistettu ja sitten niiden tilin SaaS sovellukseen merkitään poistettuna (käyttäjät koskaan täysin poistetaan, joka suojaa tietojen menettämistä jatkossa virheellisen määrityksen yhteydessä).
 - Jos käyttäjä on määritetty viimeksi sovelluksen ja jo ovat samat tilin SaaS-sovelluksessa, tilin merkitään käytössä ja käyttäjän tiettyjä ominaisuuksia voi päivittää, jos ne eivät ole ajan tasalla, hakemiston verrattuna.
 - Jos käyttäjän tiedot (esimerkiksi puhelinnumeron, toimiston sijainnin jne) on muutettu kansioon, tiedot päivitetään myös SaaS-sovelluksessa.

Lisätietoja siitä, miten määritteet on yhdistetty Azure AD ja SaaS app, katso lisätietoja artikkelista [Mukauttaminen määritteiden yhdistämismääritykset](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Sovellusluettelosta, jotka tukevat automaattinen käyttäjän valmistelu

Valitse sovelluksen Nähdäksesi opetusohjelman automaattinen valmistelu sen määrittämisestä:

- [Valinta](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Isäntävaltion](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox for Business](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Salesforce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Salesforce eristetyn](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Työpäivä](http://go.microsoft.com/fwlink/?LinkId=690250) (saapuvan valmistelu)

Sovelluksen tukemaan automaattinen käyttäjän valmistelu järjestyksessä se on määritettävä tarvittavat päätepisteet, joiden avulla voit automatisoida luominen, ylläpito ja käyttäjien poistaminen Ulkoiset ohjelmat. Tämän vuoksi kaikki SaaS sovellukset ovat yhteensopivia tätä ominaisuutta. Sovellusten, jotka tukevat tätä suunnitteluryhmät Azure AD-ryhmän voi luoda kyseisissä sovelluksissa valmistelu yhdistimen ja toimisivat on ensin nykyinen ja mahdollisten käyttäjille mukaan.

Azure AD suunnitteluryhmät ryhmän lisäsovellukset valmistelu tukipyyntö yhteyshenkilölle lähettää viestin [Azure Active Directory palautteen keskustelupalstalla](https://feedback.azure.com/forums/169401-azure-active-directory/)kautta.

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Määritteiden yhdistämismääritykset käyttäjän valmisteluun mukauttaminen](active-directory-saas-customizing-attribute-mappings.md)
- [Määritteiden yhdistämismääritykset lausekkeiden kirjoittamiseen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Vaikutusalueen määrittäminen käyttäjien valmisteluun suodattimet](active-directory-saas-scoping-filters.md)
- [Ota käyttöön automaattinen valmisteleminen käyttäjien ja ryhmien Azure Active Directorysta sovellusten SCIM avulla](active-directory-scim-provisioning.md)
- [Tilin valmistelu ilmoitukset](active-directory-saas-account-provisioning-notifications.md)
- [Luettelo siitä, miten voit integroida SaaS sovellusten opetusohjelmat](active-directory-saas-tutorial-list.md)

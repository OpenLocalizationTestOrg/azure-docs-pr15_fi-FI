<properties 
    pageTitle="Microsoft Azure multi-factor Authentication käyttäjän tilat"
    description="Lisätietoja käyttäjän hyötyä Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="user-states-in-azure-multi-factor-authentication"></a>Azure multi-factor Authentication-käyttäjän tilat

Azure Monimenetelmäisen todentamisen käyttäjätilien on kolme eri seuraavista tiloista:

Tila | Kuvaus |Muu selain sovellusten vaikuttaa| Huomautuksia
:-------------: | :-------------: |:-------------: |:-------------: |
Ei käytössä | Uusi käyttäjä ei ole rekisteröity monimenetelmäisen todentamisen oletustilaan.|Ei|Käyttäjä ei käytetä monimenetelmäisen todentamisen.
Käytössä |Käyttäjän on rekisteröity monimenetelmäisen todentamisen.|Ei.  Ne jatkaa työskentelyä, ennen kuin Rekisteröintiprosessi on valmis.|Käyttäjän on käytössä, mutta se ei ole valmis rekisteröinnin loppuun. Ne pyydetään suorittamaan seuraavan kirjauduttaessa sisään.
Voimassa|Käyttäjä on rekisteröity ja käyttämällä monimenetelmäisen todentamisen rekisteröinti prosessi on valmis.|Kyllä.  Sovelluksen salasanoja käyttäville sovelluksille tarvitaan. | Käyttäjä voi tai ei voi määrittänyt rekisteröinti. Jos Rekisteröintiprosessi on suoritettu, osallistujat käyttävät monimenetelmäisen todentamisen. Muussa tapauksessa käyttäjän pyydetään suorittamaan seuraavan kirjauduttaessa sisään.

## <a name="changing-a-user-state"></a>Käyttäjän tilan muuttaminen
Käyttäjät-tila muuttuu sen mukaan, onko aikaa on kulunut MFA asennus ja voiko käyttäjä on valmis prosessi.  Kun otat käyttöön MFA käyttäjän, tila vaihtuu käyttäjät käytöstä käytössä.  Kun käyttäjä, jonka tila on muutettu käytössä, kirjautuu ja prosessi on valmis, henkilön tila muuttuu pakotettu.  

### <a name="to-view-a-users-state"></a>Voit tarkastella käyttäjän tila
--------------------------------------------------------------------------------
1.  Kirjaudu **Azure perinteinen portaaliin** järjestelmänvalvojana.
2.  Valitse vasemmassa reunassa **Active Directorysta**.
3.  Valitse- **kansion** kohdasta haluat ottaa käyttöön käyttäjän kansio.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Valitse **käyttäjät**yläreunassa.
5.  Valitse **Hallitse multi-factor Auth**sivun alareunaan.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Tämä avaa uusi selaimen välilehti.  Sinulla voi tarkastella käyttäjät-tilaan.
![Valitse kansio](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Voit muuttaa tila käytöstä, käytössä
1.  Kirjaudu **Azure perinteinen portaaliin** järjestelmänvalvojana.
2.  Valitse vasemmassa reunassa **Active Directorysta**.
3.  Valitse- **kansion** kohdasta haluat ottaa käyttöön käyttäjän kansio.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Valitse **käyttäjät**yläreunassa.
5.  Valitse **Hallitse multi-factor Auth**sivun alareunaan.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Tämä avaa uusi selaimen välilehti.  Etsi käyttäjä, jonka haluat ottaa käyttöön monimenetelmäisen todentamisen. Joudut ehkä muuttamaan näkymän yläreunassa. Varmista, että tila on **käytöstä.** 
 ![Salli käyttäjän](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Aseta **Tarkista** henkilön nimen vieressä oleva ruutu.
7.  Valitse oikealta kohdasta **Ota käyttöön**.
![Jos käyttäjä](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Valitse **Ota käyttöön multi-factor todennus**.
![Jos käyttäjä](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Olisi huomaat käyttäjän tila on muuttunut poistaminen **käytöstä** **käytössä**.
![Antaa käyttäjien](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Kun olet ottanut käyttäjille, on suositeltavaa, että ne ilmoittaa sähköpostitse.  Se pitäisi myös osoittaa niitä siitä, miten ne niiden selaimen sovellusten avulla välttää voi käyttää tiedostoa.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Voit muuttaa tila käytössä/pakotettu käytöstä
1.  Kirjaudu **Azure perinteinen portaaliin** järjestelmänvalvojana.
2.  Valitse vasemmassa reunassa **Active Directorysta**.
3.  Valitse- **kansion** kohdasta haluat ottaa käyttöön käyttäjän kansio.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Valitse **käyttäjät**yläreunassa.
5.  Valitse **Hallitse multi-factor Auth**sivun alareunaan.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Tämä avaa uusi selaimen välilehti.  Etsi käyttäjä, jonka haluat poistaa käytöstä. Joudut ehkä muuttamaan näkymän yläreunassa. Varmista, että tila on joko **käytössä** tai **Pakotettu.**
7.  Aseta **Tarkista** henkilön nimen vieressä oleva ruutu.
7.  Valitse oikeanpuoleisesta **käytöstä**.
![Käyttäjän poistaminen käytöstä](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Voit pyydetään vahvistamaan tätä.  Valitse **Kyllä**.
![Käyttäjän poistaminen käytöstä](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Valitse näkyy, että se onnistui.  Valitse **sulkea.** 
 ![Käyttäjän poistaminen käytöstä](./media/multi-factor-authentication-get-started-user-states/userstate4.png)

<properties
    pageTitle="Azure AD-toimialueen palveluista: Ota Azure AD-toimialueen palvelut | Microsoft Azure"
    description="Azure Active Directory-toimialueen palveluiden käytön aloittaminen"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Ota Azure AD-toimialueen palvelut

## <a name="task-3-enable-azure-ad-domain-services"></a>Tehtävä 3: Ota Azure AD-toimialueen palvelut
Tässä tehtävässä voit ottaa käyttöön Azure AD toimialuepalveluita hakemistossa. Suorita seuraavat määritysvaiheet käyttöön Azure AD toimialuepalveluita hakemistossa.

1. Siirry **Azure perinteinen portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Valitse vasemmanpuoleisessa ruudussa **Active Directory** -solmu.

3. Valitse Azure AD-vuokraajan (hakemisto), jonka haluat ottaa käyttöön Azure AD-toimialueen palveluista.

    ![Valitse Azure AD-hakemisto](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Valitse **määritys** -välilehti.

    ![Määritä hakemisto-välilehti](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Siirry osioon, liittyvä **toimialuepalveluita**.

    ![Toimialueen palveluiden Tietolähdemääritykset-osasta](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Vaihda liittyvä **Kyllä** **käyttöön tähän kansioon toimialuepalveluita** vaihtoehto. Huomaat joitakin määritysten Lisäasetukset Azure AD-toimialueen palveluiden näy sivulla.

    ![Toimialueen palveluiden ottaminen käyttöön](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Kun otat Azure AD-toimialueen palveluista vuokraajan, Azure AD Luo ja tallentaa Kerberos ja NTLM tunnistetiedon hajautusarvot, joita tarvitaan käyttäjien.

7. Määritä **toimialueen palveluiden DNS-toimialuenimi**.

   - Hakemiston oletustoimialuenimen (eli päättyen **. onmicrosoft.com** jälkiliitteen) on oletusarvoisesti valittuna.

   - Luettelo sisältää kaikki toimialueet, jotka on määritetty Azure AD-kansion – kuten vahvistettu sekä vahvistamaton toimialueet, joita voit määrittää 'Domains'-välilehteä.

   - Lisäksi voit myös kirjoittaa mukautettua toimialuenimeä. Tässä esimerkissä on kirjoittanut mukautettua toimialuenimeä "contoso100.com".

     > [AZURE.WARNING] Varmista, että toimialuenimi (esimerkiksi contoso100, jos 'contoso100.com' toimialueen nimi) määrittää toimialueen etuliite on vähemmän kuin 15 merkkiä. Et voi luoda Azure AD-toimialueen palveluista toimialueen toimialueen etuliite yli 15 merkkiä.

8. Varmista, että olet valinnut hallitun toimialueen DNS-toimialuenimi ei vielä ole virtual verkossa. Tarkista erikseen, jos:

   - Sinulla on jo toimialue virtual verkon samaan DNS-toimialuenimeä.

   - olet valinnut virtual verkon paikallisen verkoston VPN-yhteyden ja sinulla on toimialueen DNS-toimialuenimen paikallisen verkossa.

   - aiemmin luodun pilvipalvelussa nimi on virtual verkossa.

9. Seuraava vaihe on valita virtual verkko, jossa haluat Azure AD-toimialueen palvelut ovat käytettävissä. Valitse VPN ja Kiinteä aliverkon liittyvä **Yhdistä toimialuepalveluita tämän virtual verkon**avattavasta on luotu.

   - Varmista, että olet määrittänyt virtual verkko kuuluu Azure alue, joka tukee Azure AD-toimialueen palveluista. Lisätietoja [Azure services alueittain](https://azure.microsoft.com/regions/#services/) sivun tietää Azure alueet, joiden Azure AD-toimialueen palveluista on käytettävissä.

   - Virtuaalinen verkkojen alueelta, jossa ei ole tuettu Azure AD-toimialueen palveluista Älä näytä avattavasta luettelosta.
   
   - Käytä Kiinteä aliverkon virtual verkoston Azure AD-toimialueen palveluista. Varmista, älä valitse yhdyskäytävän aliverkon. Katso [Verkko huomioon otettavia seikkoja](active-directory-ds-networking.md). 

   - Vastaavasti virtual verkkoja, jotka on luotu käyttämällä Azure Resurssienhallinta eivät näy avattavasta luettelosta. Resurssienhallinta virtual verkkojen ei tällä hetkellä tue Azure AD-toimialueen palveluista.

10. Azure AD-toimialueen palveluista, valitse **Tallenna** -tehtäväruudusta sivun alareunassa.

11. Sivulla näkyy "odotetaan..." tila, kun Azure AD-toimialueen palveluista on parhaillaan käytössä hakemistossa.

    ![Ota toimialueen palvelut - Odottava tila](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure AD-toimialueen palveluista on suuri käytettävyys hallitun toimialueen. Kun olet ottanut käyttöön Azure AD-toimialueen palveluista, Huomaa IP-osoitteet, jolla toimialuepalvelut ovat käytettävissä VPN näkyvät yksi kerrallaan. Toinen IP-osoite näkyy pian, pian palvelu mahdollistaa suuren käytettävyyden toimialueen. Kun suuri käytettävyys on määritetyn ja käytössä on käytössä toimialueellasi, näkyy kaksi IP-osoitteiden **määrittäminen** -välilehden **toimialuepalveluita** -osassa.

12. Noin 20 – 30 minuutin kuluttua näet ensimmäisen IP-osoite, jossa toimialue-palvelut ovat käytettävissä virtual verkon **määrittäminen** -sivulla **IP-osoite** -kenttään.

    ![Käytössä - toimialuepalveluita ensimmäisen IP valmistelun yhteydessä](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Kun suuri käytettävyys on toiminnassa toimialueen, näet kaksi sivulla IP-osoitteet. Hallitut toimialueesi on käytettävissä valitun virtual verkossa on nämä kaksi IP-osoitteet. Huomautus IP-osoitteiden alaspäin, jotta voit päivittää virtual verkon DNS-asetukset. Tämä vaihe mahdollistaa näennäiskoneiden virtual verkon toimintoja, kuten toimialueen liity toimialueelle.

    ![Toimialueen palveluiden käytössä - sekä IP-osoitteet valmistelun yhteydessä](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Azure AD-vuokraajan koon mukaan (käyttäjien lukumäärä ryhmittelee jne), hallitun toimialueen synkronointi kestää jonkin aikaa. Synkronoinnin tapahtuu taustalla. Suuri alihallinnoista kymmeniä tuhansia objektien kanssa voi kestää päivän tai kahden kaikille käyttäjille, ryhmäjäsenyyksiä ja synkronoidaan tunnistetiedot.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Vaihe 4 – päivityksen Azure virtual verkon DNS-asetukset
Seuraava määritys tehtävä on Azure virtual verkon DNS-asetuksia voi [päivittää](active-directory-ds-getting-started-dns.md).

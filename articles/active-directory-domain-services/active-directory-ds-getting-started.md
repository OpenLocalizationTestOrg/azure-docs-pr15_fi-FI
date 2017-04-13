<properties
    pageTitle="Azure AD-toimialueen palveluista: AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän luominen | Microsoft Azure"
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
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Azure AD-toimialueen palveluiden käytön aloittaminen

Tässä artikkelissa käydään läpi tarvitse ottaa käyttöön Azure AD-toimialueen palveluista Azure AD-vuokraajan määritystehtävät.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Tehtävä 1: Luo 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmä
Hallintaryhmän luominen Azure Active Directory-vuokraajan on ensimmäinen tehtävä. Tämä erityinen hallintaryhmän kutsutaan **AAD Ohjauskoneen järjestelmänvalvojat**. Tämän ryhmän jäsenet on myönnetty järjestelmänvalvojan oikeudet tietokoneissa, joihin on toimialueen liittyneet Azure AD-toimialueen palveluista hallitun toimialueeseen. Tämän ryhmän lisätään toimialueeseen liittyneet tietokoneissa ' järjestelmänvalvojaryhmän '. Tämän ryhmän jäsenet käyttää lisäksi etätyöpöydän toimialueeseen liittyneet koneet etäkäyttö.  

> [AZURE.NOTE] Azure AD-toimialueen palvelut-sovelluksella luodut hallitun toimialueella ei ole toimialueen järjestelmänvalvoja tai yrityksen järjestelmänvalvojan oikeudet. Hallitut toimialueiden nämä oikeudet on varattu-palvelu ja ovat ei sisällä vuokraajan käyttäjille. Voit kuitenkin suorittaa joitakin sellaisten määritysten tässä tehtävässä luodaan määräten järjestelmänvalvojien ryhmään. Nämä toiminnot ovat tietokoneiden liittäminen toimialueen Järjestelmänvalvojat-ryhmään toimialueeseen liittyneet tietokoneissa, määrittäminen ryhmän käytännön jne.

Määritysten tässä tehtävässä hallintaryhmään ja lisätä yhden tai usean käyttäjän hakemistossa ryhmään. Seuraavien toimien hallintaryhmään luomiseen Azure AD-toimialueen palveluista:

1. Siirry **Azure perinteinen portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Valitse vasemmanpuoleisessa ruudussa **Active Directory** -solmu.

3. Valitse Azure AD-vuokraajan (hakemisto), jonka haluat ottaa käyttöön Azure AD-toimialueen palveluista. Voit luoda vain yhden toimialueen kunkin Azure AD-hakemiston.

    ![Valitse Azure AD-hakemisto](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Valitse **ryhmät** -välilehti.

5. Jos haluat lisätä ryhmän Azure AD-vuokraajan, kohdasta **Lisää ryhmä** sivun alareunassa-tehtäväruudusta.

    ![Lisää ryhmä-painike](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Luo **AAD Ohjauskoneen järjestelmänvalvojat**-ryhmä. Määritä **RYHMÄTYYPPI** **Suojaus**.

    > [AZURE.WARNING] Azure AD-toimialueen palvelujen oikeus käyttöön hallittu toimialueen, luo ryhmän tarkka tämännimistä.

    ![Järjestelmänvalvojaryhmän luominen](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Lisää ryhmän kuvaus, jotta muut ymmärtää, että tämä ryhmä käytetään Myönnä järjestelmänvalvojan oikeudet sisällä Azure AD-toimialueen palveluista.

8. Ryhmän luomisen jälkeen Valitse ryhmä, johon haluat nähdä ominaisuudet tämän ryhmän nimi. Voit lisätä käyttäjiä tämän ryhmän jäsenet, napsauta alareunan paneeli **Lisää jäseniä** -painiketta.

    ![Lisää ryhmän jäsenet-painike](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. Valitse **Lisää jäseniä** -valintaikkunasta käyttäjät, joilla on tämän ryhmän jäsenet ja valitse sitten valintaruutu, kun olet valmis.

    ![Käyttäjien lisääminen järjestelmänvalvojien ryhmään](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Tehtävä 2: Luo tai valitse Azure virtual verkossa
Seuraava määritys tehtävä on [Luo tai valitse Azure virtual verkon](active-directory-ds-getting-started-vnet.md).

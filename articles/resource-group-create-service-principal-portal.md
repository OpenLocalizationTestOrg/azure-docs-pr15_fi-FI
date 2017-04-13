<properties
   pageTitle="Luo palvelu pääasiallista portaalissa | Microsoft Azure"
   description="Kerrotaan, miten voit luoda uuden Active Directory-sovelluksen ja palvelun lyhennys, jonka avulla voidaan ja Roolipohjainen käyttöoikeuksien valvonta Azure Resurssienhallinta hallittavan resurssien käytön."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Yritysportaalin avulla voit luoda Active Directory-sovelluksen ja palvelun lyhennys, voit käyttää resurssit

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Kun sovellus, joka on käyttää tai muokata resursseja, Active Directory (AD)-sovelluksen määrittäminen ja määrittää siihen tarvittavat käyttöoikeudet. Tässä ohjeaiheessa esitellään, miten voit suorittaa nämä vaiheet portaalin. Perinteinen portaalin avulla on tällä hetkellä Luo uusi Active Directory-sovellus ja siirry Azure portaalin Määritä rooli-sovellukseen. 

> [AZURE.NOTE] Tämän artikkelin vaiheet koskevat vain käytettäessä **perinteinen portal** AD-sovelluksen luominen. **Jos käytät AD-sovelluksen luomiseen Azure portaalissa, nämä vaiheet ei onnistu.** 
>
> Voit ehkä helpompaa määrittämään AD-sovelluksen ja palvelun pääasiallista [PowerShell](resource-group-authenticate-service-principal.md) tai [Azure CLI](resource-group-authenticate-service-principal-cli.md), erityisesti, jos haluat käyttää varmenteen todentamiseen. Tässä ohjeaiheessa ei näy käyttämisestä varmenne.

Katso selitys Active Directory-käsitteistä, [sovellusten ja palvelun lyhennys objekteja](./active-directory/active-directory-application-objects.md). Saat lisätietoja Active Directory käyttöoikeuksien [Todennus tilanteita, joissa Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Yksityiskohtaiset ohjeet sovelluksen integroiminen Azure resurssien hallintaa varten katso [luvan Azure Resurssienhallinta API Kehittäjän ohje](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Active Directory-sovelluksen luominen

1. Kirjaudu sisään – [perinteinen portal](https://manage.windowsazure.com/)Azure-tiliisi. 

2. Varmista, että tiedät oletusarvo Active Directory-tilauksen. Voit antaa vain access for Applicationsilla tilaus samassa kansiossa. Valitse **asetukset** ja Etsi-tilaukseen liittyvää kansionimi.  Lisätietoja on artikkelissa [miten Azure tilaukset liittyvät Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Etsi oletuskansio](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Valitse vasemmanpuoleisessa ruudussa **Active Directorysta** .

     ![Valitse Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Valitse Active Directory, jota haluat käyttää sovelluksen luomiseen. Jos sinulla on useampi kuin yksi Active Directory-sovelluksen luominen oletuskansio tilauksen.   

     ![Valitse kansio](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Voit tarkastella sovellukset-kansiossa, valitse **sovellukset**.

     ![Näytä sovellukset](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Jos et ole luonut sovelluksen ennen kansiossa, suurin piirtein seuraavanlaisen seuraavassa kuvassa pitäisi näkyä. Valitse **SOVELLUKSEN lisääminen**

     ![sovelluksen lisääminen](./media/resource-group-create-service-principal-portal/create-application.png)

     Tai valitse **Lisää** ala-ruudussa.

     ![Lisää](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Valitse sovellus, jonka haluat luoda. Valitse **Lisää sovellus organisaation kehittää**Tässä opetusohjelmassa. 

     ![uusi sovellus](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Sovelluksen nimi ja valitse sitten luotavan sovelluksen tyypin. Tässä opetusohjelmassa Luo **WEB APPLICATION ja/tai verkko-Ohjelmointirajapinnan** ja napsauta Seuraava-painiketta. Jos valitset **Alkuperäisen ASIAKASSOVELLUS**, on tämän artikkelin vaiheet eivät vastaa käyttökokemuksen.

     ![sovelluksen nimi](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Täytä sovelluksen ominaisuuksia. **KIRJAUDU edelleen URL-osoite**on sivustoon, joka kuvaa sovelluksen URI. Web-sivuston olemassaolo ei ole vahvistettu. Säätää, joka määrittää sovelluksen URI **Sovelluksen tunnus URI**.

     ![sovelluksen ominaisuudet](./media/resource-group-create-service-principal-portal/app-properties.png)

Olet luonut sovelluksen.

## <a name="get-client-id-and-authentication-key"></a>Hae asiakkaan tunnus ja todennusta avain

Kun ohjelmallisesti kirjautumisesta, tarvitset tunnuksen sovelluksen. Jos sovellus suoritetaan oma tunnistetietoja, sinun on myös todennus-näppäintä.

1. Valitse sovelluksen salasanan **määrittäminen** -välilehti.

     ![sovelluksen kokoonpanon määrittäminen](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopioi **Asiakastunnus**.
  
     ![Asiakastunnus](./media/resource-group-create-service-principal-portal/client-id.png)

3. Jos sovellus suoritetaan Omat tunnistetiedot-kohdassa, siirry **näppäimet** -osioon ja valitse kuinka kauan haluat salasana on voimassa.

     ![näppäimet](./media/resource-group-create-service-principal-portal/create-key.png)

4. Valitse Luo key-tuotetunnuksen **Tallenna** .

     ![Tallenna](./media/resource-group-create-service-principal-portal/save-icon.png)

     Tallennetun avaimen tulee näkyviin, ja voit kopioida sen. Et voi noutaa myöhemmin siten kopioi se nyt.

     ![tallennettu avain](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Vuokraajan tunnuksen hankkiminen

Kun ohjelmallisesti kirjautumisesta, sinun täytyy välittää vuokraajan tunnus todennuspyynnön kanssa. Web Apps-sovellusten ja verkkosovelluksissa API voit hakea vuokraajan tunnus valitsemalla näytön alareunassa **Näytä päätepisteet** ja haetaan tunnus seuraavassa kuvassa esitetyllä tavalla.  

   ![vuokraajan tunnus](./media/resource-group-create-service-principal-portal/save-tenant.png)

Voit myös hakea vuokraajan tunnuksen PowerShellin kautta:

    Get-AzureRmSubscription

Voit myös Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>Käyttöoikeuksien määrittäminen valtuutetun

Jos sovellus käyttää resurssien kirjautunut sisään käyttäjän puolesta, sinun on myönnettävä sovelluksen valtuutetun luvan käyttää muissa sovelluksissa. **Määritä** -välilehden **muiden sovellusten käyttöoikeudet** -osassa käyttöoikeuden myöntäminen Oletusarvon mukaan valtuutetun käyttöoikeus on jo Azure Active Directory käytössä. Jätä muuttamatta valtuutetun käyttöoikeudet.

1. Valitse **Lisää sovellus**.

2. Valitse **Windows Azure Service Management API**-luettelosta. Valitse valmis-kuvake.

      ![Valitse sovellus](./media/resource-group-create-service-principal-portal/select-app.png)

3. Valitse avattavasta luettelosta valtuutetun käyttöoikeudet, **Access Azure hallinnan kuin organisaation**.

      ![Valitse käyttöoikeudet](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Tallenna muutokset.

## <a name="assign-application-to-role"></a>Määritä rooli-sovellus

Jos sovellus on käynnissä Omat tunnistetiedot-kohdassa, Määritä rooli sovelluksen. Päättää, mitä rooli edustaa sovelluksen oikeat käyttöoikeudet. Lisätietoja käytettävissä olevat roolit on artikkelissa [RBAC: roolien hallinta](./active-directory/role-based-access-built-in-roles.md). 

Määritä rooli-sovellukseen, jos sinulla on oikeat käyttöoikeudet. Tarkemmin sanottuna sinulla on oltava `Microsoft.Authorization/*/Write` , jotka myönnetään [omistajan](./active-directory/role-based-access-built-in-roles.md#owner) rooli tai [käyttäjän Access](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) -järjestelmänvalvojaksi käyttö. Osallistujan rooli ei ole tarvittavat käyttöoikeudet.

Voit määrittää alueen tasolla, tilauksen, resurssiryhmä tai resurssi. Käyttöoikeudet periytyvät alemman tason vaikutusalueen. Esimerkiksi lisäämällä sovelluksen resurssiryhmä lukija-rooliin tarkoittaa voivat lukea resurssiryhmän ja se sisältää resursseja.

1. Määritä rooli sovelluksen, siirry perinteinen-portaalista [Azure portal](https://portal.azure.com).

1. Tarkista, varmista, että voit määrittää palvelun lyhennys roolin käyttöoikeudet. Valitse **omat käyttöoikeudet** tilissäsi.

    ![Omat käyttöoikeudet](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Tarkastele määritettyjä käyttöoikeuksia tilissäsi. Kuten edellä mainittiin, omistaja tai käyttäjä käyttää järjestelmänvalvojan rooleihin kuuluvat tai on mukautettu rooli määrää myöntää Microsoft.Authorization kirjoitusoikeudet. Seuraava kuva esittää tilille, jolla määritetään osallistujan rooli tilaukseen, joka ei ole tarvittavia oikeuksia sovelluksen roolin määrittäminen.

    ![Näytä omat käyttöoikeudet](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Jos et avulla myöntää käyttöoikeuksia sovellukseen, sinun täytyy joko pyynnön, että tilauksen järjestelmänvalvoja lisää sinut käyttäjä käyttää järjestelmänvalvojan roolin käyttöoikeuksista, tai pyydä, järjestelmänvalvoja antaa sovelluksen käyttöä.

1. Siirry laajuus haluat liittää sovelluksen taso. Jos haluat määrittää tilauksen alueessa rooli, valitse **tilaukset**.

     ![Valitse tilaus](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Valitse Määritä sovelluksen tilauksessa.

     ![Valitse tilaus varauksen](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Valitse oikeassa yläkulmassa **Access** -kuvaketta.

     ![Valitse käyttöön](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Tai jos haluat määrittää roolin resurssin ryhmän alueessa, siirry resurssiryhmä. Valitse resurssi-ryhmän sivu **käyttöoikeuksien hallinta**.

     ![Valitse käyttäjät](./media/resource-group-create-service-principal-portal/select-users.png)

     Seuraavat vaiheet ovat samat kaikki alueen.

2. Valitse **Lisää**.

     ![Valitse Lisää](./media/resource-group-create-service-principal-portal/select-add.png)

3. Valitse rooli, **Reader** (tai jostakin rooli, jonka haluat määrittää sovelluksen).

     ![Valitse rooli](./media/resource-group-create-service-principal-portal/select-role.png)

4. Kun näet käyttäjäluettelon, voit lisätä rooliin-sovelluksia ei näytetä. Näet vain ryhmän ja käyttäjät.

     ![Näytä käyttäjät](./media/resource-group-create-service-principal-portal/show-users.png)

5. Etsi sovellus on etsittävä sitä. Kirjoita sovelluksesi nimi ja muuttaa luettelo käytettävissä olevista vaihtoehdoista. Valitse sovellus, kun näet sen luettelosta.

     ![Määritä rooli](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Valitse **OK** valmis roolin määrittäminen. Käyttää roolin resurssiryhmän luettelosta sovelluksesi pitäisi tulla näkyviin.


Saat lisätietoja käyttäjien ja sovellusten rooleille portaalin kautta, [Käytä roolimäärityksiä hallittavan Azure tilauksen resurssien käytön](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Esimerkki sovellukset

Esimerkki seuraavien sovellusten näyttää, miten Kirjaudu palvelun lyhennys.

**.NET**

- [Ottaa käyttöön SSH käytössä AM .NET-malli](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resurssit ja .NET resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java-resurssit - käyttöönotto Azure Resurssienhallinta mallilla - aloittaminen](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java-resurssit - hallita resurssiryhmä - aloittaminen](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Ottaa käyttöön SSH käytössä AM Python käyttämällä mallia](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure resurssi ja Python resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Ottaa käyttöön SSH käytössä AM Node.js käyttämällä mallia](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resursseja ja Node.js resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Ottaa käyttöön SSH käytössä AM Ruby-malli](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure resurssi ja Ruby resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja suojauskäytäntöjen määrittäminen on artikkelissa [Azure Roolipohjainen käyttöoikeuksien valvonta](./active-directory/role-based-access-control-configure.md).  
- Katso nämä vaiheet videoesitys [Ohjelmallisesti hallinnan ottaminen käyttöön Azure-resurssien Azure Active Directory-hakemistosta](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).


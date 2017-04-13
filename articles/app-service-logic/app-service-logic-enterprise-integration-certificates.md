
<properties
    pageTitle="Varmenteiden käyttäminen ja yrityksen integrointi Pack | Microsoft Azure"
    description="Opettele käyttämään varmenteet yrityksen integrointi Pack ja logiikka sovellukset"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Tietoja varmenteet ja yrityksen Integration Pack

## <a name="overview"></a>Yleiskatsaus
Yrityksen integrointi käyttää sertifikaatteja B2B tietoliikenteen suojaamista. Voit käyttää kahdentyyppisiä varmenteita yrityksen integrointi sovellusten:

- Julkinen todistuksista, joilla on hankittu varmenteiden myöntäjältä (CA).
- Yksityinen todistuksista, joilla voit myöntää itse. Nämä varmenteet kutsutaan joskus itse allekirjoitettua varmenteet.


## <a name="what-are-certificates"></a>Mitä ovat varmenteet?
Digitaalisten asiakirjojen, Vahvista jäsenyys sähköisen viestinnän osallistujien ja joka suojatun sähköisen viestinnän varmenteet.

## <a name="why-use-certificates"></a>Varmenteiden käyttötarkoitus
Joskus B2B viestintä on luottamuksellisena. Yrityksen integrointi käyttää varmenteet suojaamiseen tietoja kahdella tavalla:

- Viestien sisällön salaamalla
- Mukaan viestien digitaalinen allekirjoittaminen  

## <a name="how-do-you-upload-certificates"></a>Miten lataat varmenteet?

### <a name="public-certificates"></a>Julkinen varmenteet
Käyttämään *julkisten varmenteen* logiikan sovelluksia B2B ominaisuuksia, sinun on Lataa sertifikaatti integrointi-tilillesi. Voit käyttää *itse allekirjoitettua varmennetta*, toisaalta, ensin lataamalla sen [Azure avaimen säilö](../key-vault/key-vault-get-started.md "avaimen säilö tietoja").

Kun olet ladannut varmennetta, se on käytettävissä avulla voit suojata B2B viestit määrittäessäsi ominaisuuksiensa [toimeenpano](./app-service-logic-enterprise-integration-agreements.md) , jotka luot.  

Seuraavassa on ladataan julkisen sertifikaatteja integrointi tilille kirjauduttuasi sisään Azure-portaaliin seuraavasti:

1. Valitse **Selaa**.  
    ![Valitse Selaa-painike](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. Kirjoita **integrointi** suodattimen hakuruutuun ja valitse sitten kohteen tulosten luettelosta **Integrointi tilit** .     
    ![Valitse Integration-tilit](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Valitse integration tili, johon haluat lisätä varmenne.  
    ![Valitse integration-tili, johon haluat lisätä varmenne](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Valitse **Varmenteet** -ruutu.  
    ![Valitse varmenteet-ruutu](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. **Varmenteet** -sivu, joka avautuu Valitse **Lisää** -painiketta.
    ![Valitse Lisää-painike](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Kirjoita sertifikaatin **nimi** ja valitse sitten varmenteen tyyppi. (Tässä esimerkissä on käytetty julkisten varmenteen tyyppi.) Valitse kansiokuvake **varmenteen** tekstiruudun oikeassa reunassa.

7. Kun tiedostonvalitsin avautuu, Etsi ja valitse varmennetiedosto, jotka haluat ladata integrointi-tiliisi.

8. Valitse varmenne ja valitse sitten **OK** tiedoston valitsimella. Tämä tarkistaa ja Lataa sertifikaatti integrointi-tiliisi.

8. Valitse lopuksi takaisin **Lisää varmenne** -sivu, valitse **OK** -painiketta.  
    ![Valitse OK-painike](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. Noin minuutin, näkyy ilmoitus, joka ilmaisee, että sertifikaatti lataus on valmis.

10. Valitse **Varmenteet** -ruutu. Raportissa pitäisi näkyä lisätyn varmenne.  
    ![Katso uutta varmennetta.](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Yksityinen varmenteet
Voit ladata yksityinen varmenteet integrointi-tilillesi noudattamalla seuraavia ohjeita:  

1. [Lataa avain säilö oman yksityinen avain] (../key-vault/key-vault-get-started.md "Lisätietoja avaimen säilö").  

    > [AZURE.TIP] Sinun on sallittava Azure App palvelun avaimen säilö toimintojen suorittamiseen logiikan sovellukset-ominaisuus. Käyttöoikeuden myöntäminen voi logiikan sovellusten palvelun lyhennys PowerShell-komennon avulla:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Yksityinen varmenteen luominen.  

3. Lataa yksityinen sertifikaatti integrointi-tiliisi.

Kun otetut edelliset vaiheet, voit yksityinen sertifikaatin luominen sopimuksia.

Seuraavassa on lueteltu ladataan yksityinen varmenteet integrointi tilille kirjauduttuasi sisään Azure-portaaliin seuraavasti:  

1. Valitse **Selaa**.  
    ![Lataa integrointi tilille yksityinen sertifikaattien tarkasteleminen](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. Kirjoita **integrointi** suodattimen hakuruutuun ja valitse sitten kohteen tulosten luettelosta **Integrointi tilit** .     
    ![Valitse Integration-tilit](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Valitse integration tili, johon haluat lisätä varmenne.  
    ![Valitse integration-tili, johon haluat lisätä varmenne](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Valitse **Varmenteet** -ruutu.  
    ![Valitse varmenteet-ruutu](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. **Varmenteet** -sivu, joka avautuu Valitse **Lisää** -painiketta.
    ![Valitse Lisää-painike](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Kirjoita sertifikaatin **nimi** ja valitse sitten varmenteen tyyppi. (Tässä esimerkissä on käytetty julkisten varmenteen tyyppi.) Valitse kansiokuvake **varmenteen** tekstiruudun oikeassa reunassa.

7. Kun tiedostonvalitsin avautuu, Etsi ja valitse varmennetiedosto, jotka haluat ladata integrointi-tiliisi.

8. Kun olet valinnut varmennetta, tiedosto valitsimella valitsemalla **OK** . Tämä toiminto tarkistaa varmenteen ja lataa integrointi-tiliisi.

9. Valitse lopuksi takaisin **Lisää varmenne** -sivu, valitse **OK** -painiketta.  
    ![Lisää varmenne](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. Noin minuutin, näkyy ilmoitus, joka ilmaisee, että sertifikaatti lataus on valmis.

11. Valitse **Varmenteet** -ruutu. Raportissa pitäisi näkyä lisätyn varmenne.
    ![Katso uutta varmennetta.](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Kun olet ladannut varmennetta, se on käytettävissä avulla voit suojata B2B viestit määrittäessäsi ominaisuuksiensa [sopimuksia](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Seuraavat vaiheet
- [Luo logiikan-sovellusta, joka käyttää B2B ominaisuuksia](./app-service-logic-enterprise-integration-b2b.md)  
- [Luo B2B sopimus](./app-service-logic-enterprise-integration-agreements.md)  
- [Lisätietoja avaimen säilö] (../key-vault/key-vault-get-started.md "Lisätietoja avaimen säilö")  

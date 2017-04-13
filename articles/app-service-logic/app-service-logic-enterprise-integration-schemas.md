<properties
    pageTitle="Yleistä rakenteista ja yrityksen Integration Pack | Microsoft Azure"
    description="Opettele käyttämään rakenteita yrityksen Integration Pack ja logiikka-sovelluksissa"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Tietoja rakenteita ja yrityksen Integration Pack  

## <a name="why-use-a-schema"></a>Rakenteen käyttötarkoitus
Rakenteiden avulla voit vahvistaa, että näyttöön tulee XML-tiedostot ovat kelvollisia ennalta määritetyn muodon odotettu tietojen kanssa. Tarkista viestit, jotka vaihdetaan B2B tilanne käytetään rakenteet.

## <a name="add-a-schema"></a>Lisää rakenne
Azure-portaalista:  

1. Valitse **Lisää palveluja**.  
![Näyttökuva, Azure-portaaliin, "Lisää palveluja" korostettuna](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. Kirjoita suodatin-hakuruutuun **integrointi**ja tulosten luettelosta **Integrointi tilit** .     
![Näyttökuva hakuruudusta suodatin](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Valitse **integration tili** , johon lisäät rakenteeseen.    
![Näyttökuva integrointi tilien luettelo](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Valitse **Mallit** -ruutu.  
![Näyttökuva, IEP integrointi-tili "Mallit" korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Lisää rakennetiedostoon, joka on pienempi kuin 2 Mt  

1. Valitse **Mallit** -sivu, joka avautuu (edellä kuvatut toimet), **Lisää**.  
![Rakenteet näyttökuva sivu, jossa "Lisää"-painike korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Kirjoita rakenteen nimi. Valitse sitten rakennetiedoston lataaminen **rakenteen** tekstiruudun vieressä olevaa kansiokuvaketta. Kun lataus on valmis, valitse **OK**.    
![Näyttökuva "Lisää rakenne", "Pieni tiedoston" korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Lisätä rakennetiedoston suurempia kuin 2 Megatavun (enintään 8 Mt)  

Tämä prosessi riippuu blob-säilö käyttöoikeustaso: **julkisen** vai **ei anonyymin käytön**. Voit selvittää tämä käyttöoikeustaso **Azure-tallennustilan Explorer**- **Blob-objektien säilöjen**, valitse haluamasi blob-säilö valitseminen Valitse **Suojaus**ja valitse **Käyttöoikeustaso** -välilehti.

1. Jos access blob suojaustaso on **julkinen**, toimi seuraavasti.  
  ![Näyttökuva Azure tallennustilan Explorerista, "Blob säilöjen", "Suojaus" ja "Julkinen" korostettuna](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    a. Mallin lataaminen tallennustilan ja kopioi URI.  
    ![Näyttökuva tallennustilan tilin URI korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. Valitse **Lisää rakenne** **suurten tiedostojen**ja anna URI **Sisällön URI** -tekstiruutuun.  
    ![Näyttökuva rakenteita, "Lisää" ja "Suuri tiedosto" korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Jos access blob suojaustaso on **ei anonyymin käytön**, toimi seuraavasti.  
  ![Näyttökuva Azure tallennustilan Explorerista, "Blob säilöjen", "Suojaus" ja "Ei ole anonyymin käytön" korostettuna](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    a. Mallin lataaminen tallennustilan.  
    ![Näyttökuva tallennustilan tilin](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Luo jaettu käyttö allekirjoituksen rakenteen.  
    ![Näyttökuva tallennustilan sccount kanssa jaettujen käytön allekirjoitukset-välilehti näkyy korostettuna](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c-näppäinyhdistelmää. Valitse **Lisää rakenne** **suuri tiedosto**ja antaa jaettua käyttöä allekirjoituksen URI **Sisällön URI** -tekstiruutuun.  
    ![Näyttökuva rakenteita, "Lisää" ja "Suuri tiedosto" korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. EIP integrointi tilin **Mallit** -sivu pitäisi tulla näkyviin lisätyn rakenne.  
![Näyttökuva, EIP integrointi-tili "Mallit" ja uusi rakenne näkyy korostettuna](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Muokkaa malleja
1. Valitse **Mallit** -ruutu.  
2. Valitse rakenne- **Mallit** -sivu, joka avaa muokattavan.
3. Valitse **Mallit** -sivu **Muokkaa**.  
![Rakenteet näyttökuva sivu](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Valitse muokattavat käyttämällä tiedoston valitsin-valintaikkuna, joka avaa rakennetiedostoon.
5. Valitse **Avaa** tiedostonvalitsin.  
![Näyttökuva tiedostonvalitsin](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Saat ilmoituksen, joka ilmaisee lataus onnistui.  

## <a name="delete-schemas"></a>Poista rakenteet
1. Valitse **Mallit** -ruutu.  
2. Valitse **Mallit** -sivu, joka avautuu, valitse poistettava rakenne.  
3. Valitse **Mallit** -sivu valitsemalla **Poista**.
![Rakenteet näyttökuva sivu](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Vahvista valintasi valitsemalla **Kyllä**.  
![Näyttökuva "Poista rakenteen" vahvistussanoma](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Lopuksi Huomaa, että olevien **mallien** sivu rakenteiden luettelossa päivittää, ja poistit rakennetta ei enää näy.  
![Näyttökuva, EIP integrointi-tili "Mallit" korostettuna](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lue lisää yrityksen integrointi pakettiin").  

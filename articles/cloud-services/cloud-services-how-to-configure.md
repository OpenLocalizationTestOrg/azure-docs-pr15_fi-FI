<properties 
    pageTitle="Pilvipalvelussa (perinteinen portaalin) määrittämisestä | Microsoft Azure" 
    description="Opettele määrittämään pilvipalveluihin Azure-tietokannassa. Opi pilveen palvelun määritykset päivittäminen ja määrittäminen etäkäyttöpalvelimen roolin esiintymissä." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-configure-cloud-services"></a>Pilvipalveluihin määrittäminen

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-configure-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-configure.md)

Voit määrittää pilvipalveluun yleisimmin käytettyjä asetuksia Azure perinteinen-portaalissa. Tai jos haluat päivittää määritys-tiedostot suoraan, lataa palvelun määritystiedoston päivittää, ja lataa päivitetyt tiedosto ja Päivitä pilvipalvelussa määritysmuutoksia. Kummassakin tapauksessa määritysten päivitykset siirretty kahdessa rooli.

Azure perinteinen portal mahdollistaa myös käyttöön [Etätyöpöytäyhteys roolin Azure Cloud Services-palveluissa](cloud-services-role-enable-remote-desktop.md)

Azure vain varmistaa, että 99.95 prosenttia palvelun saatavuus määritys-päivitysten aikana Jos sinulla on vähintään kaksi jokaisen roolin rooli esiintymät. Joka mahdollistaa yhden virtual machine käsitellä asiakkaan pyyntöjä, kun toinen päivitetään. Lisätietoja on artikkelissa [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Muuta pilvipalveluun

1. [Azure perinteinen portal](http://manage.windowsazure.com/) **Pilvipalveluihin**ja nimeä pilvipalvelussa, **Määritä**.

    ![Määrityssivulla](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    **Määritä** -sivun Määritä valvonta, rooli-asetusten päivittäminen ja Vieras-käyttöjärjestelmän ja perheen roolin esiintymien valitseminen. 

2. **Valvonta**Aseta seurannan tason yksityiskohtainen tai mahdollisimman vähän ja määritä diagnostiikka yhteyden merkkijonoja, joita tarvitaan yksityiskohtainen seurantaa varten.

3. Palvelun roolien (ryhmitelty roolin mukaan) voit päivittää seuraavat asetukset:
    
    >**Asetukset**  
    >Muokkaa muut asetukset, jotka on määritelty service-kokoonpanotiedosto (.cscfg) *ConfigurationSettings* osista arvoja.
    >
    >**Varmenteet**  
    >Voit muuttaa varmenteen allekirjoitus, joka on käytössä roolin SSL-salausta. Jos haluat muuttaa varmennetta, täytyy ladata ensin uusi varmenne (sivulla **Varmenteet** ). Päivitä roolin asetukset näkyvät varmenteen merkkijonon allekirjoitus.

4. **Käyttöjärjestelmä**käyttöjärjestelmän perhe tai versio roolin esiintymien muuttaminen tai valitse **Automaattinen** käyttöjärjestelmäversio Automaattiset päivitykset käyttöön. Käyttöjärjestelmän-asetukset soveltuvat web roolit ja työntekijä roolit, mutta eivät vaikuta näennäiskoneiden.

    Käyttöönoton aikana viimeisimmän käyttöjärjestelmäversio on asennettu roolin kaikki esiintymät ja käyttöjärjestelmät päivittyvät automaattisesti oletusarvon mukaan. 
    
    Jos tarvitset cloud-palvelun suorittamisen eri käyttöjärjestelmän versio yhteensopivuusvaatimukset omassa koodissasi, voit valita käyttöjärjestelmän perhe- ja versio. Kun valitset tietyn käyttöjärjestelmäversio, keskeytetään pilvipalvelussa automaattinen käyttöjärjestelmän päivitykset. Sinun on varmistettava käyttöjärjestelmät vastaanottaa päivityksiä.
    
    Jos olet ratkaissut kaikki yhteensopivuusongelmia, jotka sovelluksia työskentelee viimeisimmän käyttöjärjestelmäversio, voit ottaa automaattisen käyttöjärjestelmän päivitykset, määrittämällä **automaattisen**käyttöjärjestelmäversio. 
    
    ![OS asetukset](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Tallenna määritykset ja push-roolin esiintymät, valitse **Tallenna**. (Valitse **Hylkää** voit peruuttaa muutokset). **Tallenna** ja **hylätä** lisätään komentopalkin asetuksen muuttamisen jälkeen.

## <a name="update-a-cloud-service-configuration-file"></a>Päivitä cloud service-kokoonpanotiedosto

1. Lataa pilveen palvelun määritysten tiedoston (.cscfg), jonka nykyiset määritykset. Cloud-palvelun **määrittäminen** -sivulla Valitse **Lataa**. Valitse **Tallenna**tai **Tallenna nimellä** tallentaaksesi tiedoston.

2. Kun päivität service-kokoonpanotiedosto, lataa ja päivitysten määrittäminen:

    1. Valitse **Määritä** sivun **lataaminen**.
    
        ![Lataa määritys](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. **Kokoonpanotiedosto**käyttää ja valitse sitten päivitetty .cscfg tiedoston **Selaa** .
    
    3. Jos cloud-palvelu sisältää roolit, joilla on vain yksi, valitse määritys-päivitykset, jatka roolien **käyttää asetuksia, vaikka useita rooleja sisältävät yhden esiintymän** -valintaruutu.
    
        Jos olet määrittänyt kaikki rooli on vähintään kaksi esiintymät, Azure voi taata vähintään 99.95 prosenttia saatavuuden pilvipalvelussa sijaitsevaan palvelupäivityksistä määritysten aikana. Lisätietoja on artikkelissa [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).
    
    4. Valitse **OK** (valintamerkki). 


## <a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja käyttöönottamisesta [pilvipalveluun](cloud-services-how-to-create-deploy.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name.md).
* [Hallitse pilvipalvelussa sijaitsevaan](cloud-services-how-to-manage.md).
* [Etätyöpöytäyhteys – Azure pilvipalveluihin roolia ottaminen käyttöön](cloud-services-role-enable-remote-desktop.md)
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate.md).

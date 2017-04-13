<properties
    pageTitle="Kaksivaiheinen vahvistus-asetusten hallinta | Microsoft Azure"
    description="Käytön mukaan lukien yhteystietojen tai määrittäminen laitteisiin Azure Monimenetelmäisen todentamisen hallitseminen."
    services="multi-factor-authentication"
    keywords = "monimenetelmäisen todentamisen asiakas-todennus ongelma Korrelaatiotunnus"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Kaksivaiheinen vahvistus-asetusten hallinta

Tässä artikkelissa on vastauksia kysymyksiin kaksivaiheinen vahvistus tai multi-factor authentication-asetusten päivittäminen. Jos tilisi kirjautumisessa on ongelmia, tarkista [kaksivaiheinen vahvistus on ongelmia](multi-factor-authentication-end-user-troubleshoot.md) vianmäärityksen Ohje.


## <a name="where-to-find-the-settings-page"></a>Asetukset-sivun löytäminen
Sen mukaan, miten yrityksesi määritetty Azure Monimenetelmäisen todentamisen on muutamia merkit, jossa voit muuttaa asetuksia kuten puhelinnumerosi.

Jos tietyn URL-osoite tai vaiheet hallittavan kaksivaiheinen vahvistus on lähetetty IT-järjestelmänvalvoja, noudata näitä ohjeita. Muussa tapauksessa seuraavien ohjeiden mukaisesti, kaikki muut pitäisi toimia. Jos noudattamalla seuraavia ohjeita, mutta ei näy samoja asetuksia, siis työpaikan tai oppilaitoksen mukauttaa oman portal. Pyydä järjestelmänvalvojaa portaaliin Azure Monimenetelmäisen todentamisen linkin.


1. [Https://myapps.microsoft.com](https://myapps.microsoft.com) kirjautuminen  
2. Ylhäällä Valitse **profiili**.  
3. Valitse **lisäsuojauksen vahvistus**.  

    ![Omat sovellukset](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Ylimääräinen suojaustarkistussivu Lataa asetusten kanssa.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Haluan muuttaa puhelinnumeroa tai lisätä Toissijainen numero

On tärkeää määrittää toissijaisen todennus puhelinnumero.  Ensisijainen puhelinnumero ja mobile-sovellus on todennäköisesti saman puhelin, toissijainen puhelinnumero on ainoa tapa osaat pääset palaamaan tilille, jos puhelimen katoaa tai varastamiselta.

> [AZURE.NOTE]
> Jos ei pääsevät Ensisijainen puhelinnumero ja tarvitset apua tiliisi, katso Tutustu ohjeaiheista [kaksivaiheinen vahvistus on ongelmia](multi-factor-authentication-end-user-troubleshoot.md).

**Voit muuttaa ensisijaista puhelinnumerosi seuraavasti:**  

1. Valitse ylimääräinen suojaustarkistussivu Valitse tekstiruutu, jossa on nykyisen puhelinnumerosi ja muokata sitä uusi puhelinnumero.  
2. Valitse **Tallenna**.  
3. Jos kyseessä on numero, jota käytät ensisijainen tarkistusmenetelmän, on tarkistaa uusi numero, ennen kuin voit tallentaa sen.  


**Voit lisätä toissijaista puhelinnumeroa seuraavasti:**  

1. Valitse ylimääräinen suojaustarkistussivu valintaruudut vieressä **vaihtoehtoisia käyttöoikeuden puhelin.**  
2. Anna toissijainen puhelinnumero tekstiruutuun.  
3. Valitse **Tallenna** ja muutokset on tehty loppuun.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Miten Microsoft Authenticator Puhdista vanha laitteesta ja siirry uuden?
Kun poistat laitteestasi tai nollaa laite, se ei poista aktivoinnin uudelleen. Käytettävä artikkelin [siirtäminen uuteen laitteeseen](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app).

## <a name="next-steps"></a>Seuraavat vaiheet
- Saat vinkkejä ja ohjeita [on kaksivaiheinen vahvistus on ongelmia](multi-factor-authentication-end-user-troubleshoot.md)
- Määritä [sovelluksen salasanat](multi-factor-authentication-end-user-app-passwords.md) sovellukset, jotka eivät tue kaksivaiheista vahvistusta varten.

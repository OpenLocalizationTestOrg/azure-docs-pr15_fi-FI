<properties 
    pageTitle="Windows-todennusta ja Azure multi-factor Authentication-palvelin"
    description="Tämä on, jotka auttavat otetaan käyttöön Windows-todennusta ja Azure multi-factor Authentication Server Azure multi-factor authentication-sivu."
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
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-todennusta ja Azure multi-factor Authentication-palvelin

Windows-todennusta-osan avulla järjestelmänvalvoja voi ottaminen käyttöön ja määrittäminen Windows-todennuksen sovelluksia.  Seuraavassa on luettelo kannattaa pitää mielessä ennen Windows-todennuksen määrittäminen.

-  uudelleenkäynnistys tarvita, ennen kuin päätepalvelut Azure-Monimenetelmäisen todentamisen on voimassa.
-  Jos et ole käyttäjän luettelossa 'Vaadi Azure Monimenetelmäisen todentamisen käyttäjän vastine' on valittuna, jonka voi kirjautua tietokoneen uudelleenkäynnistyksen jälkeen.
-  Luotettu IP-osoitteet on riippuvainen onko sovellus voidaan lisätä asiakkaan IP todennusta. Tällä hetkellä vain päätepalvelut tuetaan.  







>[AZURE.NOTE]Tätä ominaisuutta ei tueta Windows Server 2012 R2 suojatun päätepalvelut.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Suojaamiseen sovelluksen Windows-todennuksen, toimimalla seuraavasti.

1. Napsauta Azure multi-factor Authentication Server Windows-todennusta-kuvaketta.
![Windows-todennus](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Valitse Ota käyttöön Windowsin todennus-valintaruutu. Oletusarvoisesti tämä valintaruutu ei ole valittu.
3. Sovellukset-välilehdellä järjestelmänvalvojat voivat määrittää yhden tai useamman-sovelluksia Windows-todennusta varten.
4. Valitse palvelimeen tai sovelluksen – Määritä, onko palvelin/ohjelmien käytössä. Valitse OK.
5. Valitse Lisää... painike.
6. Luotetut IP-osoitteet-välilehdessä voit ohittaa Azure multi-factor Authentication Windows-istunnot peräisin tietyn IP-osoitteet. Esimerkiksi jos työntekijöiden sovellus Office- ja kotona, voit päättää et halua niiden puhelimet soisivat Azure Monimenetelmäisen todentamisen office osoitteessa varten. Voit määrittää office-aliverkon Luotetut IP-osoitteet tapahtumana.
7. Valitse Lisää... painike.
8. Valitse yksi IP, jos haluat ohittaa yksittäinen IP-osoite.
9. Valitse IP-alueen, jos haluat ohittaa koko IP-alue. Esimerkki 10.63.193.1-10.63.193.100.
10. Valitse aliverkon, jos haluat määrittää alueen, IP-osoitteet käyttämällä aliverkon-merkintätapaa. Kirjoita ensimmäinen IP osoitteiden ja valitse avattavasta luettelosta haluamasi verkkopeite.
11. Napsauta OK-painiketta.

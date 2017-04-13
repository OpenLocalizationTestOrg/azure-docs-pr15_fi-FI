<properties
    pageTitle="Vianmääritys: Luo ja muodostaa yhteyttä tietokoneeseen Learning työtilan | Microsoft Azure"
    description="Ratkaisuja yleisiin ongelmiin luominen ja yhteyden Azure koneen Learning-työtilaan"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Vianmäärityksen opas: Luo ja muodosta yhteys tietokoneen Learning-työtila

Tässä oppaassa on joitakin ratkaisuja tapahtui haasteita usein, kun olet määrittämässä Azure koneen Learning työtilat.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Työtilan omistaja

Kun luot uuden tietokoneen Learning työtilan, tunnuksen kirjoitat TYÖTILAN omistaja-kenttä on oltava kelvollinen Microsoft-tilin (aiemmin Windows Live ID), esimerkiksi john-contoso@live.com tai john-contoso@hotmail.com. Se ei voi olla-Microsoft tili, kuten yrityksen sähköpostitilin. Jos haluat luoda maksuttomalla Microsoft-tilillä, siirry [www.live.com](http://www.live.com).

Huomaa, että tili, jota käytetään kirjautumaan Azure-portaaliin työtilan luominen automaattisesti ole *avata* työtilan, jollet Määritä tilin omistaja. Työtilan avaaminen koneen Learning Studiossa, sinun on kirjauduttava Microsoft-Account, joka on määritetty työtilan omistajaksi tai haluat vastaanottaa kutsun liittymään työtilan omistaja. Perinteinen Azure-portaalista voit kuitenkin *hallinta* työtilan, joka sisältää omistajan muuttaminen ja käytön määrittäminen.

Lisätietoja työtilan hallinnasta on kohdassa [Manage Azure koneen Learning työtila].

[Hallitse Azure koneen Learning-työtila]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Sallitun alueet

Koneen oppiminen on tällä hetkellä käytettävissä alueiden rajoitettu määrä. Jos tilaus ei sisällä jotakin näistä alueista, voivat nähdä virhesanoman, "Sinulla ei ole tilaukset sallittu alueilla."

Jos haluat, että alueen lisätään tilaukseen, valitse **Ota yhteyttä Microsoftin tukeen** perinteinen Azure-portaalista, valitse ongelman tyyppi **Laskutus** ja Lähetä pyyntö näyttöön tulevien ohjeiden.

![Ota yhteys Microsoftin tuotetukeen][screen1]

## <a name="storage-account"></a>Tallennustilan tilin

Koneen Learning-palvelu tarvitsee tallennustilan-tilin tietojen tallentamiseen. Voit käyttää aiemmin luotua tallennustilan-tiliä, tai voit luoda uuden tallennustilan tilin, kun luot uuden tietokoneen Learning työtilan (Jos käytät kiintiön Luo uusi tallennustilan-tili).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Työtilan luominen][screen2]

Uuden tietokoneen Learning työtilan luonnin jälkeen voit kirjautuminen tietokoneen Learning Studio työtilan omistajaksi määritetty Microsoft-tilin avulla. Jos käytössä ilmenee tulee virhesanoma "Työtilan ei löydy" (vastaa seuraavassa näyttökuvassa on esitetty), käytä seuraavaa selaimen evästeet poistamiseen.

![Työtilan ei löydy][screen3]

**Jos haluat poistaa selaimen evästeet**

Jos käytät Internet Explorer, napsauta **Työkalut** -painiketta oikeassa yläkulmassa ja valitse **Internet-asetukset**.  

![Internet-asetukset][screen4]

Valitse **Yleiset** -välilehdessä **Poista...**

![Yleiset-välilehti][screen5]

**Poista Poista selaushistoria** -valintaikkunassa **evästeet ja sivustotiedot** on valittuna ja valitse **Poista**.

![Poista evästeet][screen6]

Kun evästeet on poistettu, Käynnistä selain ja siirry sitten [Microsoft Azure koneen oppiminen](https://studio.azureml.net) -sivulta. Kun ohjelma pyytää käyttäjänimeä ja salasanaa, kirjoita määrittämäsi työtilan omistajaksi samalle Microsoft-tilille.

Tavoitteenamme on Tee tietokoneen Learning-helppoa kuin saumaton mahdollisimman. Ota kirjaa kaikki kommentit ja ongelmia osoitteessa Auta meitä yhteyshenkilönä olet paremman [Azure koneen Learning keskustelupalstalle](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) .

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png

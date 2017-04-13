<properties
    pageTitle="Tietoja tiede Virtual koneet Azure-tietokannassa | Microsoft Azure"
    description="Määritä määrittäminen tietojen tiede Virtual koneen"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Tietoja tiede Virtual koneet Azure-tietokannassa

Ohjeet toimitetaan tässä, jotka kuvaavat määrittämisestä kuin IPython muistion palvelimien Azure-AM ja Azure-AM SQL-palvelun kanssa. Windows-virtuaalikoneen on määritetty tukemaan työkaluilla, kuten IPython muistikirjan, Azure-tallennustilan Explorer ja AzCopy sekä muita apuohjelmia, jotka ovat hyödyllisiä tietoja tiede projektien. Azure tallennustilan Explorer ja AzCopy, esimerkiksi on kätevä tapoja tietojen lataaminen Azure tallennustilan paikallisesta tietokoneesta tai lataa se tietokoneeseesi tallennustilan. 

Tämä valikon linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit määrittää tietojen tiede ympäristöissä eri versio käyttää [Ryhmän tietojen tiede prosessi (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Erilaisia Azuren näennäiskoneiden on valmistelun yhteydessä ja voi käyttää osana pilvipohjainen tietojen tiede-ympäristöä käyttävät. Päätös siitä, mitä sävy virtuaalikoneen käyttämään määräytyy mallintaa koneen learning sekä tiedot pilvipalveluun kohdeluettelon kohde tyypin ja tietojen määrän mukaan. 

* Ottaa huomioon, kun arvioimisesta kysymyksiä, artikkelissa [Suunnitteleminen Azure koneen Learning tietojen tiede ympäristösi](machine-learning-data-science-plan-your-environment.md). 
* Katso luettelo joistain tilanteita, joissa voi ilmetä, kun teet kehittyneen analyysin, [tilanteita, joissa Lisäasetukset Analytics-prosessi ja tekniikka Azure koneen oppiminen](machine-learning-data-science-plan-sample-scenarios.md)

Kaksi joukkoa ohjeet toimitetaan:

* Näyttää, miten valmisteleminen Azure virtuaalikoneen IPython muistikirjan ja muita työkaluja tietojen tiede tapauksissa, jossa kuin SQL Azure tallennustilaa lomakkeen avulla voidaan tallentaa tiedot eivät [määrittäminen Azure virtuaalikoneen, IPython muistikirjan palvelimeksi kehittyneen analyysin varten](machine-learning-data-science-setup-virtual-machine.md) .

* [Määritä Azure SQL Serverin virtual-koneen IPython muistikirjan palvelimeksi varten kehittyneen analyysin](machine-learning-data-science-setup-sql-server-virtual-machine.md) esitetään, kuinka voit valmisteleminen Azure SQL Server-virtual machine IPython muistikirjan ja muiden Työkalut, joita voit tehdä tietojen tiede tapauksissa, jossa on SQL-tietokanta voidaan käyttää tietojen tallennusta varten.

Kun valmisteltu ja määritetty, nämä näennäiskoneiden ovat käyttövalmiina IPython muistikirjan palvelinten tarkasteluun ja tietojen käsittely ja muiden tehtävien tarvitaan yhdessä Azure koneen oppiminen ja ryhmän tietojen tiede prosessi (TDSP). Seuraavat vaiheet tietojen tiede prosessin yhdistetään [TDSP oppimispolku](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja voi sisältää ohjeet, joita tietojen siirtäminen SQL Server- tai HDInsight, käsitellä ja kuuntele sinne valmistelussa learning tiedoista Azure Konepohjaisten Oppimistekniikoiden.


> [AZURE.NOTE] Azure-Virtuaalikoneissa laitteiden hinnoittelu kuin **maksaa vain käyttötarkoitus**. Jos haluat varmistaa, että sinulla on ei laskutettava käytettäessä ei virtuaalikoneen, sen on oltava [Azure perinteinen Portal](http://manage.windowsazure.com/) **pysäytetty (Deallocated)** -tilassa. Vaiheittaiset ohjeet tai miten voit poistaa varauksen virtuaalikoneen on artikkelissa [sulkeminen ja poistaa virtual machine, kun ne eivät ole käytössä varauksen](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 

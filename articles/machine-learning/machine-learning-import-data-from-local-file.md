<properties
    pageTitle="Tietojen tuominen koneen Learning Studio paikallisesta tiedostosta | Microsoft Azure"
    description="Voit tuoda paikallisen tiedoston koulutus tietojen Azure koneen Learning Studio."
    keywords="tuominen tietojen, tietojen muoto, tietotyyppejä, tietolähteitä, koulutus tiedot"
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
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Koulutus-tietojen tuominen Azure koneen Learning Studio paikallisesta tiedostosta

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Käyttämään omiin tietoihisi koneen Learning Studiossa, voit ladata datatiedoston etuajassa tietojoukko moduuli luominen työtilan paikalliselle kiintolevylle. 


## <a name="import-data-from-a-local-file"></a>Tietojen tuominen paikallinen tiedosto

Voit tuoda tietoja paikalliselle kiintolevylle toimimalla seuraavasti:

1. Valitse **+ Uusi** koneen Learning Studio-ikkunan alareunassa.
2. Valitse **TIETOJOUKKO** ja **FROM paikallinen tiedosto**.
3. **Lataa uusi tietojoukko** -valintaikkunassa selaamalla ladattava tiedosto
4. Nimi, Määritä tietotyyppi ja kirjoita halutessasi kuvaus. Kuvaus suositellaan – sen avulla voit tallentaa ominaisuuksia tiedoista, jotka kannattaa muistaa käytettäessä tiedot myöhemmin.
5. **Tämä on olemassa olevan tietojoukon uuden version** valintaruutu avulla voit päivittää olemassa olevan tietojoukon uusilla tiedoilla. Valitse tämä valintaruutu ja kirjoita sitten olemassa olevan tietojoukon nimi.

Aikana Lataa näet viestin, että tiedosto on ladattu. Lataa aika riippuu tiedot koon ja yhteyden nopeus-palveluun.
Jos tiedät, että tiedoston kestää kauan, voit tehdä muun muassa sisällä koneen Learning Studio odotettaessa. Kuitenkin selaimen sulkemista aiheuttaa tietojen lataaminen epäonnistuu.

Kun tiedot on ladattu palvelimeen, se on tallennettu tietojoukko moduuli ja voivat käyttää kokeen työtilassa.
Kun muokkaat koe, löydät tietojoukkoja olet luonut moduuli-valikoimasta **Tallennettu tietojoukkoja** -luettelosta **Oma tietojoukkoja** -luettelossa. Voit vedä ja pudota tietojoukko sivulle kokeen alusta, kun haluat käyttää tietojoukon edelleen analytics ja tietokoneen learning.




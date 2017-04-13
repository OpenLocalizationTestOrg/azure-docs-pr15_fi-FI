<properties
    pageTitle="Mitä tehdä, jos Azure palvelun häiriöitä, jotka vaikuttavat Azure avaimen säilö | Microsoft Azure"
    description="Katso, mitä tehdään, jos Azure keskeytetty, jotka vaikuttavat Azure avaimen säilö."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure avaimen säilö käytettävyyttä ja redundancy

Azure avain säilöön-välilehdessä kerrosta arvojen varmistaaksesi, että näppäimet ja tietoja ovat kuitenkin käytettävissä sovelluksen vaikka yksittäisten osien palvelun epäonnistuu.

Sisällön avaimen säilö, replikoida alueella ja toissijainen alue on vähintään 150 Mailia poissa, mutta saman alueen sisällä. Tämä ylläpitää hyvin kestävyys näppäimet ja tietoja.

Yksittäisten osien avaimen säilöön-palvelun epäonnistuu, jos vaihtoehtoinen osien alueella vaihe tukemaan pyyntö Varmista, että ei ole heikkeneminen toimintoja. Sinun ei tarvitse tehdä mitään käynnistettävän tämä. Se tapahtuu automaattisesti ja läpinäkyvä sinulle.

Harvinaisten tapahtuman koko Azure aluetta ei ole käytettävissä, jonka teet Azure avaimen säilö alueella olevat pyynnöt ovat automaattisesti reititetty (*päälle epäonnistui*) toissijainen alue. Kun ensisijainen alue on käytettävissä uudelleen, pyynnöt reititetään back (*takaisin epäonnistui*) ensisijainen alueen. Uudelleen sinun ei tarvitse tehdä mitään, koska tämä tapahtuu automaattisesti.

Liittyy muutama huomautukset huomioon:

* Jos alue-automaattisesti voi kestää muutaman minuutin kuluttua-palvelun kautta epäonnistuu. Pyynnöt, jotka tehdään tänä aikana saattaa epäonnistua, kunnes vikasietotilaa on valmis.
* Automaattisesti päätyttyä avaimen säilö on vain luku-tilassa. Pyynnöt, joita tuetaan tässä tilassa ovat seuraavat:
 * Luettelon avaimen vaults
 * Hae avaimen vaults ominaisuudet
 * Luettelon tietoja
 * Hae tietoja
 * Luettelon avaimet
 * Saat Key (ominaisuudet)
 * Salaaminen
 * Salauksen purkaminen
 * Rivitys
 * Poistaa rivityksen
 * Tarkista
 * Kirjaudu
 * Varmuuskopiointi
* Kun automaattisesti epäonnistui takaisin, (mukaan lukien luku *ja* Kirjoita pyynnöt) pyynnön kaikentyyppisillä ovat käytettävissä.

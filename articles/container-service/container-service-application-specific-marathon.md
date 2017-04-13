<properties
   pageTitle="Sovelluksen tai käyttäjän määrittämä Marathon palvelun | Microsoft Azure"
   description="Luo sovelluksen tai käyttäjän määrittämä Marathon palvelun"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Säilöjen, Marathon, mikro-palveluja, Ohjauskoneen/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Luo sovelluksen tai käyttäjän määrittämä Marathon palvelun

Azure säilö-palvelu sisältää pää-palvelimien, emme määritetään Apache Mesos ja Marathon. Näiden avulla voidaan orchestrate sovellustesi klusterin, mutta ei kannata käyttää perusmuodon palvelimet tähän tarkoitukseen. Esimerkiksi tweaking Marathon määrittäminen edellyttää kirjautumalla järjestelmään perusmuodon palvelimet itse ja tehdä muutoksia--Tämä tehostaa yksilöllinen perusmuodon palvelimissa, jotka ovat hieman erilaiset standardin ja varusteista ja hallita erikseen. Lisäksi vaatii yhden ryhmän määritykset eivät välttämättä ole paras mahdollinen kokoonpano toisen ryhmän.

Tässä artikkelissa kerrotaan sovelluksen tai käyttäjän määrittämä Marathon palvelun lisäämisestä.

Koska tämä palvelu kuuluvat yksittäisen käyttäjän tai ryhmän, ne ovat maksuttomia määrittäminen tavalla, jotka haluavat. Azure säilö palvelun Varmista myös, että palvelu on edelleen käynnissä. Jos palvelun epäonnistuu, Azure säilö-palvelu uudelleen se puolestasi. Yleensä ei jopa huomaat se oli käyttökatkot.

## <a name="prerequisites"></a>Edellytykset

[Ota Azure säilö palvelun esiintymän](container-service-deployment.md) orchestrator sisältötyyppi Ohjauskoneen/OS ja [Varmista, että asiakkaan muodostaa yhteyttä klusterin](container-service-connect.md). Lisäksi seuraavat toimet.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Luo sovelluksen tai käyttäjän määrittämä Marathon palvelun

Aloita luomalla JSON määritystiedoston, joka määrittää, jonka haluat luoda sovelluksen-palvelun nimi. Tähän Käytämme `marathon-alice` framework nimeksi. Tiedoston tallentaminen suunnilleen `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Seuraavaksi asentaa Marathon esiintymän asetuksilla, jotka on määritetty määritystiedostossa Ohjauskoneen/OS CLI avulla:

```bash
dcos package install --options=marathon-alice.json marathon
```

Pitäisi tulla näkyviin oman `marathon-alice` palvelu käynnissä Ohjauskoneen/OS käyttöliittymän palvelut-välilehti. Käyttöliittymään on `http://<hostname>/service/marathon-alice/` Jos haluat käyttää suoraan.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Määritä toimialueen Ohjauskoneen/OS CLI palvelun käyttämiseen

Voit halutessasi määrittää toimialueen Ohjauskoneen/OS-CLI uuden palvelun käyttämiseen määrittämällä `marathon.url` ominaisuuden siten, että `marathon-alice` esiintymän seuraavasti:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Voit varmistaa, että CLI toimii vastaan Marathon esiintymää `dcos config show` komento. Voit palata komennon käyttäminen perusmuodon Marathon-palvelun `dcos config unset marathon.url`.

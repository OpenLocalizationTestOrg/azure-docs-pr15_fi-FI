<properties
   pageTitle="Julkiset ja yksityiset Ohjauskoneen/OS agentti jakavat ACS | Microsoft Azure"
   description="Miten julkisista ja yksityisistä agentti jakavat suoritetaan Azure säilö Service-klusterin."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Toimialueen Ohjauskoneen/OS agentti jakavat Azure säilö-palveluun

Toimialueen Ohjauskoneen/OS Azure säilö palvelun jakaa käyttäjäagenttien julkinen tai yksityinen jakavat. Käyttöönoton voidaan tehdä joko resurssivarantoon vaikuttavia helppokäyttötoimintojen säilö-palvelun laitteiden välillä. Koneet voidaan käsiksi Internetissä (julkinen) tai säilyttää sisäinen (yksityinen). Tässä artikkelissa antaa lyhyesti, miksi on julkisista ja yksityisistä resurssivarantoon.

### <a name="private-agents"></a>Yksityinen agenttien vuoksi

Yksityinen agentti solmujen Suorita-reititettävä verkon kautta. Verkoston on käytettävissä vain järjestelmänvalvoja-vyöhykkeen tai julkisen vyöhykkeen reunan reitittimen kautta. Oletusarvon mukaan Ohjauskoneen/OS käynnistää yksityinen agentti solmut-sovellukset. Katso lisätietoja verkkosuojauksen [Ohjauskoneen/OS ohjeissa](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

### <a name="public-agents"></a>Julkinen agenttien vuoksi

Julkinen agentti solmujen Suorita Ohjauskoneen/OS sovelluksia ja palveluja kaikkien käytettävissä verkon kautta. Katso lisätietoja verkkosuojauksen [Ohjauskoneen/OS ohjeissa](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

## <a name="using-agent-pools"></a>Agentti jakavat käyttäminen

Oletusarvon mukaan **Marathon** ottaa käyttöön minkä tahansa uuden sovelluksen *Yksityinen* agentti solmut. Sinun on erikseen käyttöön *julkinen* solmu sovelluksen sovelluksen luonnin aikana. Valitse **Valinnainen** -välilehti ja kirjoita **slave_public** **Hyväksynyt resurssin roolit** -arvo. Tätä prosessia on kuvattu [seuraavassa](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ja [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) asiakirjoissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisätietoja [toimialueen Ohjauskoneen/OS säilöjen hallinta](container-service-mesos-marathon-ui.md).

Lisätietoja [Avaa palomuurin](container-service-enable-public-access.md) myöntämä Azure julkisen annettavien Ohjauskoneen/OS säilö.
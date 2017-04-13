<properties
   pageTitle="Liikenteen hallinta päätepisteen ottaminen käyttöön tai poistaa käytöstä | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten liikenteen hallinta-profiilin päätepisteet ottaminen käyttöön tai poistaa käytöstä."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Liikenteen hallinta päätepisteen ottaminen käyttöön tai poistaminen käytöstä

Voit myös poistaa yksittäisiä päätepisteet liikenteen hallinta profiilin osana olevat. Päätepisteet ovat pilvipalveluiden ja sivustot. Päätepisteen poistaminen käytöstä jättää sen osana profiilin, mutta profiilin toimii aivan kuin päätepiste ei sisälly ei. Tämä toiminto on erittäin hyödyllinen poistaminen väliaikaisesti päätepisteen, joka sijaitsee ylläpidon tila tai on otettava käyttöön uudelleen. Kun päätepisteen on miltei, se voidaan ottaa käyttöön

>[AZURE.NOTE] **Päätepisteen poistaminen käytöstä ei mitään tehdään sen käyttöönoton tilan Azure-tietokannassa. Luo siis kelvollinen päätepisteen säilyy ylös- ja vastaanottaa liikenne myös silloin, kun käytöstä liikenteen hallinta. Lisäksi päätepisteen toisen profiilin poistaminen käytöstä ei vaikuta toiseen profiiliin sen tila.**

## <a name="to-disable-an-endpoint"></a>Päätepisteen poistaminen käytöstä

1. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää, jota haluat muokata päätepistettä asetukset ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
1. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka sisältyvät kokoonpanosi päätepisteet.
1. Valitse päätepiste, jonka haluat poistaa käytöstä ja valitse sitten sivun alareunassa **Poista käytöstä** .
1. Liikenne lopetetaan juoksutus päätepisteen perusteella DNS--elinaika (TTL) määritetty liikenteen hallinta toimialuenimi. Voit muuttaa TTL-liikenteen hallinta profiilin määritys-sivulla.

## <a name="to-enable-an-endpoint"></a>Jos haluat ottaa käyttöön päätepisteen


1. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää, jota haluat muokata päätepistettä asetukset ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
1. Valitse sivun yläreunassa **päätepisteet** tarkastelemiseen, jotka sisältyvät kokoonpanosi päätepisteet.
1. Valitse päätepiste, jonka haluat ottaa käyttöön ja valitse sitten sivun alaosassa **Ota käyttöön** .
1. Liikenne alkaa juoksutus uudelleen nimellä sanellun mukaan profiili-palveluun.

## <a name="next-steps"></a>Seuraavat vaiheet

[Liikenteen hallinta - Disable, ota käyttöön tai poistaa seuraavasti:](disable-enable-or-delete-a-profile.md)

[Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)

[Liikenteen hallinta suorituskykyyn liittyviä tietoja](traffic-manager-performance-considerations.md)
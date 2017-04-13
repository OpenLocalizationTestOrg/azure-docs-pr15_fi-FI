<properties
 pageTitle="Ajoituksen rajoitukset ja oletusarvot"
 description="Ajoituksen rajoitukset ja oletusarvot"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>Ajoituksen rajoitukset ja oletusarvot

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Ajoituksen kiintiön, rajoitukset, oletusarvot ja ylikuormitustilan

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>X-ms-pyynnön-tunnus-ylätunniste

Jokainen pyyntö vastaan ajoitus-palvelu palauttaa**x-ms-pyynnön-tunnus**-nimisen vastauksen otsikon. Tämä ylätunniste on peittävä arvo, joka yksilöi pyynnön.

Jos olet varmistanut, että pyyntö oikein muotoilla pyyntö johdonmukaisesti kaatuvat, saattaa raportoi virheestä Microsoftin tätä arvoa käytetään. Raporttiin esimerkiksi x-ms-pyynnön-tunnus, keskimääräinen aika, pyynnön arvo, tilaus, työn sivustokokoelman ja/tai projektin ja haluamasi toiminto, joka pyynnön yritti tunnus.

## <a name="see-also"></a>Katso myös


 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Azure ajoitus-käsitteistä, terminologiaan ja kohteen hierarkia](scheduler-concepts-terms.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Azure ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Azure ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)

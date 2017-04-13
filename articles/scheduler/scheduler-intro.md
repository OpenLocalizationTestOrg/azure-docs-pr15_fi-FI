<properties
 pageTitle="Azure ajoituksen ominaisuudet | Microsoft Azure"
 description="Azure ajoituksen voit määrittämisen avulla kuvataan toimintojen suorittamiseen pilveen. Se sitten ajoittaa ja suorittaa näitä toimia automaattisesti."
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
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Azure ajoituksen ominaisuudet

Azure ajoituksen voit määrittämisen avulla kuvataan toimintojen suorittamiseen pilveen. Se sitten ajoittaa ja suorittaa näitä toimia automaattisesti.  Ajoituksen tekee tämän käyttämällä [Azure portaalin](scheduler-get-started-portal.md), koodi, [REST API](https://msdn.microsoft.com/library/mt629143.aspx)tai PowerShellin Azure.

Ajoitus Luo, ylläpitää ja käynnistää ajoitetun työn.  Ajoitus ei isännöi minkä tahansa työmääriä tai suorittaa koodia. Vain _käynnistää_ koodin muualla isännöity IT – Azure-paikallisen, tai toisessa palvelussa. Käynnistää HTTP, HTTPS, tallennustilan jonossa, palvelun bus jonossa tai palvelun bus aiheen kautta.

Ajoitus ajoittaa [työt](scheduler-concepts-terms.md), säilyttää suorittamisen seurauksena jokin voit tarkistaa, että deterministically ja luotettavasti ajoittaa työmääriä suoritetaan. Azure WebJobs (Azure App palvelun Web Apps-toiminnon osana) ja muita ominaisuuksia ajoituksen Azure käyttää ajoitus taustalla. [Ajoituksen REST API](https://msdn.microsoft.com/library/mt629143.aspx) avulla voit hallita näitä toimia viestintä. Näin ollen ajoitus tukee [monimutkaisia aikatauluja ja kehittyneet toistuminen](scheduler-advanced-complexity.md) helposti.

On useita tilanteita, joissa lainata itse ajoituksen käyttö. Esimerkki:

+ _Toistuva sovelluksen toiminnot:_ Twitter-tietojen on kerääminen säännöllisesti syötteen.
+ _Päivittäin ylläpito:_ Päivittäinen Hakemistopoistojen lokiin-varmuuskopiot ja muita ylläpitotehtäviä. Järjestelmänvalvoja voi esimerkiksi halutessaan Varmuuskopioi tietokanta kello 1 Seuraava yhdeksän kuukauden päivittäin.

Ajoituksen avulla voit luoda, päivittää, poistaa, tarkastella ja töiden ja [työn sivustokokoelmat](scheduler-concepts-terms.md) ohjelmallisesti hallinta käyttämällä komentosarjoja ja portaalissa.

## <a name="see-also"></a>Katso myös

 [Azure ajoituksen käsitteitä ja termejä kohteen hierarkia](scheduler-concepts-terms.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Miten voit luoda monimutkaisia aikatauluja ja Lisäasetukset toistuminen Azure schedulerilla](scheduler-advanced-complexity.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Azure ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Azure ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)

 [Azure ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)

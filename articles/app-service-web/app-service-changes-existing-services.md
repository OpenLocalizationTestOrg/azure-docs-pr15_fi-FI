<properties
    pageTitle="Azure App palvelu ja sen vaikutus aiemmin Azure palvelut"
    description="Kerrotaan Azure-sovelluksen uusi palvelu ja niiden ominaisuuksista vaikutuksista olemassa olevia palveluja Azure-tietokannassa."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure App palvelun ja aiemmin Azure palveluja

Tässä artikkelissa käsitellään muutoksia aiemmin Azure-palveluihin yhdistää useita Azure palveluiden [Sovelluksen Azure-palvelu](https://azure.microsoft.com/services/app-service/), uusi integroitu tarjoaa muuta osana.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Yleiskatsaus

[Azure App palvelu](https://azure.microsoft.com/services/app-service/) on uusi ja ainutkertainen pilvipalvelussa, jonka avulla voit luoda web- ja minkä tahansa ympäristön ja laitteeseen-mobiilisovellukset kehittäjät. Sovelluksen palvelu on integroitu ratkaisu, joka on suunniteltu nopeuttaa toistuvien coding Funktiot ja enterprise- ja SaaS järjestelmien integroida automatisoi liiketoimintaprosesseja tarpeitasi suojaus, luotettavuutta ja skaalattavuus kokouksen aikana.

Sovelluksen palvelun yhdistää seuraavat aiemmin Azure palvelut - [sivustoihin](https://azure.microsoft.com/services/websites/), [Mobile-palveluihin](https://azure.microsoft.com/services/mobile-services/)ja [Biztalk-palveluihin](https://azure.microsoft.com/services/biztalk-services/) yksittäinen yhdistetty palvelun, vaikka lisääminen tehokkaita uusia ominaisuuksia.  Sovelluksen-palvelun avulla voit isännöidä app seuraavanlaisia:

-   Web Apps-sovelluksista
-   Mobile-sovellukset
-   API-sovellukset
-   Logiikan sovellukset

Seuraavassa taulukossa kerrotaan, miten olemassa olevaa sovelluksen palvelun Azure services mallia ja sen käytettävissä sovelluksen tyypit.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Nykyinen Azure-palvelu</th>
<th align="left", style="width:10%">Azure sovelluksen-palvelu</th>
<th align="left", style="width:80%">Mikä on muuttunut</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure sivustot</td>
<td align="left">Web Apps-sovelluksista</td>
<td align="left"><li>Azure sivustot App palvelun on ehdottoman rajoitettu tietokantaobjektin nimen sivustojen verkkosovelluksissa.
<p><li>Kaikki olemassa olevat kopioita sivustojen ovat nyt Web Apps-sovelluksen-palvelussa.</p>
<p><li>Voit käyttää omaa aiemmin sivustojen kautta <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure-portaalissa</a>, saat näkyviin kaikki aiemmin luoduissa sivustoissa <em>Web Apps</em>-kohdassa.</p>
<p><li><em>WWW-isännöinnin suunnitteleminen</em> on nyt <em>Sovellus-palvelun suunnittelu</em>. <em>Sovelluksen-palvelun suunnittelu</em> ylläpitää App palvelun, kuten Web, matkapuhelin, logiikan tai API sovellukset app kaikenlaisia.</p>
<p><li>Azure palvelun Web sovellukset on yleinen käytettävyys.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Lisätietoja verkkosovelluksissa</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile-palvelujen</td>
<td align="left">Mobile-sovellukset</td>
<td align="left"><p><li>Mobile-palveluihin edelleen käytettävissä erillinen palveluna ja ovat täysin tuettu.</p>
<p><li>Mobile-sovellusten on app-sovelluksen-palvelun, jonka kaikki Mobile-palveluihin ja lisää toimintoja.</p>
<p><li>On helppoa siirtämään <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">mobiilisovelluksiin Mobile-palvelut</a>.</p>
<p><li>Osana sovelluksen palvelun Mobile-sovellusten saat uuden ominaisuuksista lisäksi Mobile-palvelut paikallisen ja SaaS järjestelmien, kuten väliaikainen paikkaa, WebJobs tai paremmin skaalausasetukset.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Lisätietoja Mobile-sovellusten</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API-sovellukset</td>
<td align="left">
<p><li>API-sovellukset on uusi sovellus App-palvelussa, jonka avulla voit helposti luoda ja käyttää ohjelmointirajapinnan pilveen.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Lisätietoja API sovellusten</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logiikan sovellukset</td>
<td align="left">
<p><li>Logiikan sovellukset on uusi sovellus App-palvelussa, jolla voit automatisoida helposti Liiketoimintaprosessi.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Lisätietoja logiikan sovellusten</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk palvelut</td>
<td align="left">BizTalk API-sovellukset</td>
<td align="left">
<li><p>BizTalk Services edelleen käytettävissä erillinen palveluna ja ovat täysin tuettu.</p>
<li><p>Kaikki BizTalk palvelujen ominaisuudet integroidaan App Service API sovellusten ottaminen käyttöön käyttäjät voivat suorittaa yrityssovelluksen integrointi ja B2B integrointi skenaarioita kanssa seuraavia sovelluksen sovelluksen-palvelussa</p>
<li><p>Logiikan sovelluksilla voi nyt automatisoi liiketoimintaprosesseja, käyttämällä visual suunnittelukokemusta työnkulkujen luominen</p></td>
</tr>
</tbody>
</table>

Katso lisätietoja seuraavasta [App palvelun ohjeasiakirjoissa](https://azure.microsoft.com/documentation/services/app-service/).

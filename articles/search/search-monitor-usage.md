<properties 
   pageTitle="Seurata käyttöä ja tilastoja Azure-hakupalvelu | Microsoft Azure | Isännöityjen pilvipalvelussa haku" 
   description="Resurssin kulutus ja indeksin koon Azure hakuja isännöityä cloud search-palvelun käyttöön Microsoft Azure seuraaminen" 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Seurata käyttöä ja tilastoja Azure-hakupalvelu

Seurannan indeksit ja asiakirjan koko kasvua auttavat Säädä itse ennen kuin olet muodostanut yhteyttä palvelun yläraja pallolla kapasiteetti. 

Voit seurata resurssien käyttö-määrät ja tilastot palvelun ovat helposti tarkastella [Azure-portaalissa](https://portal.azure.com), mutta voit myös laskea tiedot ohjelmallisesti Jos kokoat asiakaspalvelu hallintatyökalu. Tässä artikkelissa käsitellään tapoja vaiheet.

Voit käyttää myös uusi liikenne analytics hakutoiminnon tiedot kohteeseen activity indeksi tasolla varten. Käy [Haun liikenne Analytics Azure-haun](search-traffic-analytics.md) avulla pääset alkuun.

##<a name="view-counts-and-metrics-in-the-portal"></a>Määrät ja arvot tarkasteleminen portaalissa 

1. Kirjautuminen [Azure Portal](https://portal.azure.com). 

2. Avaa palvelu Raporttinäkymät-ikkunan Azure Search-palvelun. Ruutujen palvelun löytyy kotisivulla tai voit selata palvelu-Valitse JumpBar. Katso vaiheittaiset ohjeet [luominen palvelu](search-create-service-portal.md) .

Käyttö-osassa on mittari, joka kertoo, mikä osa käytettävissä olevien resurssien ovat tällä hetkellä käytössä.

  ![][1]

Peruuttaminen jaettujen palveluiden on enintään replikaan ja kunkin osion. Lisäksi se vain tue 10 000 tiedostoa kokonaan tai 50 Megatavua tietoa, ensimmäistä.

##<a name="get-index-statistics-using-the-rest-api"></a>Hae indeksi tilaston REST-Ohjelmointirajapinnalla

Azure haun REST-Ohjelmointirajapinta ja .NET SDK tarjoavat ohjelmallisesti pääsyn palvelun arvot.  Jos käytössäsi on [Indeksoijilla](https://msdn.microsoft.com/library/azure/dn946891.aspx) Lataa indeksin Azure SQL-tietokanta tai DocumentDB, Lisää Ohjelmointirajapinnan on käytettävissä saat asetat luvut. 

  + [Hae tilastot](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Laske-asiakirjat](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Indeksointitoiminnon tila](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet

Tarkista [rajoitukset ja kapasiteettiin](search-limits-quotas-capacity.md) määrittämään osiot ja tarvitset Jos olemassa olevan kapasiteetin ei ole tarpeeksi replikoiden yhdistelmä. 

Käy [Microsoft Azure-Search-palvelun hallinta](search-manage.md) lisätietoja palvelun hallinta.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 

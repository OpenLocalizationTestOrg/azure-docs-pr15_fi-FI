
<properties
   pageTitle="Azure ohjeiden Elasticsearch | Microsoft Azure"
   description="Azure ohjeiden Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Azure ohjeiden Elasticsearch 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch on erittäin skaalattava Avaa lähde hakukone ja tietokanta. Se on hyödyllinen tilanteissa, jotka edellyttävät nopea analysointi ja suuri tietojoukkoja tietoja etsiminen. Nämä ohjeet tarkastelee avaimen tietyiltä suunnitellessasi Elasticsearch-klusterin, keskittyä optimointi ja testaa asetusten kanssa.

> [AZURE.NOTE]Nämä ohjeet oletetaan, että jotkin Elasticsearch basic tuntemusta. Lisätietoja käy [Elasticsearch sivuston](https://www.elastic.co/products/elasticsearch). 

- **[Käynnissä Elasticsearch Azure-][]** on lyhyt esittely Elasticsearch Yleiset rakenteen ja kerrotaan, miten voit toteuttaa käyttämällä Azure Elasticsearch-klusterin. 
- **[Säädön tietojen nieltynä suorituskyvyn Elasticsearch Azure-][]** kuvaa Elasticsearch-klusterin käyttöönottoa ja sisältää asetuksia ottaa huomioon, kun odotat tietojen annosmuuntokertoimien hyvin korvaus Perusteellisessa analyysissa.
- **[Säädön tietojen koostaminen ja kyselyjen suorituskykyä koskevat Elasticsearch Azure-][]** tarjoaa Perusteellisessa analyysissa, kun vaihtoehtojen päättäminen optimoiminen järjestelmän kyselyn ja Etsi suorituskyvyn.
- **[Toimintakykyyn ja palautus-Elasticsearch Azure-määrittäminen][]** kuvataan Elasticsearch-klusterin, jotka voivat auttaa pienentämään tietojen menettämisen ja laajennetut tiedot palautus aikoja mahdollisuuksia tärkeitä ominaisuuksia.
- **[Azure-Elasticsearch suorituskyvyn testauksen ympäristön luominen][]** käsitellään määrittämään ympäristössä testikäyttöön kyselyn toiminnoista ja suorituskyvyn tietojen nieltynä Elasticsearch-klusterin. 
- Käynnissä olevien suorituskyvyn testien käyttämällä JMeter testi suunnitelmien perustettu JUnit testin suorittamiseen tehtäviä, kuten tietojen lataaminen Elasticsearch-klusterin kyselyjä Java-koodi **[JMeter käyttöönoton testata Elasticsearch suunnitteleminen][]** on yhteenveto.
- **[Käyttöönotto JMeter JUnit värimallin tulostus testikäyttöön Elasticsearch suorituskyvyn][]** kerrotaan, miten voit luoda ja käyttää JUnit värimallin tulostus, joita voit luoda ja tietojen lataaminen Elasticsearch-klusterin. Tämä tarjoaa joustavia lähestymistapa lataamaan testaus, voit luoda suuria määriä testitiedot ilman ulkoisia tietoja tiedostot mukaan. 
- **[Automaattinen Elasticsearch vikasietoisuudelle testien suorittaminen][]** on yhteenveto käytetään sarjassa vikasietoisuudelle testien suorittaminen. 
- **[Automaattinen Elasticsearch suorituskyvyn testien suorittaminen][]** on yhteenveto käytetään sarjassa suorituskyvyn testien suorittaminen.


[Azure Elasticsearch käytössä]: guidance-elasticsearch-running-on-azure.md
[Azure-Elasticsearch tietojen nieltynä suorituskyvyn säätö]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Suorituskyvyn, testaus Elasticsearch Azure-ympäristön luominen]: guidance-elasticsearch-creating-performance-testing-environment.md
[Käyttöönoton JMeter testaussuunnitelmaa Elasticsearch varten]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Käyttöönotto testikäyttöön Elasticsearch suorituskyvyn JMeter JUnit värimallin-tulostus]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Tietojen koostaminen ja Elasticsearch Azure-kyselyn suorituskykyä]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Määrittäminen Elasticsearch Azure-toimintakykyyn ja palauttaminen]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Automaattinen Elasticsearch Vikasietoisuudelle testien suorittaminen]: guidance-elasticsearch-running-automated-resilience-tests.md
[Automaattinen Elasticsearch suorituskyvyn testien suorittaminen]: guidance-elasticsearch-running-automated-performance-tests.md

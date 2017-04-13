<properties
 pageTitle="Tietoja ja HDInsight WebHCat virheiden ratkaiseminen"
 description="Katso, kuinka tietoja yleisten virheiden palauttama WebHCat HDInsight- ja niiden ratkaisemiseksi."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Tietoja ja Ratkaise virhe vastaanotti WebHCat (Templeton), valitse Hdinsightiin

Kun käyttäminen (aiemmalta nimeltään Templeton,) WebHCat HDInsight, näyttöön voi tulla virheitä. Tämä asiakirja sisältää ohjeita yleisten virheiden – miksi ne sijaitsevat ja miten niiden ratkaisemiseksi.

##<a name="what-is-webhcat"></a>Mikä on WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on REST-Ohjelmointirajapinnalla Hadoop [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), taulukon ja tallennustilaa hallinta kerroksen. WebHCat HDInsight klustereiden oletusarvon mukaan käytössä ja käytetään eri työkalujen lähettää työt, Hae kirjautumatta sisään klusterin tilan jne.

##<a name="modifying-configuration"></a>Määritysten muokkaaminen

> [AZURE.IMPORTANT] Monet tässä asiakirjassa virheistä ilmetä, koska määritetty enimmäismäärä on ylitetty. Kun tarkkuus-vaihe maininnat, että voit muuttaa arvoa, sinun on käytettävä jompikumpi seuraavista suorittamiseen muutos:

* For **Windows** -klustereiden: komentosarja-toiminnon avulla voit määrittää arvon klusterin luonnin aikana. Lisätietoja on artikkelissa [kehittäminen komentosarjatoiminnot](hdinsight-hadoop-script-actions.md).

* Saat **Linux** klustereiden: Käytä Ambari (web tai REST API) Muokkaa arvoa. Lisätietoja on ohjeaiheessa [hallinta HDInsight Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Oletusarvon määrittäminen

Määritysten oletusarvot, jotka voivat vaikuttaa suorituskykyyn WebHCat tai aiheuttaa virheitä, jos nämä arvot on ylitetty ovat seuraavat:

| Asetus | Kuvaus | Oletusarvo |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.Capacity.Maximum-sovellukset][maximum-applications] | Töiden, joka voi olla käytössä samanaikaisesti enimmäismäärä (odottaa tai käynnissä) | 10 000 |
| [templeton.Exec.Max-prosesseja][max-procs] | Pyynnöt, jotka voivat samanaikaisesti served enimmäismäärä | 20 |
| [mapreduce.jobhistory.Max-ikä-ms][max-age-ms] | Työhistoria säilytetään päivien määrä | 7 päivää |

##<a name="too-many-requests"></a>Liian monta pyynnöt

**HTTP-tilakoodi**: 429

| Syy | Tarkkuus |
| ----- | ---------- |
| Olet ylittänyt suurin samanaikaiset pyynnöt served mukaan WebHCat minuutissa (oletus 20) | Samanaikaiset pyynnöt enimmäismäärä yli havainnollistamiseen varmistaaksesi, että voit lähettää ei pienennä tai suurenna samanaikainen pyynnön rajoitus muokkaamalla `templeton.exec.max-procs`. Lisätietoja [Modifying määritys](#modifying-configuration) |

##<a name="server-unavailable"></a>Palvelin ei ole käytettävissä

**HTTP-tilakoodi**: 503

| Syy | Tarkkuus |
| ---------------- | ------------------- |
| Tämä tapahtuu yleensä automaattisesti välillä klusterin ensisijainen ja toissijainen HeadNode aikana | Odota kaksi minuuttia ja yritä uudelleen |

##<a name="bad-request-content-could-not-find-job"></a>Pyyntö ei kelpaa sisällön: työn ei löydy

**HTTP-tilakoodi**: 400

| Syy | Tarkkuus |
| ---------------- | ------------------- |
| Projektin tiedot on ole tyhjennetty mukaan Työhistoria puhdistin | Työhistoria säilytys oletusaika on 7 päivää. Tämä voi muuttaa muokkaamalla `mapreduce.jobhistory.max-age-ms`. Lisätietoja [Modifying määritys](#modifying-configuration) |
| Työn on on lopetettu vuoksi automaattisesti | Yritä kaksi minuuttia työn lähetys |
| Virheellinen työn tunnus on käytetty | Työn tunnus oikeellisuuden tarkistaminen |

##<a name="bad-gateway"></a>Virheellinen yhdyskäytävä

**HTTP-tilakoodi**: 502

| Syy | Tarkkuus |
| ---------------- | ------------------- |
| Sisäinen muistista tapahtuu WebHCat prosessin sisällä | Odota muistista valmis tai WebHCat-palvelun käynnistäminen uudelleen |
| Aikakatkaisu Odotetaan vastausta ResourceManager-palvelusta. Näin voi käydä, kun aktiivinen sovellusten määrä määritetty suurin (oletus 10 000) | Odota käynnissä olevat työt loppuun tai suurenna samanaikainen työn rajoitus muokkaamalla `yarn.scheduler.capacity.maximum-applications`. Lisätietoja [Modifying määritys](#modifying-configuration)  |
| Kun yrität hakea kaikista projekteista – [HAE /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) puhelun `Fields` on määritetty`*` | Älä noutaa *kaikki* tiedot, käytä `jobid` noutaa tiedot vain tietyt tunnus suurempi projekteille. Tai Älä käytä`Fields` |
| WebHCat-palvelu ei ole käytettävissä HeadNode automaattisesti aikana | Odota kaksi minuuttia ja yritä uudelleen |
| On yli 500 WebHCat lähetettyjä odottavia töitä | Odota, kunnes odottamassa työt ennen lähettämistä uusia töitä |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 

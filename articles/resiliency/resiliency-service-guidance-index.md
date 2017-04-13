<properties
   pageTitle="Palvelun vikasietoisuudelle ohjeet | Microsoft Azure"
   description="Linkkejä palauttaminen ja ennakoiva vikasietoisuudesta ja käytettävyyden ohjeita Microsoft Azure-palvelut."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Microsoft Azure palvelun vikasietoisuudesta ohjeet
Microsoft Azure on suunniteltu antaa sinulle tarvittavat resurssit, kun tarvitset niitä. Sinun tulee tietää sekä miten arkkitehti käytön, Azure palveluiden saavuttamiseksi suuren käytettävyyden sekä Jos sovelluksesi vaikuttaa keskeytetty osana hyvä rakenne ja toiminnallisia kuvaukset. Helpota tämän prosessin, tässä asiakirjassa on linkkejä tietojen palauttaminen ohjeet sekä rakenne-ohjeet eri Azure palveluihin.

##<a name="disaster-recovery-guidance"></a>Tietojen palauttaminen ohjeet
Tietojen palauttaminen ohjeet alla olevia linkkejä ovat voi antaa sinulle auttavat hyödyntämään sovelluksen online-tilaan nopeasti, jos ovat vaikuttaneet mukaan Azure keskeytetty tarvitsemasi tiedot. Seuraavissa linkeissä on luotu auttaa kysymykseen, "I 'M parhaillaan vaikuttaa mukaan Azure keskeytetty mitä voin tehdä?"

##<a name="design-guidance"></a>Rakenne-ohjeet
Alla rakenne ohjeet linkit ovat rakenne ja arkkitehtuuri ohjeita, jotka on luotu voi auttaa sinua ymmärtämään kuinka kannattaa käyttää kunkin Azure palvelun niin, että suurentaa sovelluksen toiminta-aika. Seuraavissa linkeissä on luotu auttaa kysymykseen "Miten voin määrittää Varmista, että ohjelmavirhe, laitteistovirhe, palvelu tai muu virhe ei vaikuttaa sovelluksessa yleistä käytettävyyttä?" Jos ei tällä hetkellä näyttöä palvelu ei ole tietyn ohjeita, [suuri käytettävyys sovellusten rakennettu Microsoft Azure](./resiliency-high-availability-azure-applications.md) -artikkelissa voi olla lisätiedot, jotka auttavat sinua rakenteessa. 

##<a name="service-guidance"></a>Palvelun ohjeet
| Palvelun  | Tietojen palauttaminen ohjeet | Rakenne-ohjeet |
|:---------|:--------------------------:|:------------------:|
| [Pilvipalveluihin] (https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services")       | [linkki] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure pilvipalveluihin tietojen palauttaminen ohjeet")   | Ei käytettävissä |
| [Avaimen säilö] (https://azure.microsoft.com/services/key-vault/ "Azure avaimen säilö")                      | [linkki] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure avaimen säilö tietojen palauttaminen ohjeet")        | Ei käytettävissä |
| [Tallennustilan] (https://azure.microsoft.com/services/storage/ "Azure-tallennustilan")                            | [linkki] (../storage/storage-disaster-recovery-guidance.md "Azuren tallennustilaan tietojen palauttaminen ohjeet")          | Ei käytettävissä |
| [SQL-tietokannat] (https://azure.microsoft.com/services/sql-database/ "Azure SQL-tietokannat")           | [linkki] (../sql-database/sql-database-disaster-recovery.md  "Azure SQL-tietokannan tietojen palauttaminen ohjeet")    | [linkki] (../sql-database/sql-database-business-continuity.md "Yleistä liiketoiminnan jatkuvuus Azure SQL-tietokanta") |
| [Näennäiskoneiden] (https://azure.microsoft.com/services/virtual-machines/ "Azure-Virtuaalikoneissa") | [linkki] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azuren näennäiskoneiden tietojen palauttaminen ohjeet") | Ei käytettävissä |
| [VPN] (https://azure.microsoft.com/services/virtual-network/ "Azure Virtual verkossa")    | [linkki] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure Virtual Network tietojen palauttaminen ohjeet")  | Ei käytettävissä |

##<a name="next-steps"></a>Seuraavat vaiheet
Jos etsit ohjeita, jotka laajemmin keskitytään järjestelmien ja ratkaisut, lue [palauttaminen ja suuren käytettävyyden sovellusten Microsoft Azure rakennettu](https://aka.ms/drtechguide).

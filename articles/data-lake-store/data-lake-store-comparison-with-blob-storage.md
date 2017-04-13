<properties
   pageTitle="Azure järvi tietovaraston verrattuna Azure Blob-objektien tallennustilaan | Microsoft Azure"
   description="Azure järvi tietovaraston verrattuna Azure Blob-objektien tallennustilaan"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Azure järvi tietovaraston ja Azure-Blob-säiliö vertailu

Tämän artikkelin taulukossa on kuvattu Azure järvi tietovaraston ja Azure-Blob-säiliö erot avaimen tietyiltä suuri Tiedonkäsittelyn pitkin. Azure-Blob-säiliö on yleisiä tarkoitus, skaalattava objektin säilö, joka on tarkoitettu tallennustilan tilanteita, joissa on useita erilaisia. Azure järvi tietosäilö on hyper-akseli säilö, joka on optimoitu big datasta analytics toiminnoista.

|    | Azure järvi tietosäilö  | Azure-Blob-säiliö  |
|----|-----------------------|--------------------|
| Tarkoitus  | Optimoitu big datasta analytics työmääriä tallennustila                                                                          | Yleisiä tarkoitus objektin säilöön tallennustilan tilanteita, joissa on useita erilaisia                                                                                |
| Käyttötapauksiin                                   | Erä, vuorovaikutteinen, streaming analyysin ja tietokoneen learning tietoja kuten lokitiedostot IoT tiedot, valitse virtaa, on paljon | Tekstin tai binaarinen tietoja, kuten sovelluksen kaikenlaisia takaisin loppuun, varmuuskopiotiedot, media tallennustila streaming ja yleisiä tarkoitus tietoja |
| Avaimen käsitteitä                                | Tietosäilö järvi tili sisältää kansioita, joka puolestaan sisältää-tiedostoina tallennettuja tietoja | Tallennustilan tilillä on säilöjen, joka puolestaan on BLOB lomakkeen tiedot |
| Rakenne | Hierarkkinen tiedostojärjestelmässä                                                                                                    | Objektin säilön tasainen nimitila  |
| OHJELMOINTIRAJAPINTA   | REST API https-VÄLITYSPALVELIMEN kautta | REST API päälle HTTP/HTTPS                                                                                                                            |
| Palvelinpuolen Ohjelmointirajapinta                             | [WebHDFS-yhteensopiva REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure-Blob-säiliö REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoop-tiedostosta järjestelmän-asiakasohjelma                   | Kyllä                                                                                                                         | Kyllä                                                                                                                                                 |
| Tietoja toiminnot - todennus            | [Azure Active Directory-käyttäjätietojen](../active-directory/active-directory-authentication-scenarios.md) perusteella | Jaettuja tietoja - [Tilin pikanäppäimet](../storage/storage-create-storage-account.md#manage-your-storage-account) ja [Jaetun allekirjoitus pikanäppäinten](../storage/storage-dotnet-shared-access-signature-part-1.md)perusteella.                                                                       |
| Tietoja toiminnot - protokollaa     | OAuth 2.0. Puhelut on oltava kelvollinen JWT (JSON Web suojaustunnuksen) myöntämä Azure Active Directory                     | Hash-based Message Authentication Code (HMAC). Puhelut on oltava Base64-koodattu SHA-256 hash HTTP-pyyntö osa. |
| Tietoja toiminnot - todennus               | POSIX käyttöoikeusluettelot (käyttöoikeusluettelot).  Azure Active Directory-käyttäjätietojen perusteella käyttöoikeusluettelot voi määrittää tiedostojen ja kansioiden taso. | Tilin tason lupaa – Käytä [Tilin pikanäppäimet](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Tilin, säilön tai blob vahvistus - käyttö [Jaettu allekirjoituksen pikanäppäimet](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Tietoja toiminnot - tarkistaminen                    | Käytettävissä. Katso [seuraavat](data-lake-store-diagnostic-logs.md) tiedot.                                                                                                                   | Käytettävissä                                                                                                                                           |
| Muut tiedot salaus                     | Läpinäkyvä palvelinpuolen (tulossa)<ul><li>Ja palvelua hallita näppäimet</li><li>Asiakkaan hallitun näppäimet Azure KeyVault kanssa</li></ul>| <ul><li>Läpinäkyvä palvelinpuolen</li> <ul><li>Ja palvelua hallita näppäimet</li><li>Asiakkaan hallitun näppäimet Azure KeyVault (tulossa) kanssa</li></ul><li>Asiakkaan salaus</li></ul> |
| Hallintatoiminnot (kuten tilin luominen) | [Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-what-is.md) (RBAC) myöntämä Azure tilin hallinta                                                                       | [Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-what-is.md) (RBAC) myöntämä Azure tilin hallinta                                                                                               |
| Kehittäjän SDK: T                              | .NET-Java-Node.js                                                                                                         | .NET-Java-Python Node.js, C++, Ruby                                                                                                              |
| Analytics työmäärää suorituskyky              | Rinnakkaisia analytics työmääriä optimoitu suorituskyky. Suuri nopeus ja IOPS.                                           | Analytics toiminnoista ei ole optimoitu                                                                                                               |
| Kokorajoitukset                                 | Rajoja tilin koot, tiedostoille tai tiedostojen määrä                                                                   | Tietyt rajoitukset kuvata [tähän](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| GEO redundancy                              | Paikallisesti-tarpeettomat (yksi Azure alueen tiedot useita kopioita)                                                             | Paikallisesti tarpeettomat (LRS), yleisesti tarpeettomat (GRS), luku-käyttäminen yleisesti tarpeettomat (RA-GRS). Katso [tähän](../storage/storage-redundancy.md) lisätietoja |
| Palvelun tilan                               | Julkinen esikatselu                                                                                                              | Yleisesti saatavilla                                                                                                                                 |
| Alueellisten käytettävyys  | Katso [tähän](https://azure.microsoft.com/regions/#services)| Katso [tähän](https://azure.microsoft.com/regions/#services) |
| Hinta                                       |     Katso [hinnat](https://azure.microsoft.com/pricing/details/data-lake-store/)| Katso [hinnat](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Seuraavat vaiheet

- [Yleistä Azure järvi tietosäilö](data-lake-store-overview.md)
- [Tietosäilö järvi käytön aloittaminen](data-lake-store-get-started-portal.md)




<properties
    pageTitle="Azure avain säilöön-ratkaisun Log Analytics | Microsoft Azure"
    description="Lokitiedoston Analytics Azure avain säilöön-ratkaisun avulla voit tarkastella Azure avaimen säilö lokitiedot."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure avaimen säilö (esikatselu)-ratkaisun Log Analytics

>[AZURE.NOTE] Tämä on [Esikatselu-ratkaisun](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Lokitiedoston Analytics Azure avain säilöön-ratkaisun avulla voit tarkastella Azure avaimen säilö AuditEvent lokitiedot.

Voit ottaa valvontalokin tapahtumien kirjaus käyttöön Azure avaimen säilö. Lokitiedostot kirjoitetaan Azure-Blob-säiliö, missä ne sitten voi indeksoida Log Analytics haku-ja analysointia varten.

## <a name="install-and-configure-the-solution"></a>Asenna ja määritä ratkaisu

Seuraavien ohjeiden avulla voit asentaa ja määrittää Azure avain säilöön-ratkaisua:

1.  [Diagnostiikan kirjaus avaimen säilö,](../key-vault/key-vault-logging.md) resursseja, jota haluat seurata ottaminen käyttöön
2.  Määritä lokin analyysitiedot lokit lukea Blob-objektien tallennustilaan käyttämällä [JSON tiedostot Blob-objektien tallennustilaan](../log-analytics/log-analytics-azure-storage-json.md)kuvatut toimet.
3.  Ottaa Azure avain säilöön-ratkaisun käyttämällä [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md)kuvatut toimet.  

## <a name="review-azure-key-vault-data-collection-details"></a>Azure avaimen säilö sivustokokoelman tietojen tarkasteleminen

Azure avain säilöön-ratkaisun kerää Azure-blob-säiliö diagnostiikka lokit Azure avaimen säilö varten.
Tietojen kerääminen tarvitaan ei agentti.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten tiedot kerätään Azure avaimen säilö.

| Käyttöympäristö | Suora agentti | Systems Center Operations Manager (SCOM)-agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | Sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Azure|![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Kyllä](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minuutin|

## <a name="use-azure-key-vault"></a>Käytä Azure avaimen säilö

Kun olet asentanut ratkaisun, voit tarkastella tilat käyttämällä **Azure avaimen säilö** oman valvottu avain Vaults ajan myötä vierekkäin pyynnön yhteenveto Log Analytics **Yleiskatsaus** -sivulla.

![Kuva Azure avain säilöön-ruutu](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Kun olet valinnut **Yhteenveto** -ruutu, voit tarkastella yhteenvetojen lokit ja siirtyminen sitten seuraavat tiedot:

- Äänenvoimakkuuden kaikki avaimen säilö toiminnot ajan mittaan
- Toiminnon tietomääristä epäonnistui ajan mittaan
- Keskimääräinen toiminnallisia viive-toiminnolla
- Toimintoja, joiden toimintoja, joka kestää yli 1000 ms ja luettelo toiminnoista, joita kestää yli 1000 ms palvelun laatua

![Azure avaimen säilö Raporttinäkymät-ikkunan kuva](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Azure avaimen säilö Raporttinäkymät-ikkunan kuva](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Voit tarkastella toiminnon tietoja

1. Valitse **sivulla** Napsauta **Azure avain säilöön** -ruutua.
2. **Azure avaimen säilö** koontinäytössä Tarkista yhteenvetotiedot jossakin näiden ja valitse sitten jokin voit tarkastella yksityiskohtaisia tietoja haku-sivulla.

    Mihin tahansa log etsintäsivuja näet tulokset aika, yksityiskohtaiset tulokset ja log hakujen. Voit myös suodattaa muita tarkentamalla hakutuloksia mukaan.

## <a name="log-analytics-records"></a>Kirjaudu Analytics-tietueet

Azure avain säilöön-ratkaisun analysoi tietueet, joissa on eräänlainen **KeyVaults** , kerätään Azure Diagnostiikka [AuditEvent lokitiedot](../key-vault/key-vault-logging.md) .  Nämä tietueet ominaisuudet ovat seuraavassa taulukossa.  

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | Asiakas, joka pyynnön IP-osoite |
| Luokka      | Avaimen säilö lokitiedot AuditEvent on yksi, käytettävissä-arvo.|
| CorrelationId | Valinnainen GUID, asiakas voi välittää yhdistää asiakaspuolen lokien service (avaimen säilö) lokitietoihin. |
| DurationMs | REST API-pyynnön millisekunteina kulunut aika. Tämä ei ole verkon latenssin, joten aika, joka mitta asiakaspuolen eivät ehkä vastaa tällä hetkellä. |
| HttpStatusCode_d | HTTP-tilakoodin pyynnön palauttama |
| Id_s       | Pyynnön yksilöllinen tunnus |
| Identity_o | Tunnistetiedot, jotka on esitetty, kun REST API-pyynnön tunnuksesta. Tämä on yleensä "käyttäjä", "palvelun lyhennys" tai yhdistelmä "käyttäjän + sovelluksen tunnus" kuten tuloksena Azure PowerShell-cmdlet-pyynnön. |
| OperationName      | Toiminnon, kuten kuvattu [Azure avaimen säilö kirjaaminen](../key-vault/key-vault-logging.md) nimi|
| OperationVersion      | Asiakkaan REST API-versio|
| RemoteIPLatitude | Asiakas, joka pyynnön leveyttä |
| RemoteIPLongitude | Asiakas, joka pyynnön pituutta |
| RemoteIPCountry | Asiakas, joka pyynnön maa  |
| RequestUri_s | Pyynnön URI |
| Resurssi   | Avaimen säilö nimi |
| ResourceGroup | Resurssiryhmä avaimen säilö. |
| ResourceId | Azure Resurssienhallinta resurssin tunnus. Avaimen säilö lokitiedot ei aina avaimen säilö resurssin tunnus. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | HTTP-tila|
| ResultType      | REST API pyynnön tulos|
| SubscriptionId | Azure Tilaustunnus tilauksen sisältävä avaimen säilö |


## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaista tietoa sisältävä Azure avaimen säilö.

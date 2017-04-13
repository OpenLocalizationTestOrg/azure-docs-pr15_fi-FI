<properties
    pageTitle="WWW-palvelun ottamisesta käyttöön alueille | Microsoft Azure"
    description="Ohjeet käyttöön (kopioi) uusi Web-palvelu ja muut alueet."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>WWW-palvelun ottamisesta käyttöön useita alueita

Uusi Azure-verkkopalvelut avulla voit helposti käyttää verkkopalvelun alueille ilman useita tilauksia tai työtilat. 

Hinnat on tietyn vuoksi sinun on määritettävä kunkin alueen, jossa käyttöönottoa WWW-palvelun Laskutus palvelupaketin alue.

## <a name="to-create-a-plan-in-another-region"></a>Luo suunnitelma toisen alueen

1. Kirjaudu sisään [Microsoft Azure Konepohjaisten Oppimistekniikoiden verkkopalveluihin](https://services.azureml.net/).
2. Valitse **suunnitelmien** -valikkovaihtoehto.
3. Näytä sivun päällä-palvelupakettia Valitse **Uusi**.
4. Valitse tilaus, joissa uuden palvelupaketin sijaitsevat **tilauksen** avattavasta valikosta.
5. Valitse uuden palvelupaketin alueen **alueen** avattavasta valikosta. Valitun alueen suunnittelu-asetukset näkyvät sivun **Suunnitteleminen asetukset** -osassa.
6. Valitse palvelupaketin resurssiryhmä **Resurssiryhmä** avattavasta valikosta. Katso lisätietoja resurssiryhmien [hallinta Azure resurssien palvelun portaalissa](../azure-portal/resource-group-portal.md).
7. Kirjoita **Tilauksen nimen** suunnitelman nimi.
8. **Palvelupakettivaihtoehdot**-kohdassa Valitse uuden palvelupaketin laskutuksen taso.
9. Valitse **Luo**.


## <a name="deploying-the-web-service-to-another-region"></a>WWW-palvelun käyttöönoton toisen alueen

1. Valitse **Verkkopalvelut** -valikkovaihtoehto.
2. WWW-palvelun otat käyttöön uuden alueen valitseminen
3. Valitse **Kopioi**.
4. **WWW-palvelunimi**Kirjoita uusi nimi WWW-palvelun.
5. Kirjoita **WWW-Palvelukuvaus**-WWW-palvelun kuvaus.
6. Valitse tilaus, joissa uusi web-palvelu sijaitsevat **tilauksen** avattavasta valikosta.
7. Valitse resurssiryhmä WWW-palvelun **Resurssiryhmä** avattavasta valikosta. Katso lisätietoja resurssiryhmien [hallinta Azure resurssien palvelun portaalissa](../azure-portal/resource-group-portal.md).
8. Valitse **alue** avattavasta valikosta alue, jossa haluat ottaa käyttöön web-palveluun.
9. Valitse tallennustilan-tili, johon haluat tallentaa WWW-palvelun **tallennustilan tilin** avattavasta valikosta.
10. Valitse **Hinta suunnitelman** avattavasta valikosta vaiheessa 8 valitun alueen suunnitelma.
11. Valitse **Kopioi**.


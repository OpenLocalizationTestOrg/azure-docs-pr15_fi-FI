<properties
    pageTitle="Laskutus Microsoft Azure tilausten ilmoitusten määrittäminen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan, miten voit määrittää ilmoitusten Azure laskun jotta vältyt laskutuksen paljon."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Microsoft Azure tilausten laskutuksen ilmoitusten määrittäminen

On kyse siitä, kuinka paljon olet tehtäviinsä kuukausittain Azure tilauksen? Jos ole tilin Azure-tilauksen järjestelmänvalvoja, voit käyttää Azure Laskutus ilmoitusten palvelun voit luoda mukautettuja laskutuksen ilmoituksia, joiden avulla voit seurata ja hallita laskutuksen tehtävän Azure-tileillä.

Tämä palvelu on esikatselu-palvelu, joten sinun on tehtävä ensiksi on Kirjaudu ylös sen. Lisätietoja [Esikatselu ominaisuudet-sivulla](https://account.windowsazure.com/PreviewFeatures) voit ottaa tämän ominaisuuden käyttöön Azure tilin hallinta-portaalissa.

## <a name="set-the-alert-threshold-and-email-recipients"></a>Ilmoitusten vastaanottajat kynnysarvo ja sähköpostin määrittäminen

Kun saat sähköpostin vahvistus laskutuksen palvelun otetaan käyttöön tilauksen, käsiksi [tilaukset-sivulla](https://account.windowsazure.com/Subscriptions) tilin-portaalissa. Napsauta muutettavan tilauksen, joita haluat seurata, ja valitse sitten **ilmoitukset**.

![][Image1]

Valitse seuraavaksi **Lisää ilmoituksen** luominen ensimmäisen vaiheen – voit määrittää korkeintaan viisi laskutuksen ilmoitusten tilauskohtaisten eri raja-arvon ja korkeintaan kahta kunkin ilmoituksen sähköpostin vastaanottajalle.

![][Image2]

Kun olet lisännyt ilmoituksen, voit antaa sille yksilöllisen nimen, valitse kaupankäynnin kulujen raja-arvon ja valitse sähköpostiosoitteet, johon ilmoitukset lähetetään. Järjestäessäsi raja-arvon halutessasi **Laskutus yhteensä** tai **Raha luottotietojen** **Ilmoitusten varten** -luettelosta. Laskutuksen yhteensä ilmoitus lähetetään, kun tilaus tehtäviinsä kynnysarvo. Raha luottoa, saat ilmoituksen lähetetään, kun raha hyvitykset pudota rajan alle. Raha hyvitykset koskevat yleensä vapaa yritykset ja tilaukset, jotka liittyvät MSDN-tilit.

![][Image3]

Azure tukee mitä tahansa sähköpostiosoitetta, mutta ei tarkista, että sähköpostiosoite toimii, tarkista niin kirjoitusvirheitä varten.

## <a name="check-on-your-alerts"></a>Tarkista-ilmoitukset

Määritettyäsi ilmoitusten tilin keskelle luetellaan ne ja näkyy, kuinka monta Lisää voit määrittää. Kunkin ilmoituksen näet päivämäärä ja aika, se on lähetetty, olipa laskutus yhteensä tai raha luottotietojen ilmoituksen ja määrität rajoitus. Päivämäärän ja kellonajan esitysmuoto on 24 tunnin järjestämiseen (UTC-ajasta) ja päivämäärä on yyyy-mm-dd-muodossa. Ilmoituksen luettelon muokkaavan plusmerkkiä tai valitse Poista se Roskakori.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png

<properties
    pageTitle="Azure portal avulla voit luoda ilmoituksia Azure-palveluiden | Microsoft Azure"
    description="Azure portaalin avulla voit luoda Azure ilmoituksia, jotka voivat aiheuttaa ilmoituksia tai automaatio, kun määrittämäsi ehdot täyttyvät."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Azure portal avulla voit luoda Azure-palveluiden ilmoitukset

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Yleiskatsaus

Tämän artikkelin avulla voit määrittää Azure ilmoitusten Azure-portaalissa.   

Voit vastaanottaa ilmoituksen seurantaa mittaukset tai Azure services-tapahtumat.

- **Metrijärjestelmän arvot** - ilmoitusten käynnistimien, kun tietyn mittarin arvo ylittää raja-arvon, voit määrittää jompaankumpaan suuntaan. Toisin sanoen se käynnistää sekä kun ehto täyttyy ja valitse sen jälkeen, kun ehto on enää täyty.    
- **Tehtävän tapahtumien poistaminen** - ilmoituksen esimerkkitilanne *jokaisen* tapahtuman tai, vain silloin, kun tietty määrä tapahtumien ilmetä.


Voit määrittää ilmoituksen, kun se seuraavasti:

- Lähetä sähköposti-ilmoitusten palvelun järjestelmänvalvoja ja apuyhteyshenkilöiden
- Sähköpostin lähettäminen uusia sähköpostiviestejä, jotka määrität.
- Kutsu webhook
- Aloita Azure runbookin, (vain-portaalista Azure) suorittaminen

Voit määrittää ja ilmoitusten sääntöjä käyttämällä koskevien tietojen hakeminen

- [Azure portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [käyttöliittymä (CLI)](insights-alerts-command-line-interface.md)
- [Azure näytön REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Luoda ilmoituksen säännön metrijärjestelmä Azure-portaalissa

1. Etsi [portal](https://portal.azure.com/)resurssin ovat kiinnostuneita seuranta ja valitse se.

2. Valitse seuranta-kohdassa **ilmoituksia** tai **ilmoitusten säännöt** . Teksti ja kuvake saattavat vaihdella hiukan eri resursseille.  

    ![Seuranta](./media/insights-alerts-portal/AlertRulesButton.png)


3. Valitse **Lisää ilmoitus** -komento ja täytä kentät.

    ![Lisää ilmoitus](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nimi** ilmoituksen sääntö ja valitse **kuvaus**, joka näyttää myös ilmoituksen sähköpostitse.
5. Valitse **metrijärjestelmä** haluat valvoa ja valitse sitten lisätiedot **ehto** ja **raja** -arvon. On myös **ajan** , jonka metrisillä sääntö on täytyttävä, ennen ilmoitusten Käynnistimet. Jos käytät kauden "PT5M" ja ilmoituksen näyttää suorittimen yli 80 %, ilmoituksen esimerkiksi tuottaa kun Suoritin on ollut johdonmukaisesti edellä 80 % 5 minuuttia. Kun ensimmäinen käynnistin ilmenee, se tuottaa uudelleen kun Suoritin pysyy 80 % alla 5 minuuttia. Suorittimen mitta ilmenee 1 minuutin välein.   

6. Valitse **sähköpostin omistajat...** , jos haluat Järjestelmänvalvojat ja apuyhteyshenkilöiden sähköpostitse, kun ilmoituksen käynnistyy.

7. Jos haluat lisätietoja sähköpostit lähetetään ilmoitus, kun ilmoituksen käynnistyy, lisätä **muita järjestelmänvalvojan email(s)** -kenttään. Erottaa toisistaan puolipisteillä - useina viesteinä*email@contoso.com;email2@contoso.com*

8. Laittaa kelvollinen URI **Webhook** -kenttään, jos haluat kutsua kun ilmoituksen käynnistyy.

9. Jos käytät Azure automaatio, voit valita Runbookin suorittamaan, kun ilmoituksen käynnistyy.

10. Valitse **OK** kierron luoda ilmoituksen.   

Muutaman minuutin kuluessa ilmoituksen on aktiivinen ja käynnistää yllä olevien ohjeiden mukaan.

## <a name="managing-your-alerts"></a>Ilmoitusasetusten hallinta

Kun olet luonut ilmoituksen, voit valita sen ja:

- Näytä kaavio, jossa näkyy metrisillä kynnysarvo ja edellisenä todelliset arvot.
- Muokkaa tai poista se.
- **Poistaminen käytöstä** tai **ottaminen käyttöön** , se halutessasi tilapäisesti lopettaminen tai Jatka ilmoituksen-ilmoitusten vastaanottaminen.



## <a name="next-steps"></a>Seuraavat vaiheet

* [Saat yleiskatsauksen Azure seuranta](monitoring-overview.md) , mukaan lukien tietotyypeistä voit kerätä ja valvonta.
* Lisätietoja [-ilmoituksia määritettäessä webhooks](insights-webhooks-alerts.md).
* Lisätietoja [Azure automaatio Runbooks](..\automation\automation-starting-a-runbook.md).
* [Vianmäärityslokit yleiskatsaus](monitoring-overview-of-diagnostic-logs.md) ja kerääminen yksityiskohtaiset hyvin usein arvot palvelustasi.
* Varmista, että palvelu on käytettävissä ja vastaa [arvot sivustokokoelman yleiskatsaus](insights-how-to-customize-monitoring.md) hakeminen

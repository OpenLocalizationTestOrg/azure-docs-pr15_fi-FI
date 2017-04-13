<properties
   pageTitle="Näytä käyttöönoton toiminnot-portaalissa | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Azure-portaalin käyttäminen Resurssienhallinta käyttöönotosta virheiden korjaaminen vianmäärityksen avulla."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Näytä käyttöönoton toiminnot Azure-portaalissa

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShellin](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-troubleshoot-deployments-rest.md)

Voit tarkastella toimintojen käyttöönottoa palvelun Azure-portaalissa. Voit tarkastella eniten tarkasteltaessa toimintoja, kun olet saanut virheen käyttöönoton aikana, jotta tässä artikkelissa kerrotaan toiminnoista, joita ei ole onnistunut tarkasteleminen. Portaalin tarjoaa käyttöliittymän, jonka avulla voit helposti etsiä virheitä ja määrittää mahdolliset korjaukset.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Vianmääritys käyttöönoton toimintojen avulla

Käyttöönotto-toimintojen näkyviin noudattamalla seuraavia ohjeita:

1. Käyttöönoton yhdistävää resurssiryhmän Huomaa edellisen käyttöönoton tilan. Voit valita tämän tilan tarkempia.

    ![Käyttöönottotila](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Näet käyttöönoton historiatietojen. Valitse käyttöönoton, joka epäonnistui.

    ![Käyttöönottotila](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Valitse **epäonnistui. Saat lisätietoja napsauttamalla tätä** , kuvaus siitä, miksi asennus epäonnistui. Alla olevassa kuvassa DNS-tietue ei ole yksilöllisiä.  

    ![Tarkastele epäonnistui käyttöönotto](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Tämä virhesanoma tulee näyttöön olisi riittävästi, voit aloittaa vianmääritys. Jos tarvitset lisätietoja siitä, mitkä tehtävät on valmis, voit tarkastella toimintojen mukaisesti seuraavien ohjeiden mukaisesti.

4. Voit tarkastella kaikkia käyttöönotto-toimintojen **käyttöönotto** -sivu. Saat lisätietoja toiminnon valitseminen

    ![Näytä toiminnot](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    Tällöin näet, että tallennustilan tilin, VPN ja käytettävyyden määrittäminen luotiin. Julkiseen IP-osoitteeseen epäonnistui ja muita resursseja on ole yrittänyt.

5. Voit tarkastella tapahtumien käyttöönoton valitsemalla **tapahtumat**.

    ![tapahtumien tarkasteleminen](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Näet kaikki tapahtumat käyttöönottoa varten ja valitse jokin lisätietoja.

    ![Katso tapahtumat](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Käytä valvontalokien vianmääritys

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Virheet käyttöönottoa näkyviin noudattamalla seuraavia ohjeita:

1. Tarkastella resurssin ryhmän valvontalokien lisäasetusten **Valvonta lokitiedot**.

    ![Valitse valvontalokit](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. **Valvontalokien** -sivu tulee näkyviin kaikki resurssiryhmät viimeisimmät toiminnot yhteenveto-tilaukseesi. Se sisältää ajan graafinen ja toimintojen tila- ja toimintojen luettelo.

    ![Näytä toiminnot](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Voit suodattaa tietyn ehdot keskittyminen valvontalokien näkymää. Valitse **suodattimen** yläreunaan **valvontalokien** -sivu.

    ![suodatin-lokit](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Valitse **Suodatin** -sivu ehtoja valvontalokien näkymää rajoittaminen vain toimintoja, joiden haluat olevan näkyvissä. Voit esimerkiksi suodattaa toimintojen Näytä vain resurssiryhmän virheet.

    ![suodatin-asetusten määrittäminen](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Voit suodattaa toimintojen määrittämällä ajanjakson. Seuraava kuva suodattaa tietyn 20 minuutin aikajakson näkymä.

    ![Määritä aika](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Voit valita toimintoja luettelossa. Valitse toiminto, joka sisältää haluat tutkiminen virheen.

    ![Valitse toiminto](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Näet kaikki tapahtumat, toiminnolle. Huomaa **Korrelaatio tunnukset** yhteenvedossa. Tämä on vain liittyvät tapahtumia. Se voi olla hyötyä, kun käsittelet tekniseen tukeen, voit tehdä ongelman vianmäärityksen. Voit valita minkä tahansa tapahtuman näkevän tapahtuman tiedot.

    ![Valitse tapahtuma](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Näet tapahtuman tiedot. Erityisesti huomiota **Ominaisuudet** virheen tietoja.

    ![Näytä valvonta lokitiedot](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Valvontalokin suodatinta säilyttää seuraavan kerran, voit tarkastella sitä, joten joudut ehkä muuttamaan näistä arvoista laajentaa näkymän toimintoja.

## <a name="next-steps"></a>Seuraavat vaiheet

- Ohjeita tietyn käyttöönoton virheiden ratkaisemisesta on artikkelissa [ratkaista yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md).
- Lisätietoja seurannassa muuntyyppisten toiminnot valvontalokien käyttämisestä on artikkelissa [valvonta toimintojen resurssien hallinta](resource-group-audit.md).
- Tarkista ennen sen suorittamista käyttöönoton, on artikkelissa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy.md).

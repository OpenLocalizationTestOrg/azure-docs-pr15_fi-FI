<properties 
    pageTitle="Etätyöpöydän pilvipalveluihin (Node.js)" 
    description="Opettele etäyhteyksien työpöydän käytön isännöintipalvelu Azure Node.js sovelluksen näennäiskoneiden." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Azure-tietokannassa etätyöpöydän ottaminen käyttöön

Etätyöpöytä avulla voit käyttää työpöydän Azure käynnissä roolin esiintymän. Voit määrittää virtuaalikoneen tai sovelluksen ongelmien vianmääritys Etätyöpöytäyhteys.

> [AZURE.NOTE] Tämä artikkeli koskee Node.js sovellusten isännöidään kuin Azure-pilvipalvelussa.


## <a name="prerequisites"></a>Edellytykset

- Asenna ja määritä [PowerShellin Azure](../powershell-install-configure.md).
- Node.js sovelluksen käyttöönotto Azure pilvipalveluun. Lisätietoja on artikkelissa [Muodosta ja ota käyttöön Node.js-sovelluksen Azure-pilvipalveluun](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Vaihe 1: Azure PowerShellin etätyöpöydän access-palvelun määrittäminen

Etätyöpöytä käyttämään haluat päivittää Azure palvelun määritys- ja määritystietoja käyttäjänimi, salasana ja varmenne. 

Suorita seuraavat vaiheet tietokoneessa, jossa lähdetiedostot, kun sovellus.

1. Suorita järjestelmänvalvojana **Windows PowerShell** . ( **Käynnistä-valikko** tai **Aloitusnäytössä**, Etsi **Windows PowerShell**.)

2.  Siirry kansioon, joka sisältää palvelun määritys (.csdef) ja palvelun asetustiedostot (.cscfg).

3. Kirjoita seuraavan PowerShell cmdlet-komennon:

        Enable-AzureServiceProjectRemoteDesktop

4. Tulee näkyviin Anna käyttäjänimi ja salasana.

    ![Ota käyttöön azureserviceprojectremotedesktop][enable-rdp]

3.  Kirjoita julkaisemaan muutokset seuraavan PowerShell cmdlet-komennon:

        Publish-AzureServiceProject

    ![Julkaise azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Vaihe 2: Yhdistäminen rooli-esiintymä

Kun olet julkaissut update-palvelun määritys, voit muodostaa yhteyden rooli-esiintymä.

1.  [Azure perinteinen portal] **Pilvipalveluihin** Valitse ja valitse sitten palvelun.

    ![Azure perinteinen portal][cloud-services]

2.  Valitse **esiintymät**ja valitse sitten **tuotannon** tai **vaiheet** , jotta näet palvelun esiintymät. Valitse esiintymä ja valitse sitten **Yhdistä** sivun alareunassa.

    ![Sivun tapaukset][3]

2.  Kun valitset **Yhdistä**, selaimen kehottaa tallentamaan .rdp-tiedosto. Avaa tiedosto. (Esimerkiksi, jos käytät Internet Explorer, valitse **Avaa**.)

    ![kehotus avata tai tallentaa .rdp-tiedosto][4]

3.  Kun tiedosto avataan, suojaus-kehote:

    ![Windowsin suojaus-kehote][5]

4.  Valitse **Yhdistä**ja näyttöön tulee kehote suojauksen työnkulun esiintymän käyttöoikeudet. Salasana luomasi [vaiheessa 1] [vaihe 1: Määritä etätyöpöydän käytön PowerShellin Azure-palvelu], ja valitse sitten **OK**.

    ![käyttäjänimi ja salasana-kehote][6]

Kun yhteys on muodostettu, Etätyöpöytäyhteys Näyttää työpöydän esiintymän Azure. 

![Työpöydän Etäistunto][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Vaihe 3: Määritä palvelu etätyöpöydän käyttöoikeuksien poistaminen 

Kun et enää tarvitse työpöydän etäyhteyksien pilveen rooli-esiintymissä, poista käytöstä työpöydän etäkäyttöpalvelimen [Azure PowerShellin]avulla.

1.  Kirjoita seuraavan PowerShell cmdlet-komennon:

        Disable-AzureServiceProjectRemoteDesktop

2.  Kirjoita julkaisemaan muutokset seuraavan PowerShell cmdlet-komennon:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Lisäresursseja

- [Käytettäessä etäyhteyden roolin esiintymät Azure-tietokannassa] 
- [Etätyöpöydän käyttäminen Azure roolit]
- [Node.js Developer Center](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure perinteinen portal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Käytettäessä etäyhteyden roolin esiintymät Azure-tietokannassa]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Etätyöpöydän käyttäminen Azure roolit]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 
<properties 
    pageTitle="Pilvipalvelussa (portaalin) määrittämisestä | Microsoft Azure" 
    description="Opettele määrittämään pilvipalveluihin Azure-tietokannassa. Opi pilveen palvelun määritykset päivittäminen ja määrittäminen etäkäyttöpalvelimen roolin esiintymissä. Näissä esimerkeissä Azure portaaliin." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Pilvipalveluihin määrittäminen

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-configure-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-configure.md)

Voit määrittää pilvipalveluun yleisimmin käytettyjä asetuksia Azure-portaalissa. Tai jos haluat päivittää määritys-tiedostot suoraan, lataa palvelun määritystiedoston päivittää, ja lataa päivitetyt tiedosto ja Päivitä pilvipalvelussa määritysmuutoksia. Kummassakin tapauksessa määritysten päivitykset siirretty kahdessa rooli.

Voit hallita cloud palvelun roolit tai etätyöpöydän esiintymien tietoja.

Azure vain varmistaa, että 99.95 prosenttia palvelun saatavuus määritys-päivitysten aikana Jos sinulla on vähintään kaksi jokaisen roolin roolin esiintymät. Joka mahdollistaa yhden virtual machine käsitellä asiakkaan pyyntöjä, kun toinen päivitetään. Lisätietoja on artikkelissa [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Muuta pilvipalveluun

Siirry [Azure portal](https://portal.azure.com/)avattuasi pilvipalvelussa sijaitsevaan. Täällä voit hallita monia sitä. 

![Asetukset-sivulla](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Asetukset** -sivu, jossa voit voit **ominaisuuksien**muuttaminen **määritykset**, **Varmenteet**-asennuksen **ilmoitusten sääntöjä**, ja hallita **käyttäjät** , joilla on tämä pilvipalveluun access Avaa linkkejä **tai **kaikki asetuksia** ** .

![Azure cloud palvelun asetukset-sivu](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Käyttöjärjestelmä, jota käytetään cloud-palvelua ei voi muuttaa **Azure portaalissa**, voit muuttaa asetusta palvelun [Azure perinteinen portal](http://manage.windowsazure.com/)vain. Tämä on tarkat [tähän](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Seuranta

Voit lisätä ilmoituksia cloud-palveluun. Valitse **asetukset** > **Ilmoitus säännöt** > **Lisää ilmoitus**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Tässä voit määrittää ilmoituksen. **Mertic** avattavasta ruudusta, jossa voit määrittää seuraavanlaisia tietojen ilmoituksen.

- Levyn
- Levyn kirjoittaminen
- Verkko-
- Verkon ulos
- Suorittimen prosentti 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Määritä valvonta-metrisillä ruutu

Sijaan **asetukset** > **Ilmoitusten sääntöjä**, voit valita jonkin metrisillä ruudut **pilvipalvelussa** sivu **Seuranta** -kohdassa.

![Pilvipalvelussa seuranta](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Täällä voit mukauttaa käyttämisestä ruudun kaavion tai ilmoitusten säännön lisääminen.


## <a name="reboot-reimage-or-remote-desktop"></a>Uudelleenkäynnistys, reimage tai etätyöpöydän kautta

Tällä hetkellä ei voi määrittää etätyöpöydän **Azure portal**. Voit kuitenkin määrittäminen [Azure perinteinen portal](cloud-services-role-enable-remote-desktop.md)- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)- tai [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)kautta. 

Valitse ensin cloud service-esiintymässä.

![Cloud esiintymää](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Valitse sivu, Avaa uou voi aloittaa työpöydän etäyhteyden, uudelleen etäyhteyden esiintymä tai etäyhteyden reimage (käynnistät ajan tasalla kuvan) esiintymä.

![Cloud palvelun esiintymä-painikkeet](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Oman .cscfg määrittäminen uudelleen

Voit joutua määrittämään voit cloud service- [palvelun config (cscfg)](cloud-services-model-and-package.md#cscfg) -tiedoston avulla. Ensin sinun täytyy ladata .cscfg-tiedoston, muokkaa sitä ja valitse Lataa se.

1. Napsauta **asetukset** -kuvake tai **kaikki asetukset** -linkki avaa **asetukset** -sivu.

    ![Asetukset-sivulla](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Valitse **määritys** -kohteen.

    ![Määritys-sivu](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Valitse **Lataa** -painiketta.

    ![Lataa](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Kun päivität service-kokoonpanotiedosto, lataa ja päivitysten määrittäminen:

    ![Lataa](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Valitse .cscfg-tiedosto ja valitse **OK**.

            
## <a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja käyttöönottamisesta [pilvipalveluun](cloud-services-how-to-create-deploy-portal.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name-portal.md).
* [Hallitse pilvipalvelussa sijaitsevaan](cloud-services-how-to-manage-portal.md).
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate-portal.md).
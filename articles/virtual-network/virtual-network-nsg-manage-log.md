<properties
   pageTitle="Valvoa toimintoja, laskureita ja tapahtumien NSGs | Microsoft Azure"
   description="Lue, miten voit ottaa käyttöön laskureita, tapahtumia ja toiminnallisia kirjaaminen, jotta NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Lokitiedoston analysointitietoja verkon käyttöoikeusryhmät (NSGs)

Voit erityyppisiä lokit Azure hallinta ja vianmääritys NSGs. Jotkin lokitiedostot voi käyttää portaalin ja kaikki lokeja voidaan poimittujen Azure-blob-säiliö, ja tarkastella eri työkaluja, kuten [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)-, Excel- ja PowerBI. Voit lukea lisää eri tyyppiä lokit alla olevasta luettelosta.

- **Valvontalokit:** [Azure valvontalokien](../monitoring-and-diagnostics/insights-debugging-with-events.md) (aiemmalta nimeltään toiminnallisia lokit) avulla voit tarkastella lähetettävä Azure tilauksistasi ja niiden tiloista kaikki toiminnot. Valvontalokien ovat oletusarvoisesti käytössä, ja voit tarkastella Azure esikatselu-portaalissa.
- **Tapahtumalokien:** Tämä loki avulla voit tarkastella tietoja NSG säännöt otetaan käyttöön VMs ja esiintymän roolien perusteella MAC-osoite. Sääntöjen tila kerätään 60 sekunnin välein.
- **Laskuri lokit:** Lokia avulla voit tarkastella, kuinka monta kertaa NSG niiden sääntöjen käytetty Estä tai Salli-liikenne.

>[AZURE.WARNING] Lokit ovat käytettävissä vain resurssien Resurssienhallinta käyttöönoton mallin käyttöön. Et voi käyttää lokit resurssien perinteinen käyttöönotto-mallissa. Saat paremman käsityksen malleista, viittaus on artikkelissa [tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Kirjaamisen ottaminen käyttöön
Valvontalokit on automaattisesti käytössä on aina Resurssienhallinta resurssi. Sinun on otettava käyttöön tapahtuma- ja lokiin kirjaaminen käyttöön kyseiset lokit kautta tietojen kerääminen laskuri. Voit ottaa kirjaamisen käyttöön, noudata seuraavia ohjeita.

1.  Kirjaudu sisään [Azure portal](https://portal.azure.com). Jos sinulla ei vielä ole olemassa olevaa verkon käyttöoikeusryhmää, [Luo NSG](virtual-networks-create-nsg-arm-ps.md) , ennen kuin jatkat.

2.  Valitse esikatselu-portaalissa **Selaa** >> **verkon käyttöoikeusryhmät**.

    ![Esikatselu portal - verkko-käyttöoikeusryhmät](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Valitse aiemmin luotu verkko-käyttöoikeusryhmän.

    ![Esikatselu portal - ryhmän verkkoasetukset](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. Valitse **asetukset** -sivu **Diagnostiikka**ja ** **Diagnostiikka** -ruudun vieressä **tila**, valitse**
5. Valitse **asetukset** -sivu **Tallennustilan-tili**ja valitse olemassa olevan tallennustilan tilin tai luoda uuden.  

>[AZURE.INFORMATION] valvontalokien eivät edellytä erillisen tallennustilan tilin. Tallennustilan käyttö tapahtuma- ja säännön kirjaaminen selvittää palvelun kulut.

6. Valitse avattavasta luettelosta alapuolella **Tallennustilan tilin**, haluatko kirjautua tapahtumia tai laskureita ja valitse sitten **Tallenna**.

    ![Esikatselu portal - diagnostiikka-lokit](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Valvontaloki
Azure luoda lokia (aiemmalta nimeltään "toiminnallisia lokiin") oletusarvoisesti.  Lokit säilytetään 90 päivää Azure's tapahtumalokien Storessa. Lue lisää lokitiedostot lukemalla artikkelin [tapahtumien tarkasteleminen ja valvontalokit](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Laskuri-loki
Tämä loki luodaan vain, jos se on otettu kohti NSG välein edellä esitetyllä tavalla. Tiedot tallennetaan tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. Niiden sääntöjen resursseissa on kirjautunut JSON-muoto, joka näkyy alla.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Tapahtumaloki
Tämä loki luodaan vain, jos se on otettu kohti NSG välein edellä esitetyllä tavalla. Tiedot tallennetaan tallennustilan tilin kirjautumiskerralla kirjaus käyttöön. Kirjataan seuraavat tiedot:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Tarkastella ja analysoida valvontaloki
Voit tarkastella ja analysoida valvontalokin tietoja jollakin seuraavista tavoista:

- **Azure työkalut:** Tietojen noutaminen valvontalokien PowerShellin Azure, Azure komentorivillä (CLI), Azure REST-Ohjelmointirajapinnalla ja Azure esikatselu-portaalissa.  Kummassakin menetelmässä vaiheittaiset ohjeet on kuvattu artikkelin [valvonta-toimintojen resurssien hallinta](../resource-group-audit.md) .
- **Power BI:** Jos sinulla ei vielä ole [Power BI](https://powerbi.microsoft.com/pricing) -tili, voit kokeilla ilmaiseksi. Käyttämällä [Azure valvontalokien sisällön Power BI-paketti,](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) voit analysoida, jota voit käyttää esimääritettyjä raporttinäkymien tietojen-on tai mukauttaminen.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Tarkastella ja analysoida laskuri ja tapahtumaloki

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) voi kerätä laskuri ja tapahtumaloki Blob storage tililtä-tiedostot ja sisältää visualisoinnit ja tehokkaita hakuominaisuuksia lokeista analysointia varten.

Voit muodostaa yhteyden tiliisi tallennustilan ja hakea tapahtuma-ja laskuri JSON log merkintöjä. Kun olet ladannut JSON-tiedostoja, voit muuntaa ne CSV- ja Näytä Excelissä, PowerBI tai jokin muu tietojen visualisoinnin työkalu.

>[AZURE.TIP] Jos olet tutustunut Visual Studio ja peruskäsitteet muuttaminen vakioita ja C# muuttujat arvot, voit käyttää [lokiin muuntimen Työkalut](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github käytettävissä.

## <a name="next-steps"></a>Seuraavat vaiheet

- Laskuri- ja tapahtumalokien kanssa [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) visualisointi
- [Visualisoi Power BI Azure oman valvontalokien](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogimerkintä.
- [Näkymä ja analysoida Azure valvontalokien Power BI-ja lisää](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogimerkintä.

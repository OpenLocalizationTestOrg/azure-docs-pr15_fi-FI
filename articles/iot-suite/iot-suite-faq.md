<properties
  pageTitle="Azure IoT Suite usein kysytyt kysymykset | Microsoft Azure"
  description="Usein kysyttyjä kysymyksiä IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Usein kysyttyjä kysymyksiä IoT Suite

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Mitä eroa resurssiryhmän Azure-portaalissa poistaminen ja valitsemalla Poista azureiotsuite.com esimääritettyjä ratkaisu?

- Jos poistat [azureiotsuite.com]esimääritettyjä-ratkaisun[lnk-azureiotsuite], voit poistaa kaikki resurssit, jotka on valmisteltu esimääritettyjä ratkaisu; luotaessa Jos olet lisännyt muita resursseja resurssiryhmän, ne poistetaan myös. 

- Jos olet poistanut resurssiryhmän [Azure portal][lnk-azure-portal], voit poistaa vain resursseja resurssiryhmän; Sinun on myös poistaa Azure Active Directory-sovelluksen esimääritettyjä ratkaisuun [Azure perinteinen portal][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Kuinka monta IoT keskittimeen esiintymät tilauksen voit valmistella? 

Kymmenen. Voit luoda [Azure tukevat lippu] [ link-azuresupportticket] nostaa rajoitusta, mutta oletusarvoisesti voit voit vain valmistella kymmenen IoT keskittimet tilauskohtaisten, kuvatulla tavalla [Azure tilauksen rajoitukset][link-azuresublimits]. Tuloksena koska jokaisella esimääritettyjä ratkaisu valmistelee keskittimeen IoT, voit voit valmistella vain tietyn tilauksen enintään kymmenen esimääritetyt ratkaisut. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Kuinka monta DocumentDB esiintymää tilauksen voit valmistella?

50. Voit luoda [Azure tukevat lippu] [ link-azuresupportticket] nostaa rajoitusta, mutta oletusarvoisesti vain valmistella 50 tilauskohtaisten DocumentDB esiintymät. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Kuinka monta vapaa Bing Maps-ohjelmointirajapinnan tilauksen voit valmistella?

Kaksi. Voit luoda vain kaksi sisäinen tapahtumat taso 1 Bing Maps Enterprise-palvelupaketteja Azure tilauksen. Remote seurantaa ratkaisu on valmisteltu ja sisäinen tapahtumat tason 1-palvelupaketti oletusarvoisesti. Tuloksena voit valmistella vain kaksi remote seurantaa ratkaisuja tilauksen ilman muutoksia.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Minulla on remote seurantaa ratkaisu-käyttöönoton kiinteän kartan avulla, kuinka lisään vuorovaikutteinen Bing-kartan? 
1. Bing Maps-Ohjelmointirajapinnan hankkiminen yrityksen QueryKey [Azure]-portaalista[lnk-azure-portal]: 
 1. Siirry [Azure portal]Bing Maps-Ohjelmointirajapinnan Enterprise missä resurssiryhmä[lnk-azure-portal].
 2. Valitse kaikki asetukset, sitten hallintaa. 
 3. Huomaat kahta näppäintä: MasterKey ja QueryKey. Kopioi QueryKey arvo.

     > [AZURE.NOTE] Bing Maps-Ohjelmointirajapinta yrityksen tiliä ei ole? Luoda [Azure portal] [ lnk-azure-portal] napsauttamalla + uusi, Bing Maps-Ohjelmointirajapinnan Enterprise hakeminen ja noudata kehotteita, voit luoda.

2. Avattava uusimman koodin [Azure-IoT-Remote-seuranta][lnk-remote-monitoring-github].

3. Suorita paikallisen tai cloud käyttöönoton seuraavan komentorivi asennusohjeita säilössä /docs/-kansiossa. 

4. Kun olet jo suorittanut paikallisen tai cloud käyttöönoton etsiminen oman pääkansio-*. user.config luodulla käyttöönoton aikana. Avaa tätä tiedostoa tekstieditorissa. 

5. Muuta seuraava rivi sisältämään kopioitu Oman QueryKey arvo: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Voinko luoda esimääritettyjä ratkaisu Jos käytössä on Microsoft Azure DreamSpark varten?
Tällä hetkellä et voi luoda esimääritettyjä ratkaisun [Microsoft Azure varten DreamSpark] [ lnk-dreamspark] tili. Sijaan voit luoda [maksuttoman kokeiluversion tilin Azure] [ lnk-30daytrial] vain muutaman minuutin, jonka avulla voit luoda esimääritettyjä ratkaisu.

### <a name="how-do-i-delete-an-aad-tenant"></a>Miten voin poistaa AAD-alihallintaan?

Blogimerkinnässä Eric Golpe [vaiheista poistaminen Azure AD-vuokraajan][lnk-delete-aad-tennant].

## <a name="next-steps"></a>Seuraavat vaiheet

Voit myös selata joitakin muita ominaisuuksia ja toimintoja IoT Suite valmiiksi ratkaisuja:

- [Ennakoivan ylläpito valmiiksi ratkaisun yleiskatsaus][lnk-predictive-overview]
- [IoT suojaus on alusta alkaen][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx

<properties 
    pageTitle="Vianmääritys BizTalk-palveluita käyttämällä toimintojen lokit | Microsoft Azure" 
    description="Vianmääritys BizTalk-palveluita käyttämällä toimintojen lokit. MAB-WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Vianmääritystä toimintojen lokit

## <a name="what-are-the-operation-logs"></a>Mitkä ovat toimintojen lokit
Toimintojen lokit on käytettävissä Azure perinteinen-portaalissa, jonka avulla voit tarkastella historiallisten lokit-toimintoa suoritetaan Azure palvelujen, mukaan lukien BizTalk palvelut hallintapalvelut ominaisuus. Voit tarkastella organisaatiokaavioita 180 päivää tilauksen BizTalk Service management toimintoihin liittyvät historiatiedoissa.

> [AZURE.NOTE]Tämä ominaisuus sieppaa vain hallintatoiminnot BizTalk Services, jolloin palvelu on käynnistetty, kuten lokit palautettua, ja niin edelleen. Toimintoja seurataan riippumatta siitä, onko ne suoritetaan perinteinen Azure-portaalista tai käyttämällä [BizTalk palvelun REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx). Katso täydellinen luettelo toiminnoista, joita seurataan hallintapalvelut [Toimintoja seurataan käyttämällä Azure Management Services](#bizops).<br/><br/>
Tämä ei siepata lokien liittyvien BizTalk palvelun suorituksenaikainen prosessi (kuten viestin käsittelemien siltoja jne.). Voit tarkastella lokitiedostot avulla BizTalk palvelut-portaalista seuranta-näkymässä. Lisätietoja on artikkelissa [Viestien seuranta](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>Näytä BizTalk toiminnon palvelulokit
1. Azure perinteinen-portaalissa Valitse **Management Services**ja valitse sitten **Toimintojen lokit** -välilehti.
2. Voit suodattaa lokit parametrien, kuten tilauksen, päivämääräalue, palvelutyyppi (kuten BizTalk palvelut), palvelunimi tai toiminnon (onnistui, epäonnistunut) tilan perusteella.
3. Valitse suodatetun luettelon tarkasteleminen valintamerkki. Seuraava kuva esittää liittyvien testbiztalkservice:  ![tarkastella toimintojen lokit][ViewLogs] 
4. Jos haluat nähdä lisää tietoja tietyn toiminnon, valitse rivi ja valitse **tiedot** alaosan tehtäväpalkissa.


## <a name="bizops"></a>Toimintoja seurataan Azure Management Services-palvelujen avulla
Seuraavassa taulukossa on luettelo toiminnoista, joita seurataan Azure Management Services-palvelujen avulla:

Toiminnon nimi | Tehtävä
--- | ---
CreateBizTalkService | Toiminto luo uuden BizTalk-palvelu
DeleteBizTalkService | Toiminnon BizTalk-palvelun poistaminen
RestartBizTalkService | Toiminnon BizTalk-palvelun käynnistäminen uudelleen
StartBizTalkService | Toiminnon BizTalk-palvelun käynnistäminen
StopBizTalkService | Toiminnon BizTalk-palvelun pysäyttäminen
DisableBizTalkService | Toiminnon BizTalk palvelun käytöstä
EnableBizTalkService | Toiminnon käyttöön BizTalk-palvelu
BackupBizTalkService | Toiminnon tiedostojen BizTalk-palvelun määrittäminen
RestoreBizTalkService | Palauttaa määritetyn varmuuskopioinnin BizTalk-palvelun toiminto
SuspendBizTalkService | Toiminnon BizTalk-palvelun käytön estämiseksi
ResumeBizTalkService | Toiminto, kun haluat jatkaa BizTalk-palvelu
ScaleBizTalkService | Toiminnon skaalata BizTalk palvelu, ylös tai alas
ConfigUpdateBizTalkService | Päivitystoiminto BizTalk-palvelun määrittäminen
ServiceUpdateBizTalkService | Toiminto päivittää tai alentaa toisen version BizTalk palvelu
PurgeBackupBizTalkService | Toiminto tyhjentää varmuuskopioiden BizTalk palvelun ulkopuolella säilytysaika


## <a name="see-also"></a>Katso myös
- [Voit varmuuskopioida BizTalk-palvelu](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalk palvelun palauttaminen varmuuskopiosta](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk Services: Kehittäjä, Basic, Vakio ja Premium versiot kaavio](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk Services: Valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk Services: Tila-kaavion valmistelu](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk Services: rajoittaminen](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 

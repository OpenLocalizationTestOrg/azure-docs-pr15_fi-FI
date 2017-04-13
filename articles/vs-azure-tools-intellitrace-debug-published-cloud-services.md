<properties 
   pageTitle="Virheenkorjaus julkaistun pilvipalvelussa IntelliTrace ja Visual Studio | Microsoft Azure"
   description="Virheenkorjaus julkaistun pilvipalvelussa IntelliTrace ja Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Virheenkorjaus julkaistua pilvipalvelussa IntelliTrace ja Visual Studio

##<a name="overview"></a>Yleiskatsaus

IntelliTrace, jossa voit kirjautua laaja muistin sisältö roolin esiintymän suorittaessaan Azure-tietokannassa. Jos haluat selvittää ongelman syy, voit käydä läpi koodin Visual Studio, ikään kuin se oli käynnissä Azure IntelliTrace lokit. IntelliTrace tietueet, avain koodin suorittamisen ja ympäristön tiedot, kun Azure sovellus on käynnissä pilvipalvelussa Azure-tietokannassa ja voit toista Visual Studio tallennetut tiedot. Vaihtoehtoisesti voit remote virheenkorjaus pilvipalvelussa, jossa on käytössä Azure liittäminen. Katso [Virheenkorjaus pilvipalveluihin](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace on tarkoitettu vain virheenkorjaus skenaariot ja ei voi käyttää tuotannon käyttöönottoa varten.

>[AZURE.NOTE] Voit käyttää IntelliTrace, jos sinulla on asennettu Visual Studio-yrityksen ja Azure sovelluksen-kohteiden .NET Framework 4 tai uudempi versio. IntelliTrace kerää tietoja Azure-roolien. Suorita aina 64-bittisten käyttöjärjestelmien rooli näennäiskoneiden.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Azure-sovelluksen määrittäminen IntelliTrace

IntelliTrace Azure-sovelluksen käyttöön voit luoda ja julkaista projektin Visual Studio Azure sovelluksen. Sinun on määritettävä IntelliTrace Azure-sovelluksen, ennen kuin voit julkaista Azure. Jos julkaista sovelluksesi määrittämättä IntelliTrace, mutta sen jälkeen päätät, että haluat tehdä, sinun on julkaista Visual Studio sovellus uudelleen. Lisätietoja on artikkelissa [Azure-työkaluilla pilvipalveluun julkaiseminen](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Kun olet valmis ottamaan Azure sovelluksen, varmista, että projektin muodosta-kohteet on määritetty **Virheenkorjaus**.

1. Azure projektin pikavalikon avaaminen ratkaisunhallinnassa ja sitten **Julkaise**.
 
    Ohjattu julkaiseminen Azure-sovellus tulee näkyviin.

1. Kerätä IntelliTrace lokit sovelluksen, kun se on julkaistu pilveen, valitse **Ota käyttöön IntelliTrace** -valintaruutu.

    >[AZURE.NOTE] Voit ottaa IntelliTrace tai profilointi Azure sovelluksen julkaisukerralla. Et voi ottaa käyttöön molemmat.

1. Jos haluat mukauttaa IntelliTrace peruskokoonpano, valitsemalla **asetukset** hyperlinkki.

    IntelliTrace asetukset-valintaikkunassa näkyy seuraavassa kuvassa esitetyllä tavalla. Voit määrittää lokiin, haluatko puhelun tietojen kerääminen, mitkä moduulit ja prosessit voivat kerätä kirjautuu ja olevan levytilan määrän varattavan tallenteen tapahtumat. Saat lisätietoja IntelliTrace [Virheenkorjaus IntelliTrace kanssa](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

IntelliTrace loki on kehäviittaus lokitiedostoon (oletuskoko on 250 Mt) IntelliTrace asetuksissa määritettyä enimmäiskoosta. IntelliTrace lokit kerätään virtuaalikoneen tiedostojärjestelmän tiedostoon. Kun pyydät lokit, tilannevedoksen otetaan senhetkinen ajoissa ja ladata paikalliseen tietokoneeseen.

Kun Azure sovellus on julkaistu Azure, voit määrittää Jos IntelliTrace on otettu palvelimen Resurssienhallinnassa Azure Laske-solmu seuraavassa kuvassa esitetyllä tavalla:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>Lataaminen roolin esiintymän IntelliTrace lokit

Voit ladata IntelliTrace lokit roolin esiintymän **Palvelimen**Explorerissa **Cloud Services** -solmu. Laajenna **Cloud Services** -solmu, kunnes löydät kiinnostavan esiintymän, Avaa tämä esiintymä pikavalikon ja valitse **Näytä IntelliTrace lokitiedot**. IntelliTrace lokit ladataan paikalliseen tietokoneeseen tallennetun tiedoston. Aina, kun pyydät IntelliTrace kirjaa, ohjelma luo uuden tilannevedoksen.

Kun lokit on ladattu, Visual Studio näkyy toiminnon edistyminen Azure tehtävän loki-ikkunaan. Seuraavassa kuvassa esitetyllä voit laajentaa rivin kohteen toiminnon yksityiskohdat.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Voit jatkaa Visual Studiossa työskentelyä IntelliTrace lokit ladataan. Kun lokiin on ladannut, se avautuu automaattisesti Visual Studio.

>[AZURE.NOTE] IntelliTrace lokit saattaa olla poikkeuksia, jotka puitteissa Luo ja käsittelee myöhemmin. Sisäinen framework koodi luo poikkeukset käynnistäminen rooli, jotta niitä turvallisesti ohittaa Normaali osana.

## <a name="see-also"></a>Katso myös

[Virheenkorjaus pilvipalveluihin](https://msdn.microsoft.com/library/ee405479.aspx)


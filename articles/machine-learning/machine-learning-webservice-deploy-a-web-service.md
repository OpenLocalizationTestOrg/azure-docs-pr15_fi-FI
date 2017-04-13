<properties
   pageTitle="Uusi Web-palvelu käyttöönotto"
   description="Työnkulun käyttöönottamisesta ARM-pohjainen web-palvelu"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Uusi web-palvelu käyttöönotto

Microsoft Azure Konepohjaisten oppimistekniikoiden nyt on verkkopalvelut, jotka perustuvat [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) salliminen uusi palvelupakettivaihtoehdot laskutus-ja käyttöönotto WWW-palvelun alueille.

Ottaa käyttöön Microsoft Azure koneen Learning Internet-palveluita käyttämällä verkkopalvelun Yleiset työnkulku on:

* Ennakoivan kokeen luominen
* ottaa raporttinäkymän käyttöön
* nimen määrittäminen
* laskutuksen suunnitelma
* Testaa se
* käyttää sen.

Seuraavassa kuvassa on kuvattu työnkulun.

![Web-palvelun käyttöönoton työnkulku][1]
 
## <a name="deploy-web-service-from-studio"></a>WWW-palvelun Studio käyttöönotto 

Ottamaan kokeen uusi web-palvelu. Kirjaudu sisään tietokoneeseen Learning Studio ja luo uusi ennakoivan verkkopalvelun. 

**Huomautus**: Jos olet jo asentanut kokeessa perinteinen web-palvelu ei voi ottaa sitä uusi web-palvelu.
 
Valitse kokeen alusta alareunassa **Suorita** ja valitse sitten **Käyttöön Web-palvelu** ja **Ottaa käyttöön Web-palvelu [uusi]**. Koneen Learning verkkopalvelun hallinnan käyttöönotto-sivu avautuu.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Koneen Learning Web-palvelun hallinta käyttöön koe-sivu
Anna käyttöönotto koe-sivulla WWW-palvelun nimi.
Valitse hinnoittelu suunnitelma. Jos sinulla on aiemmin hinnoittelu palvelupaketti, voit valita, muuten sinun on luotava uusi hinta-suunnitelma-palvelun. 

1.  **Hinta-suunnitelma** avattava, valitse aiemmin luotu suunnitelma tai **Valitse uusi palvelupaketti** -vaihtoehto.
2.  Kirjoita nimi, joka tunnistaa suunnitelman laskun **Tilauksen nimen**.
3.  Valitse **Kuukausittain suunnitteleminen tasoa**. Huomaa, että suunnitelma tasoa oletusarvon oletusarvo-alue- ja web-palveluun käyttämään otetaan käyttöön alueen.

Valitse **Ota käyttöön** tai for web-palvelu avautuu pikaopas-sivu.

## <a name="quickstart-page"></a>Pikaopas-sivu
Palvelun pikaopas verkkosivun antaa access ja ohjeita yleisimpiä tehtäviä, kun olet luonut uuden verkkopalvelun suorittaa. Täällä voit helposti käyttää **testisivu** ja **Kulutettava** sivun.

## <a name="testing-your-web-service"></a>WWW-palvelun testaaminen

Pikaopas-sivulla Yleiset toiminnot-kohdasta testi WWW-palvelun.   

Voit testata WWW-palvelun nimellä vastaus palvelun (RRS):

* Valitse valikkoriviltä valitsemalla **Testaa** .
* Valitse **Pyynnön vastaus**.
* Kirjoita haluamasi arvot syötteen sarakkeille että kokeen.
* Valitse Testaa **Vastaus**.

Voit tulokset tulevat näkyviin sivun oikeassa reunassa.

Voit esikatsella erä suorittamisen Service (BES) verkkopalvelun-käytetään CSV-tiedostoon:

* Valitse valikkoriviltä valitsemalla **Testaa** .
* Valitse **erä**.
* Valitse syötettäsi valitse Selaa ja Selaa otoksen datatiedostossa.
* Valitse **Testaa**.

Testauksessa tila näkyy kohdassa **Testata erätyöt**.

## <a name="consuming-your-web-service"></a>Muissa Web-palvelu

Kun käyttöön web-palvelu, Azure koneen Learning kokeissa on REST-Ohjelmointirajapinta, joka on käytetty monenlaisia laitteita ja ympäristöjen mukaan. Tämä johtuu siitä yksinkertaisia REST API hyväksyy ja vastaa kanssa JSON-muotoiset viestit. Azure koneen Learning portal on tunnus, jolla voidaan kutsua R, C# ja Python web-palveluun.
 
Kulutus-sivulla voit etsiä:

* Ohjelmointirajapinnan avain ja URI on käyttäjäprofiilipalvelun verkkopalvelu-sovelluksissa.
* Excel- ja web app-malleissa käyttö Aloita kulutus prosessin.
* Esimerkki koodi C#, python ja R aloittamista.

Lisätietoja muissa verkkopalvelut Katso, [miten tarjoaman web Azure koneen Learning-palvelu, joka on otettu käyttöön Konepohjaisten Oppimistekniikoiden kokeen](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja muissa verkkopalvelut-kohdassa:

[Miten tarjoaman Azure koneen Learning-web-palvelu, joka on otettu käyttöön Konepohjaisten Oppimistekniikoiden kokeen](machine-learning-consume-web-services.md)

[Azure koneen Learning Verkkopalvelut: Käyttöönotto- ja kulutus](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->

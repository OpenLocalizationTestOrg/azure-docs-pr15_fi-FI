<properties
    pageTitle="Automaattinen skaalaus, jossa virtuaalikoneen asteikko tietojoukkoa vianmääritys | Microsoft Azure"
    description="Vianmääritys Automaattinen skaalaus virtuaalikoneen asteikko joukkojen kanssa. Lisätietoja tyypillinen ongelmia sekä niiden ratkaisemiseksi."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Automaattinen skaalaus virtuaalikoneen asteikko joukot ja vianmääritys

**Ongelma** : olet luonut automaattisen skaalauksen poistaminen infrastruktuuri Azure resurssien hallinta käyttämällä AM asteikko joukot – esimerkiksi ottamalla tältä mallia: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – on määritetty mittakaava sääntöjen ja se toimii erinomaisesti, paitsi että riippumatta siitä, kuinka paljon VMs laitetaan kuormituksen se ei Automaattinen skaalaus.

## <a name="troubleshooting-steps"></a>Vianmääritysohjeita

Huomioitavia seikkoja ovat seuraavat:

- Kuinka monta sydämiä kunkin AM sisällä, ja ladataan jokaisen core?
 Esimerkki Azure pikaopas mallissa yläpuolella on do_work.php komentosarjan, joka lataa yksittäinen core. Jos käytät AM, joka on suurempi kuin yhden core AM koon, kuten Standard_A1 tai D1 täytyy suorittaa tämän Lataa useita kertoja. Tarkista, kuinka monta cores oman VMs tarkastelemalla [näennäiskoneiden koot Windows Azure-tietokannassa](../virtual-machines/virtual-machines-windows-sizes.md)

- Kuinka monta VMs AM-asteikko-joukko olet tekemässä kullekin työn?

    Asteikolla tapahtuman ulos vain tapahtuu, kun keskiarvon suorittimen yli **kaikki** VMs asteikko joukon ylittää raja-arvo määritetty Automaattinen skaalaus-säännöt sisäinen ajan mittaan.

- Kaipaat asteikko tapahtumat?

    Tarkista tarkastuksen kirjautuu asteikko tapahtumien Azure-portaalissa. Ehkäpä oli asteikolla ylöspäin ja asteikolla alaspäin, jossa on vastaamattomat. Voit suodattaa mukaan "Asteikko"...

    ![Valvontalokien][audit]

- Ovat asteikko ja skaalaus-kohtaa kynnysarvot riittävän eri?

    Oletetaan, että sääntöjoukon skaalata kun suorittimen keskiarvo on suurempi kuin 50 % 5 minuuttia ja käytetään, kun suorittimen keskiarvo on pienempi kuin 50 %. Tämä aiheuttaa "flapping" ongelma, kun suorittimen käyttö on lähellä tämä kynnysarvo asteikko toimintojen jatkuvasti suurentaminen ja pienentäminen olevien kokoa. Tästä syystä Automaattinen skaalaus-palvelun yrittää estää "flapping", joka voi luettelon kuin ei skaalaus. Varmista vuoksi asteikko-kohtaa ja mittakaava kynnysarvot ovat riittävän eri sallimaan välissä skaalaus tilaa.

- , Kirjoittaa JSON-mallin?

    On helppo tapahtuvista, niin aloittaminen mallista, esimerkiksi jokin edellä, jotka on osoitettu voit työskennellä ja pieni vaiheittainen muutoksia. 

- Voit manuaalisesti skaalata sisään tai ulos?

    Kokeile mallirakenteeseen AM asteikko määrittää resurssin "kapasiteettia"-asetus asetukseen VMs määrän muuttaminen manuaalisesti. Esimerkki-malli voit tehdä tämän ovat täällä: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – joudut ehkä muokata mallia, varmista, että se on tietokoneen yhtä suuri kuin asteikko määrittäminen on käytössä. Jos VMs määrän muuttaminen onnistuneesti manuaalisesti, valitse tiedät ongelma eristetään Automaattinen skaalaus.

- Tarkista, että Microsoft.Compute/virtualMachineScaleSet ja [Azure resurssin Explorer](https://resources.azure.com/) Microsoft.Insights resurssit

    Tämä on välttämätöntä vianmääritystyökalu, joka näyttää Azure Resurssienhallinta resurssien tilan. Valitse tilauksen ja katsomalla vianmäärityksen resurssiryhmä. Valitse Laske resurssin tarjoajaan katsomalla luomasi AM-asteikko-määrittäminen ja tarkista esiintymä-näkymä, jossa näkyvät käyttöönoton tilan. Tarkista myös VMs AM-asteikko-joukko esiintymä-näkymä. Valitse salliva asetus Microsoft.Insights resurssin tarjoajaan ja tarkista olet tyytyväinen Automaattinen skaalaus-sääntöjä.

- Diagnostiikan tunniste toimii ja lähettää suorituskykytietoja?

    __Päivitys:__ Azure Automaattinen skaalaus on parannettu käyttämään host mukaan arvot putkijohto joka edellyttää enää diagnostiikka-laajennus on asennettu. Tämä tarkoittaa joitakin tekstikappaleita enää käyttää Jos luot uuden myyntijakso automaattisen skaalauksen poistaminen sovellus Seuraava. Esimerkkejä Azure mallit, joka on muunnettu käyttämään host putkijohto: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Käytön mukaan host arvot Automaattinen skaalaus paremmalta seuraavista syistä:

    - Vähemmän kuin diagnostiikka ei ole laajennuksia liikkuvat osat on oltava asennettuna.
    - Yksinkertainen malleja. Voit lisätä tietoja Automaattinen skaalaus säännöt asteikko määrittäminen mallia.
    - Luotettava raportointi- ja uusi VMs nopeampaa avaamista.

    Voit edelleen käyttää diagnostiikan tunniste on vain syistä olisi tarvitset muistin diagnostiikka raportointi ja skaalausta. Host (isäntä) perusteella arvot ei raportin muistiin.

    Liittyvän mielessä vain noudata tämän artikkelin Jos käytät edelleen diagnostiikan tunnisteet oman automaattisen skaalauksen poistaminen.

    Automaattinen skaalaus Azure Resurssienhallinta voi käyttää (mutta ei enää pitää) avulla AM tunniste nimeltä diagnostiikka-tunniste. Sen tietokoneesta kuuluu äänimerkki suorituskykytietoja tallennustilan tilin Määritä mallissa. Nämä tiedot yhdistetään sitten näytön Azure-palvelu.

    Jos tiedot-palvelun tietoja ei voi lukea VMs, se pitäisi lähettää sinulle sähköpostiviestin – esimerkiksi, VMs on alaspäin, joten Tarkista sähköpostiin (sitä kirjautumiskerralla luominen Azure-tili).

    Voit siirtyä ja tarkastella tietoja itse. Tarkista Azure-tallennustilan tilin cloud Resurssienhallinnan avulla. Esimerkki [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2)Kirjaudu sisään ja valitse Azure tilauksen ovat käytössä ja diagnostiikka-tallennustilan tilin nimi Viitattu diagnostiikka tunniste määrityksen käyttöönottoa malliin...

    ![Cloud Explorer][explorer]

    Tässä näet useita taulukoita, joissa kukin AM tiedot tallennetaan. Enintään viimeisimmät rivien Etsi ottaen Linux ja esimerkkinä suorittimen-arvo. Visual Studio cloud explorer tukee kyselykieli, joten voit suorittaa kyselyä, kuten "aikaleima gt datetime'2016-02-02T21:20:00Z" ", varmista, että saat uusimmat tapahtumat (oletetaan aika on UTC-ajan). Onko tietojen näkyvissä olevat siellä vastaavat asteikko-säännöt määritetään? Alla olevassa esimerkissä koneen 20 Suoritin aloittaminen lisääntyvien 100 prosenttiin päälle viimeisen 5 minuuttia...

    ![Tallennustilan taulukot][tables]

    Jos tietojen ei ole sitten se merkitsee ongelma on käynnissä VMs diagnostiikan tunniste. Jos tiedot ovat käytettävissä, se tarkoittaa joko liittyvistä ongelmista, asteikko-sääntöjä tai tiedot-palvelun kanssa. Tarkista [Azure tila](https://azure.microsoft.com/status/).

    Kun olet toiminut näiden vaiheiden, jos sinulla on edelleen ongelmia Automaattinen skaalaus voitu Kokeile keskustelupalstoja [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)tai [pinota ylivuoto](http://stackoverflow.com/questions/tagged/azure)tai kirjaudut puhelimitse. Valmistaudu mallin ja resurssitiedot näkymän jakaminen.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png

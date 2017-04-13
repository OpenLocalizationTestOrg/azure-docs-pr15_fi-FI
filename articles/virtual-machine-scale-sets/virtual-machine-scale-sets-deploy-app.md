<properties
    pageTitle="Ota käyttöön virtuaalikoneen asteikko joukoissa-sovellus | Microsoft Azure"
    description="Ota käyttöön virtuaalikoneen asteikko joukoissa-sovellus"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Ota käyttöön virtuaalikoneen asteikko joukoissa-sovellus

Käytössä AM-asteikko-Määritä sovellus otetaan käyttöön tavallisesti jollakin seuraavista kolmesta tavasta:

- Uuden ohjelmiston asentaminen ympäristö kuva käyttöönoton aikana. Ympäristön-kuva tässä yhteydessä on käyttöjärjestelmän kuvan Azure Marketplacesta, kuten Ubuntu 16.04, Windows Server 2012 R2 jne.

Voit asentaa ohjelmiston [AM tunniste](../virtual-machines/virtual-machines-windows-extensions-features.md)avulla ympäristö-kuva. AM-tunniste on ohjelmisto, joka suoritetaan, kun AM otetaan käyttöön. Voit suorittaa milloin käyttöönoton avulla mukautettu komentosarja-tunniste, kuten koodia. [Tässä](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) on esimerkki Azure Resurssienhallinta malli on kaksi AM: Linux mukautettu komentosarja tunniste Apache ja PHP asentamiseen ja tulostaminen suorituskykytietoja Azure Automaattinen skaalaus käyttää diagnostiikan tunniste.

Tämän menetelmän etuna on se on taso erottaminen sovelluksen-koodin ja käyttöjärjestelmän välillä, ja voit ylläpitää sovelluksesi erikseen. Kurssin tarkoittaa siellä on myös, Lisää liikkuvat osat ja AM käytön aikana saattaa ei saa olla silloin ei komentosarjan ladataan ja määritetään usein.

**Jos mukautettu komentosarja-tunniste-komennon (esimerkiksi salasanan) välittää luottamuksellisia tietoja, muista määrittää `commandToExecute` - `protectedSettings` sen sijaan, että mukautettu komentosarja-tunniste-määrite `settings` määrite.**

- Luo mukautettu AM-kuva, joka sisältää yksittäisen Näennäiskiintolevyn käyttöjärjestelmän ja sovelluksen. Tähän asteikko-joukko koostuu VMs luomiasi, jos haluat säilyttää joiden kuvan kopioitu joukko. Tämän menetelmän edellyttää AM käyttöönoton milloin ylimääräistä määrityksiä. Kuitenkin `2016-03-30` versio AM asteikko joukot (ja aiemmissa versioissa) asteikon määrittäminen VMs OS levyjen kokorajoituksena yksittäisen tallennustilan tilin. Voit siis 40 VMs enimmäismäärä on asteikko joukon, eikä skaalausta 100 AM määrittäminen raja ympäristö kuvia. Artikkelissa [Asteikko määrittäminen rakenne yleiskatsaus](./virtual-machine-scale-sets-design-overview.md) lisätietoja.

- Käyttöön ympäristö tai mukautetun kuvan eli lähinnä säilö host ja asentaa sovelluksen vähintään yksi säilöjen, voit hallita orchestrator tai määritysten hallinta-työkalulla. Tämän menetelmän tietoja hyvä vaihtoehto on, että olet otetaan pilvi-infrastruktuuria sovelluskerroksen- ja ylläpitää niiden erikseen.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Mitä tapahtuu, kun AM asteikko määrittäminen Skaalaa ulos?

Kun lisäät yhden tai useamman VMs asteikolla suurentamalla kapasiteetin määrittäminen – manuaalisesti tai kautta Automaattinen skaalaus – sovellus asennetaan automaattisesti. Esimerkiksi jos asteikon on määritetty laajennukset, ne suoritetaan uusi AM aina, kun se on luotu. Jos asteikko-määrittäminen perustuu mukautetun kuvan, minkä tahansa uusi AM on mukautettu lähdekuvan kopio. Jos skaalauksen VMs ovat säilö isännät sitten säilöt mukautettu komentosarja-laajennuksen lataaminen käynnistyskoodi on ehkä tai tiedostotunnistetta voi asentaa agentti, Rekisteröi klusterin-orchestrator (kuten Azure säilö palvelu).

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Miten voit hallita sovellusten päivitykset AM asteikko joukoissa?

AM asteikko joukoissa päivitysten sovelluksen kolme tärkeimmät tavoista noudattamalla kolme sovelluksen edellisen käyttöönotto-vaihtoehtoja:

* Päivitetään AM tunnisteet. Mikä tahansa AM laajennukset, jotka on määritetty AM-asteikko-määrittäminen suoritetaan aina, kun uusi AM on otettu käyttöön, aiemmin AM reimaged tai AM tunniste on päivitetty. Jos haluat päivittää sovelluksen suoraan päivittäminen sovelluksen kautta laajennukset on elinkelpoinen lähestymistapa – yksinkertaisesti päivittää tunniste-määritys. Yksi helppo tapa tehdä on muuttamalla siten, että uuden ohjelmiston fileUris.

* Pysyvä mukautettu kuva-vaihtoehto. Kun tuominen AM kuva Ruokamyyjäiset sovelluksen (tai sovelluksen osia) voit keskittyä rakentaminen jatkuvasti automatisoida muodosta, Testaa ja kuvien käyttöönottoa. Voit suunnitella oman arkkitehtuuri helpottamiseksi vaiheistettu asteikko-joukon kyselyjä tuotannon nopea vaihtaminen. Tämän menetelmän hyvä esimerkki on [Azure Spinnaker ohjain toimi](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Pakkaaja ja Terraform tukea myös Azure Resurssienhallinta, jotta voit määrittää kuvien "-koodi," ja muodostamisesta Azure-tietokannassa, valitse Käytä Näennäiskiintolevyn asteikko määrittäminen. Kuitenkin harjoittelu niin tulee ongelmia Marketplace-kuvat-kohtaa, johon tunnisteet ja mukautettu-komentosarjoja muuttuvat tärkeämpää, koska bittien Marketplacesta ei käsitellä suoraan.

* Päivitä säilöjen. Abstrakti sovelluksen elinkaaren hallinta suuremmaksi pilvi-infrastruktuuria esimerkiksi encapsulating sovellukset ja sovelluksen osia säilöjen kyselyjä ja hallita näiden säilöjen säilö orchestrators ja määritysten valvojat kuten kokki/Puppet kautta.

Asteikon määrittää VMs muuttuvat vakaata pohjalevyn säilöjen ja vaatia ainoastaan ajoittaiset suojaus ja OS liittyvät päivitykset. Kuten edellä, Azure säilö-palvelu on hyvä esimerkki sen ympärille palvelu kehittämistä sekä ottaen tämän menetelmän.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Kuinka palautat OS päivityksen, Päivitä toimialueilla?

Oletetaan, että haluat päivittää OS kuva ja säilyttää AM asteikko määrittäminen suorittaminen. Yksi tapa tehdä on päivitettävä AM kuvien yhden AM kerrallaan. Voit tehdä PowerShell tai Azure CLI. On erillinen komennot päivittää AM asteikko määrittäminen-malli (miten sen asetukset määritetään) ja antaa yksittäisiä VMs puhelujen "manuaalisen päivityksen".

[Tässä](https://github.com/gbowerman/vmsstools) on esimerkki Python komentosarjan, joka automatisoi päivitys AM asteikko määrittää yhden päivityksen toimialueen kerrallaan. (Caveat: Lisää kuitti käsite kuin vahvistettu tuotannon käyttövalmiin ratkaisu on – voit lisätä joitakin virhe tarkistuksen jne.).

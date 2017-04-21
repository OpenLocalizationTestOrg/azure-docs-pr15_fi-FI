

Tässä artikkelissa kerrotaan ottamisesta käyttöön Azure virtuaalikoneen asteikko määrittää käyttämällä Visual Studio resurssin ryhmän käyttöönotto.


[Azure virtuaalikoneen asteikko joukot](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) ovat Azure Laske resurssin käyttöön hallinnassa samalla näennäiskoneiden helposti integroitu Automaattinen skaalaus-vaihtoehtojen avulla ja kuormituksen tasaus. Voit valmistella ja ota käyttöön AM asteikko joukot [Azure resurssien hallinta (ARM) mallien](https://github.com/Azure/azure-quickstart-templates)avulla. ARM-malleja voidaan ottaa käyttöön käyttämällä Azure CLI PowerShellin muiden ja myös suoraan Visual Studio. Visual Studio on esimerkki mallit, jotka voidaan ottaa käyttöön Azure resurssin ryhmän käyttöönoton projektin osana.

Azure resurssiryhmä ominaisuuksissa on tapa ryhmitellä yhteen ja julkaista liittyvät Azure resurssit joukko yhden käyttöönotto-toimintoa. Voit lukea lisää ne tähän: [luominen ja käyttöönotto Azure resurssiryhmät Visual Studio kautta](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/).

## <a name="pre-requisites"></a>Vaatimukset

Aloita käyttöönotto Visual Studiossa AM asteikon mukaan edellyttää seuraavia:

- Visual Studio 2013: n tai 2015
- Azure SDK 2.7 tai 2,8

Huomautus: Nämä ohjeet oletetaan käytät Visual Studio 2015 [Azure SDK 2,8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)kanssa.

## <a name="creating-a-project"></a>Projektin luominen

1. Uuden projektin luominen Visual Studio 2015 valitsemalla **tiedoston | Uusi | Projektin**.

    ![Tiedosto Uusi][file_new]

2. Valitse **Visual C# | Cloud**, valitse projektin käyttöönoton ARM-mallin luominen **Azure Resurssienhallinta** .

    ![Projektin luominen][create_project]

3.  Valitse Mallit-luettelosta tai Linux Windows virtuaalikoneen asteikko joukon mallin.

    ![Valitse malli][select_Template]

4. Kun projekti on luotu näet käyttöönoton PowerShell-komentosarjojen ja Azure-Resurssienhallinta mallin parametrin tiedoston virtuaalikoneen asteikko määrittäminen.

    ![Ratkaisunhallinnassa][solution_explorer]

## <a name="customize-your-project"></a>Projektin mukauttaminen

Voit nyt muokata malleja mukautetaan sovelluksen tarpeita, kuten AM tunniste ominaisuuksien lisääminen tai muokkaaminen kuormituksen säännöt. Oletusarvoisesti AM asteikko joukon mallit on määritetty ottamaan AzureDiagnostics-tunniste, joka helpottaa Automaattinen skaalaus sääntöjen lisääminen. Se myös ottaa käyttöön kuormituksen julkisen IP-osoitetta, määritetty saapuvan NAT sääntöjä, joiden ansiosta voit muodostaa yhteyden AM esiintymät SSH (Linux) tai RDP (Windows) – edusta-porttialue alkaa 50000, toisin sanoen kyseessä Linux, jos olet SSH porttiin 50000 julkiseen IP-osoite (tai toimialuenimi) voit reititetään ensimmäisen AM asteikko joukko 22-porttiin. Yhteyden muodostaminen portin 50001 reititetään toisen AM portti 22 ja niin edelleen.

 Muokkaa malleja Visual Studiossa erinomainen tapa on JSON jäsennyksen avulla voit järjestää parametrit, muuttujat ja resursseja. Tietoja rakenteen Visual Studio voit osoita mallin virheet ennen niiden käyttöönottoa.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>Ota käyttöön-projekti

6. ARM-mallin käyttöön Azure AM Skaalaa määrittäminen resurssien luomiseen. Napsauta hiiren kakkospainikkeella project-solmu ja valitse Ota käyttöön **| Uudet käyttöönoton**.

    ![Mallin ottaminen käyttöön][5deploy_Template]

7. Valitse tilauksen "Käyttöön, resurssiryhmä"-valintaikkunassa.

    ![Mallin ottaminen käyttöön][6deploy_Template]

8. Täällä voit myös luoda uuden Azure resurssiryhmä ottaa mallin käyttöön.

    ![Uusi resurssiryhmä][new_resource]

9. Valitse **Muokkaa parametrit** -painikkeen kirjoittaa parametreja, jotka siirretään malliin tiettyjä arvoja, kuten käyttäjänimi ja käyttöönoton luomiseen tarvitaan salasana käyttöjärjestelmän.

    ![Muokkaa parametrit][edit_parameters]

10. Valitse **Ota käyttöön**nyt. **Kohde** -ikkunassa näkyy käyttöönoton edistymisen. Huomaa, että-toimintoa suoritetaan **Käyttöönotto AzureResourceGroup.ps1** komentosarja.

    ![Kohde-ikkunassa][output_window]

## <a name="exploring-your-vm-scale-set"></a>Tutustuminen AM asteikko-määrittäminen

Kun asennus on valmis, voit tarkastella uusi AM-asteikko-joukko Visual Studio **Cloud Explorer** (Päivitä luettelo). Cloud Explorer voit hallinnoida Azure resurssien Visual Studiossa kehityssovellusten aikana. Voit tarkastella AM-asteikko-määrittäminen Azure-portaalin ja Azure resurssin Explorerissa.

![Cloud Explorer][cloud_explorer]

 Portaalissa on paras tapa visuaalisesti hallinta Azure infrastruktuurin selaimen, Azure resurssin Explorer sisältää explorer helposti ja virheenkorjaus Azure resursseja, anna ikkunan "esiintymän näkymään" ja näyttää myös tarkasteltavan resurssien PowerShell-komennoilla. AM asteikko joukot ollessa esikatselussa resurssin Explorer näkyy useimmissa tiedot AM asteikko-joukkojen.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet otettu AM asteikko määrittää Visual Studio kautta voit mukauttaa projektin sovelluksen tarpeitasi. Esimerkiksi määrittäminen Automaattinen skaalaus lisäämällä tietoja resurssin, infrastruktuuri lisääminen lomakemalliin, kuten erillinen VMs tai käyttöönotto sovellusten mukautettu komentosarja-tunniste. Esimerkki mallien hyvä lähteen löytyvät [Azure pikaopas malleja](https://github.com/Azure/azure-quickstart-templates) GitHub tietovarasto (Etsi "vmss").

[file_new]: ./media/virtual-machines-common-scale-sets-visual-studio/1-FileNew.png
[create_project]: ./media/virtual-machines-common-scale-sets-visual-studio/2-CreateProject.png
[select_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-common-scale-sets-visual-studio/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-common-scale-sets-visual-studio/8-EditParameter.png
[output_window]: ./media/virtual-machines-common-scale-sets-visual-studio/9-Output.png
[cloud_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/12-CloudExplorer.png

<properties
    pageTitle="Resurssien kanssa Azure-CLI | Microsoft Azure"
    description="Azure-käyttöliittymä (CLI) avulla voit hallita Azure resurssit ja ryhmät"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Azure-CLI avulla voit hallita Azure resurssit ja resurssiryhmät


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-rest-api.md)


Azure komentorivivalitsimet Interface (Azure CLI) on useita työkaluja, voit ottaa käyttöön ja hallita resurssien hallinnan resurssit. Tässä artikkelissa esitellään yleisiä tapoja hallita Azure resurssit ja resurssiryhmät käyttämällä Azure-CLI Resurssienhallinta-tilassa. Tietoja käyttöönotto resurssit CLI avulla on artikkelissa [Resurssienhallinta mallit ja Azure CLI käyttöönotto resursseilla](resource-group-template-deploy-cli.md). Käy tausta Azure resurssit ja Resurssienhallinta- [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Hallittavan Azure-CLI Azure resursseilla sinun on [asennettava Azure-CLI](xplat-cli-install.md)ja [Azure kirjautuminen](xplat-cli-connect.md) käyttämällä `azure login` komento. Varmista, CLI on Resurssienhallinta-tilassa (Suorita `azure config mode arm`). Jos olet tehnyt nämä asiat, olet valmis.



## <a name="get-resource-groups-and-resources"></a>Resurssiryhmien ja resurssien

### <a name="resource-groups"></a>Resurssiryhmät

Saat kaikki resurssiryhmien tilaus ja niiden sijainnit-luettelo-komennon suorittaminen

    azure group list
    

### <a name="resources"></a>Resurssit
 Luettele kaikki resurssit ryhmästä, esimerkiksi nimi *testRG*, jossa käyttäminen

    azure resource list testRG

Voit tarkastella ryhmän sisällä yksittäinen resurssi, kuten AM nimeltä *MyUbuntuVM*, käytä komentoa, kuten jompikumpi seuraavista.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Huomaa **Microsoft.Compute/virtualMachines** -parametrin. Tämä parametri ilmaisee pyydät tietoja resurssin lajin.
    
>[AZURE.NOTE]**Luettelo** -komento kuin **azure resurssi** -komentoja käytettäessä resurssin API-versio on määritettävä **+ o** -parametrin. Jos ole varma API-versio, ota mallitiedoston ja Etsi apiVersion-kenttään resurssin. Lisätietoja API versiot resurssien hallinta on [Resurssienhallinta tarjoajien, alueet, API-versiot ja rakenteet](resource-manager-supported-services.md).

Kun tarkastelet resurssin tiedot, on usein kannattaa käyttää `--json` parametria. Tämä parametri on tulosteen luettavuutta, koska joitakin arvoja sisäkkäisiä rakenteita tai sivustokokoelmat. Seuraavassa esimerkissä on kuvattu, joka palauttaa tuloksia JSON-asiakirjana **Näytä** -komento.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Voit halutessasi tallentaa JSON tiedot-tiedoston avulla &gt; merkin, voit ohjata tiedostoon. Esimerkki:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Tunnisteet

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Resurssien hallinta


Voit lisätä resurssiryhmä resurssi, kuten tallennustilan tiliä, suorita komento, joka on samalla tavalla kuin:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Lisäksi **+ o** -parametrin, joka määrittää resurssin API-versio, välittää JSON-muotoiset merkkijono, joka sisältää kaikki tarvittavat tai muita ominaisuuksia **-p** -parametrin avulla.
    
    
Jos haluat poistaa aiemmin luotu resurssi, kuten virtuaalikoneen resurssin, käytä komento, kuten jompikumpi seuraavista.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Jos haluat siirtää aiemmin resurssien toiseen resurssiryhmä tai tilauksen, komennolla **Siirry azure resurssi** . Seuraavassa esimerkissä, voit siirtää Redis välimuistin uusi resurssiryhmä. Ole **-i** -parametrin resurssitunnus CSV-luettelo on siirrettävä.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Resurssien käytön valvominen

Azure-CLI avulla voit luoda ja hallita käytännöt Azure resurssien hallintaa. Käytännön määritykset ja varataan käytännöt resurssien tausta-kohdassa [resurssien hallintaa ja käyttöoikeuksien hallinta käytännön avulla](resource-manager-policy.md).

Esimerkiksi määrittää seuraavat käytännön oikeuden hylätä kaikki pyynnöt, jossa sijainti ei ole Länsi US tai Pohjois keskitetyn US ja tallenna se käytännön määritys tiedoston policy.json:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Suorita sitten **käytännön määritys luominen** -komento:

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Tämä komento näkyy tulosteen seuraavankaltaiselta.

    + Käytännön määritys MyPolicy tietojen luominen: käytännön nimi: MyPolicy tiedot: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    tiedot: PolicyType: Mukautettu tietojen: näyttönimi: Määrittämätön tietojen: kuvaus: Määrittämätön tietojen: PolicyRule: kentän = sijaintia, kirjoita = [westus, northcentralus]-tehosteen = estä

 Haluat, käytä edellisen komennon palauttama **PolicyDefinitionId** alueessa käytännön määrittäminen. Seuraavassa esimerkissä Tämä alue on tilaus, mutta voit rajoittaa resurssiryhmille tai yksittäisille resursseille:

    Azure käytännön määrityksen luominen MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Voit saada, muuttaminen tai poistaminen käytäntöjen määritelmät **käytännön määritys näyttää**ja **käytännön määritys määrittää** **käytännön määritys poistaminen** -komennoilla.

Voit vastaavasti Hae, muuttaminen tai poistaminen käytännön varaukset **käytännön määritys näyttää**ja **käytännön määritys määrittää** **käytännön määrityksen poistaminen** -komennoilla.


## <a name="export-a-resource-group-as-a-template"></a>Vie resurssiryhmä tallentaminen mallina

Tarkastele aiemmin resurssiryhmä resurssiryhmän Resurssienhallinta mallia. Mallin vieminen on kaksi edut:

1. Voit automatisoida ratkaisun tulevien versioiden helposti, koska kaikki infrastruktuuri on määritetty malli.

2. Voit tutustut mallin syntaksi katsomalla JSON, joka edustaa ratkaisu.

Käytä Azure-CLI, voit joko Vie malli, joka vastaa resurssiryhmä nykyisen tilan tai Lataa malli, jota käytettiin tietyn käyttöönottoa varten.

* **Vie mallin resurssiryhmä** - tästä on hyötyä, kun tehdyt muutokset resurssiryhmä ja hakeminen nykyisestä tilasta JSON-esitys. Kuitenkin luotu mallissa on vain vähän parametrit ja muuttujien ei ole. Arvot-mallissa on koodattu. Ennen kuin otat luotu mallin, haluat ehkä muuntaa Lisää arvot parametrit niin voit mukauttaa eri ympäristöissä käyttöönotto.

    Resurssiryhmä mallin vieminen paikallisessa kansiossa, suorita `azure group export` komento, kuten seuraavassa esimerkissä. (Korvaa oikea käyttöjärjestelmä ympäristössä paikallisessa kansiossa).

        azure group export testRG ~/azure/templates/

* **Lataa malli tietyn käyttöönottoa** --tästä on hyötyä, kun haluat tarkastella todellinen mallin, jota käytettiin ottamaan resurssit. Malli sisältää kaikki parametrit ja määritetyn alkuperäisen käyttöönottoa varten. Jos joku muu organisaatiossa muuttanut resurssiryhmä mallin määritelmän ulkopuolella, tätä mallia ei vastaa resurssiryhmän nykyisen tilan.

    Suorita lataamaan tietyn käyttöönottoa paikallisessa kansiossa malli `azure group deployment template download` komento. Esimerkki:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Mallin vienti on esikatselu ja kaikki resurssityypit tällä hetkellä tueta vieminen mallina. Kun yrität viedä mallin, näkyviin voi tulla virhe, joka ilmoittaa resurssien ei viety. Tarvittaessa manuaalisesti määritellä nämä resurssit malliin ladata sen verkosta.



## <a name="next-steps"></a>Seuraavat vaiheet

* Saat yksityiskohtaiset tiedot käyttöönotto-toimintoa ja käyttöönoton vianmääritys Azure-CLI kanssa on ohjeaiheessa [Azure CLI näkymän käyttöönottotoimintoja](resource-manager-troubleshoot-deployments-cli.md).
* Jos haluat määrittää sovelluksen tai komentosarjan resurssien käytön CLI avulla, katso [Käytä Azure CLI Luo pääasiallista resursseihin palvelu](resource-group-authenticate-service-principal-cli.md).



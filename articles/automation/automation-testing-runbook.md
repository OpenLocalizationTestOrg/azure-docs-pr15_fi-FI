<properties 
    pageTitle="Testaus runbookin Azure automaatio | Microsoft Azure"
    description="Ennen kuin julkaiset Azure automaatio runbookin, voit kokeilla sitä varmistaakseen, joka toimii odotetulla tavalla.  Tässä artikkelissa käsitellään Testaa runbookin ja tarkastella sen Tulosta."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Azure automaatio runbookin testaaminen
Kun testaat runbookin, [Luonnos versio](automation-creating-importing-runbook.md#publishing-a-runbook) on suoritettu ja se on suorittanut kaikki toiminnot on tehty. Ei ole Työhistoria luodaan, mutta [tulostus](automation-runbook-output-and-messages.md#output-stream) ja [Varoitus ja virhe](automation-runbook-output-and-messages.md#message-streams) virtaa näkyvät testi tulosteen ruutu. [Yksityiskohtainen Stream](automation-runbook-output-and-messages.md#message-streams) viestit näkyvät tulostus-ruudussa vain, jos [$VerbosePreference muuttuja](automation-runbook-output-and-messages.md#preference-variables) on määritetty Jatka.

Vaikka luonnos-versio suoritetaan,: n runbookin suorittaa työnkulun tavallisesti edelleen ja suorittaa toimintoja vastaan resurssit-ympäristössä. Tästä syystä Testaa vain runbooks tuotannon resurssit-palvelussa.

Voit esikatsella jokaisen [runbookin tyypin](automation-runbook-types.md) toiminto on sama ja on testauksessa tekstimuotoinen editorin ja Azure-portaalissa graafinen editorin välillä ei ole ero.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Testaa runbookin Azure-portaalissa

Voit käyttää mitä tahansa [runbookin tyyppi](automation-runbook-types.md) Azure-portaalissa.

1. Avaa [tekstiä editorin](automation-editing-a-runbook.md#Portal) tai [graafisen editorin](automation-graphical-authoring-intro.md): n runbookin luonnos-versiota.
2. Valitse **Testaa** -painikkeen napsauttaminen avaa testi-sivu.
3. Jos n runbookin on parametreja, ne näkyvät vasemmanpuoleisessa ruudussa, johon voidaan lisätä testi käytettävät arvot.
4. Jos haluat suorittaa testin [Hybrid Runbookin työntekijä](automation-hybrid-runbook-worker.md), **Suorita asetusten** muuttaminen **Hybrid työntekijä** ja valitse kohderyhmän nimi.  Muussa tapauksessa oletusasetus **Azure** testin suorittaminen pilveen.
5. Aloita testi valitsemalla **Käynnistä-painiketta** .
6. Jos n runbookin on [PowerShell työnkulun](automation-runbook-types.md#powershell-workflow-runbooks) tai [graafiset](automation-runbook-types.md#graphical-runbooks), voit lopettaa tai keskeyttää sen, kun se testataan olevilla painikkeilla tulostus-ruudun alapuolella. Kun keskeytät: n runbookin, se suorittaa valitun tehtävän ennen on hyllytetty. Kun n runbookin keskeytetään, voit lopettaa sen tai käynnistä se uudelleen.
7. Tarkasta tuloste runbookin tulostus-ruudussa.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja siitä, miten voit luoda tai tuoda runbookin on artikkelissa [luominen tai tuominen Azure automaatio-runbookin](automation-creating-importing-runbook.md)
- Lisätietoja graafinen yhtä aikaa muiden kanssa on artikkelissa [graafiset yhteiskäyttö Azure automaatio](automation-graphical-authoring-intro.md)
- Aloita PowerShell työnkulun runbooks, katso [ensimmäinen PowerShell työnkulun-runbookin](automation-first-runbook-textual.md)
- Katso lisätietoja määrittäminen runboks palauttaa tilasanomat ja virheet, mukaan lukien suositellut käytännöt [Runbookin tulostus ja Azure Automaattiset viestien](automation-runbook-output-and-messages.md)
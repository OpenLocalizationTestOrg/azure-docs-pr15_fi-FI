<properties
   pageTitle="Tietoja resurssien hallinta | Microsoft Azure"
   description="Toimintaohjeet artikkelissa hallinta näkyvyyttä ja omistajuutta tietojen korostaminen rekisteröity Azure tietoluettelon."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-manage-data-assets"></a>Tietoja resurssien hallinta

## <a name="introduction"></a>Johdanto

**Azure tietoluettelon** sisältää ominaisuuksia, joilla tietojen lähteen etsiminen, käyttäjät voivat helposti löydät ja ymmärtää analysoinnissa ja päätösten tarvitsemiaan tietolähteitä. Etsinnän näiden ominaisuuksien tehdä suurimmista vaikutus, kun kaikki käyttäjät voivat löytää ja ymmärtää laaja solualue, käytettävissä olevat tietolähteet. Tässä yhteydessä tietoluettelon oletusasetuksista on kaikki rekisteröityjä tietolähteiden näkyvissä – ja havaittavissa mukaan – kaikki luettelon käyttäjät.

Tietoluettelo ei antaa käyttäjille itse tietoihin. Tietojen käyttöoikeuksia tietolähteen omistaja. Tietoluettelon avulla käyttäjät löydät tietolähteitä sekä tarkastella luettelon rekisteröity käyttömahdollisuus liittyvät metatiedot.

Voi olla tilanteessa, jossa tietolähteisiin olisi on näkyvissä vain tietyille käyttäjille tai tietyn ryhmissä. Näissä tilanteissa tietoluettelon avulla käyttäjät voivat omistajaksi kuluessa luettelon resurssien rekisteröidyt tiedot ja sitten ohjausobjektiin varat näkyvyyden ne ole.

> [AZURE.NOTE] Tässä artikkelissa kuvatut toiminnot ovat käytettävissä vain tietoluettelossa Standard Edition, Azure. Vapaa Edition ei tarjoa omistus-ja rajoittavat tietojen kohteiden näkyvyyttä.

## <a name="managing-ownership-of-data-assets"></a>Tietojen hallinta omistajuutta
Oletusarvon mukaan tietojen varat rekisteröity tietoluettelon ovat unowned; käyttäjät, joilla on oikeus käyttää luettelon voi tutkia ja huomautuksia nämä resurssit. Käyttäjille voi kestää unowned tietojen omistajuutta ja rajoittaa näkyvyyttä ne omia kohteita.

Kun tietojen tiedot annetaan omistaa vain käyttäjät, jotka on myöntänyt omistajat voi tutkia kohteen ja tarkastella sen metatietoja ja vain omistajat voivat poistaa kohteen luettelosta.

> [AZURE.NOTE] Tietojen omistajuus koskee vain tallennetut luettelon metatiedot. Se ei tuota kaikki pohjana olevassa tietolähteessä oikeudet.

### <a name="taking-ownership"></a>Ottaen omistajuus
Käyttäjille voi kestää omistajuutta tiedot valitsemalla tietoluettelon portaalissa "Kestää omistajuus"-vaihtoehto. Erityiset ei ole oikeuksia tarvitaan omistajuutta unowned tietojen sijoituksen; Kuka tahansa käyttäjä voi omistajaksi unowned tietojen resurssi.

### <a name="adding-owners-and-co-owners"></a>Lisääminen ja muokkaaminen yhdessä omistajille
Jos tietojen resurssi jo omistaa käyttäjät et voi ainoastaan omistajuutta, vaan ne on lisättävä työtovereiden omistajiksi aiemmin omistaja. Minkä tahansa omistaja voi lisätä uusia käyttäjät tai käyttöoikeusryhmät työtovereiden omistajiksi.

> [AZURE.NOTE] Paras käytäntö on vähintään kaksi henkilöille, minkä tahansa omistama tiedot annetaan omistajiksi on.

### <a name="removing-owners"></a>Poista omistajat
Samalla tavalla kuin minkä tahansa resurssi omistaja voi lisätä muiden omistajien, kaikki resurssi omistaja voi poistaa minkä tahansa rinnakkaisomistajan.

Jos resurssi omistaja poistaa itse omistajaksi, hän ei enää voi hallita kohteen. Jos resurssi omistaja poistaa itse omistaja ja ei ole muiden omistajien, kohteen takaisin unowned tila.

## <a name="visibility"></a>Näkyvyys
Tietojen resurssi omistajien voit hallita niiden omia tietoja resurssien näkyvyyden. Rajoita näkyvyyden oletusarvo – missä kaikki tietoluettelon käyttäjät voi tutkia ja tarkastella tietoja – resurssi resurssi omistaja siirtyä näkyvyysasetus "Kaikki" lähettäjä "Omistajat ja nämä käyttäjät" kohteen ominaisuuksissa. Omistajat jälkeen voit lisätä tietyille käyttäjille ja käyttöoikeusryhmät.

> [AZURE.NOTE] Mahdollisuuksien mukaan resurssi omistus- ja näkyvyys-käyttöoikeudet määritetään, käyttöoikeusryhmät eikä yksittäisille käyttäjille.

## <a name="catalog-administrators"></a>Luettelon Järjestelmänvalvojat
Tietoluettelon järjestelmänvalvojat voivat implisiittisesti muiden omistajien kaikki luettelon kohteita. Kohteiden omistajat näkyvyyttä ei voi poistaa luettelon Järjestelmänvalvojat ja järjestelmänvalvojat voivat hallita omistus- ja kaikki luettelon tietojen varat näkyvyyden.

## <a name="summary"></a>Yhteenveto
Metatietojen ja tietojen kohteiden etsiminen Data Catalog crowdsourcing mallin avulla kaikki luettelon käyttäjät voivat osallistua ja etsiminen. Standard Edition, tietoluettelon sisältää ominaisuuksia, joilla omistus- ja haluat rajoittaa näkyvyyttä ja tietyt tiedot resurssien käyttö.

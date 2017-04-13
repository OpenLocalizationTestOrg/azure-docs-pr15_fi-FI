<properties
   pageTitle="Sovellukset yksittäiset käyttäjät voivat julkaista Azure RemoteApp-sivustokokoelman (ennakkoversio) | Microsoft Azure"
   description="Katso, miten voit julkaista sovellusten yksittäisten käyttäjien sijaan Azure RemoteApp ryhmissä mukaan."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Sovellukset yksittäiset käyttäjät voivat julkaista Azure RemoteApp-sivustokokoelman (ennakkoversio)

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Tässä artikkelissa kerrotaan, miten voit julkaista sovelluksia yksittäisille käyttäjille Azure RemoteApp-sivustokokoelman. Azure RemoteApp uudet toiminnot on tällä hetkellä "yksityinen esikatselu"- ja käytettävissä vain, jos haluat valita alkuvaiheen käyttäjät arviointia varten.

Alun perin Azure RemoteApp käytössä vain yksi tapa "julkaisun" sovellusten: järjestelmänvalvoja julkaista kuvan sovelluksia ja olisi näkyvät kaikille käyttäjille kokoelmasta.

Käytetty vaihtoehto on useita sovelluksia sisällyttäminen yksittäisen kuvan ja ottaa käyttöön yhden sivustokokoelman vähentämiseksi kustannuksia. Oftentimes kaikki sovellukset eivät kaikkia käyttäjiä – Julkaise sovellukset yksittäisille käyttäjille, jotta syötteen sovelluksessa tarpeettomia sovelluksia muut eivät näe mieluummin järjestelmänvalvojat.

Tämä on nyt mahdollista Azure RemoteApp – tällä hetkellä kuin rajoitettu esikatselu-toimintoa. Seuraavassa on lyhyt yhteenveto uudet ominaisuudet:

1. Kokoelma määrittää kyselyjä seuraavilla tavoilla:
 
  - Alkuperäinen-"sivustokokoelman tilassa", jossa kokoelman kaikki käyttäjät voivat nähdä kaikki julkaistut sovellukset. Tämä on oletusarvo.
  - Uusi-sovelluksen tila", johon käyttäjät näkevät vain sovellukset, jotka on nimenomaisesti määritetty

2. Tällä hetkellä sovelluksen tila voi ottaa käyttöön vain käyttämällä Azure RemoteApp PowerShellin cmdlet-komennot.

  - Jos määritetty sovelluksen tilaan, käyttäjän varauksen kokoelmassa ei voi hallita palvelun Azure-portaalissa. Käyttäjän tehtävällä on hallitaan PowerShellin cmdlet-komennot.

3. Käyttäjät näkevät vain suoraan niitä julkaista sovellukset. Kuitenkaan voi silti mahdollista käyttäjä voi käyttää niitä suoraan-käyttöjärjestelmässä Avaa muita käytettävissä kuva sovelluksia.
  - Tämä ominaisuus ei tarjoa suojatun lukitus sovellusten; se rajoittaa vain näkyvyys-syöte-sovelluksessa.
  - Jos haluat erottaa sovellusten käyttäjiä, sinun on erillinen sivustokokoelmat käytettävät.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Saaminen Azure RemoteApp PowerShellin cmdlet-komennot

Kokeile uuden Esikatselutoiminto, sinun on käytettävä Azure PowerShellin cmdlet-komennot. Tällä hetkellä ei ole mahdollista Azure hallinta-portaalin avulla voit ottaa käyttöön uusi sovellusten julkaisun tila.

Varmista ensin, että sinulla on asennettu [PowerShellin Azure moduuli](../powershell-install-configure.md) .

Valitse Käynnistä PowerShell console järjestelmänvalvoja-tilassa ja suorita seuraavat cmdlet-komento:

        Add-AzureAccount

Se kehottaa sinua Azure käyttäjänimeä ja salasanaa. Kun olet kirjautunut sisään, voi suorittaa Azure tilauksistasi Azure RemoteApp cmdlet-komennot.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Miten tarkistetaan, mihin tila on sivustokokoelman ominaisuudet

Suorita seuraava cmdlet-komento:

        Get-AzureRemoteAppCollection <collectionName>

![Valitse sivustokokoelman-tila](./media/remoteapp-perapp/araacllelvel.png)

AclLevel-ominaisuutta voi olla seuraavat arvot:

- Sivustokokoelman: alkuperäinen julkaisun tila. Kaikki käyttäjät näkevät kaikki julkaistut sovellukset.
- Sovellus: uuden julkaisun tila. Käyttäjät näkevät vain suoraan niitä julkaista sovellukset.

## <a name="how-to-switch-to-application-publishing-mode"></a>Sovelluksen julkaisun tilaan vaihtaminen

Suorita seuraava cmdlet-komento:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Sovelluksen julkaisun tila säilytetään: aluksi kaikki käyttäjät näkevät kaikki alkuperäisen julkaistun sovellukset.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Miten käyttäjät, kuka voi nähdä tietyn sovelluksen luettelo

Suorita seuraava cmdlet-komento:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Tämä näyttää kaikki käyttäjät, kuka voi nähdä sovelluksen.

Huomautus: Näet sovelluksen aliakset (jota kutsutaan "sovelluksen alias" yllä olevaa syntaksia) suorittamalla Get-AzureRemoteAppProgram - CollectionName <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Sovelluksen käyttäjälle määrittäminen

Suorita seuraava cmdlet-komento:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Käyttäjä näkee sovelluksen Azure RemoteApp-asiakasohjelmassa ja voivat muodostaa yhteyden.

## <a name="how-to-remove-an-application-from-a-user"></a>Sovelluksen poistaminen käyttäjältä

Suorita seuraava cmdlet-komento:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Palautteen antaminen
Arvostamme palaute ja ehdotukset koskevat tämän esikatselu-toimintoa. Anna meille, mielipiteesi [kyselyn](http://www.instant.ly/s/FDdrb) .

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Etkö tiedä vielä voit kokeilla esikatselu-toimintoa?
Jos olet ei osallistuneet esikatselu vielä, käytä [kyselyn](http://www.instant.ly/s/AY83p) access pyytää.

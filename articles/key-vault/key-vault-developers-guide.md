<properties
   pageTitle="Avain kassakaapin Developer's Guide | Microsoftin Azure"
   description="Kehittäjät voivat käyttää Azure avain kassakaapin ja hallitsevat salausavaimia Microsoftin Azure-ympäristöön. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Azure avain kassakaapin kehittäjän opas
Avulla avain kassakaapin, voi turvallisesti käyttää luottamuksellisia tietoja, jotka sovellukset voivat siten, että:

- Avaimet ja salaisuudet suojataan tarvitsematta kirjoittaa koodia ja pystyt helposti käyttämään niitä sovelluksia.
- Pystyt omalla asiakkaille ja hallita omia avaimia, joten voit keskittyä tarjoaa tärkeimmät ohjelmisto-ominaisuudet. Näin sovellukset ei omista vastuuta tai vastuuta mahdollisten asiakkaiden vuokralaisen avaimet ja salaisuudet.
- Sovellus voi käyttää allekirjoittamiseen avaimet ja vielä salaus pitää avainten hallinta ulkoisessa sovelluksessa siten, että sovellus, joka on maantieteellisesti hajautettu ratkaisu sopii.

- Avain kassakaapin syyskuuta 2016 versiossa sovellukset voidaan nyt hyödyntää avain kassakaapin, todistukset. Lisätietoja, katso **avaimet ja varmenteet salaisuudet,** artiklan [viittaus MUUALLA](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Saat lisätietoja Azure avain kassakaapin yleisempiä lisätietoja [on avain kassakaapin](key-vault-whatis.md).

## <a name="videos"></a>Videot
Tässä videossa näytetään, miten voit luoda omia avain kassakaapin ja miten sitä käytetään 'Hei avain kassakaapin'-esimerkkisovelluksen.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Linkit mainittu video resursseja:
- [Azure-PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure avain kassakaapin mallikoodi](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Saat enemmän voit [Avain kassakaapin blogi](http://aka.ms/kvblog) seurata ja osallistua [Avain kassakaapin foorumi](http://aka.ms/kvforum).

## <a name="creating-and-managing-key-vaults"></a>Luonti- ja hallintaohjeisiin avaimen Vaults

Ennen kuin jatkat Azure avain kassakaapin koodin, voit luoda ja hallita vaults REST, henkilöstöpäällikkö malleja, PowerShell tai CLI, jotka on kuvattu seuraavissa artikkeleissa:

- [Luo ja hallitse avaimen kanssa LOPUT Vaults](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Luo ja hallitse PowerShellin tärkeimpiä Vaults](key-vault-get-started.md)
- [Luo ja hallitse avaimen kanssa CLI Vaults](key-vault-manage-with-cli.md)
- [Luo avain kassakaapin ja lisää secret kautta Azure Resource Manager-malli](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Vastaan vaults tärkeimmät toiminnot ovat AAD – valtuutetuille ja todennetuille kautta määritellään kassakaapin avain kassakaapin 's oma käyttöoikeuskäytännön.

## <a name="coding-with-key-vault"></a>Koodaus avain kassakaapin kanssa

Ohjelmoijille kassakaapin avain hallinta-Järjestelmä koostuu useita liityntäkohtia MUUALLA kuin säätiön [Avain kassakaapin REST API Reference](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Voi, onnistunut lupa seuraavat toimet:

- Hallitsevat salausavaimia käyttäen [Luo](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Tuo](https://msdn.microsoft.com/library/azure/dn903626.aspx), [Päivitä](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Poista](https://msdn.microsoft.com/library/azure/dn903611.aspx) ja muita toimintoja

- Hallinta [, [Päivitä](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Poista](https://msdn.microsoft.com/library/azure/dn903613.aspx) ja muita toimintoja](https://msdn.microsoft.com/library/azure/dn903633.aspx)käyttäen salaisuudet

- Käyttävät salausavaimia [merkki](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Tarkista](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) ja [Salaa](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[purkaa](https://msdn.microsoft.com/library/azure/dn878097.aspx) toiminnot

Seuraavat SDK: T ovat avain kassakaapin käsittelyyn käytettävissä:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK-ohjeissa](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js SDK-ohjeissa](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[SDK-paketti .NET-Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK-paketti](https://www.npmjs.com/package/azure-keyvault)|

Saat lisätietoja .NET SDK 2.x-version [julkaisutiedot](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Esimerkkikoodi
Valmis esimerkkejä avain kassakaapin avulla sovellukset Katso:

- Esimerkkisovelluksessa .NET *HelloKeyVault* ja Azure web service-esimerkki. [Azure avain kassakaapin koodiesimerkkejä](http://www.microsoft.com/download/details.aspx?id=45343)
- Opetusohjelman avulla voit opetella käyttämään WWW-sovelluksesta, Azure, Azure avain kassakaapin. [Käytä Azure avain kassakaapin WWW-sovelluksesta](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>How-TOS

Seuraavissa artikkeleissa ja skenaarioita on tehtävä erityisiä ohjeita Azure avain kassakaapin käsittelemistä varten:

- [Muuta avaimen säilö vuokralaisen tunnus siirtää tilauksen jälkeen](key-vault-subscription-move-fix.md) - kun tilauksesi Azure siirtää vuokralaisen A B vuokralaisen aiemmin oman avaimen vaults ovat vuokralaisen B. korjaus käyttää tämän oppaan-suojausobjektien (käyttäjien ja sovellusten) mukaan ole käytettävissä.
- [Avain kassakaapin käyttö palomuurin takana](key-vault-access-behind-firewall.md) - avaimen säilö, avaimen säilö-asiakassovellus voi käyttää useita eri toimintoja varten perusteltujen tarvitsee käyttää.

- [Luo ja Azure avain kassakaapin avaimet Transfer HSM-Protected](key-vault-hsm-protected-keys.md) - tämän avulla voit suunnitella, luoda ja siirtää sitten oman:: HSM suojattu käytettävät näppäimet Azure avain kassakaapin.
- [Miten siirtää suojatun arvot (esimerkiksi salasanat) käyttöönoton aikana](../resource-manager-keyvault-parameter.md) - kun haluat välittää parametreina turvallinen arvo (esimerkiksi salasanan) käyttöönoton aikana voit tallentaa arvon kuin salainen avain kassakaapin Azure-ja henkilöstöpäällikkö muita malleja, arvon viittaus.
- [Käyttäminen avain kassakaapin extensible avainten hallinta SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - Azure avaimen säilö SQL Server Connector mahdollistaa Azure avain kassakaapin palvelun sovelluskehitysympäristönä Extensible Key Management (EKM)-palvelun, voit suojata salausavaimensa hakemukset-linkin SQL Serverin ja SQL-tietokannassa--VM Läpinäkyvä tiedon salaus, salauksen varmuuskopioinnin ja sarakkeen tason salausta.
- [VMs-avain kassakaapin varmenteiden käyttöönotto](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - VM: ssä käytössä Azure cloud-sovellus tarvitsee todistus. Miten saat tämän varmenteen kyselyjä tässä VM tänään?
- [Miten asennus avain kassakaapin avain päästä päähän-kierto ja seuranta](key-vault-key-rotation-log-monitoring.md) - tämä kerrotaan miten avaimen kierto ja Azure avain kassakaapin kanssa valvonnan kautta.

Integrointi ja Azure Vaults avaimen käyttäminen lisää-tehtäväkohtaisia ohjeita Saat esimerkkejä [Ryan Jonesin Azure henkilöstöpäällikkö malli on kassakaapin avain](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Integroitu avaimen säilö

Seuraavissa artikkeleissa käsitellään muita skenaarioita ja tehdä meille, tai avain kassakaapin integroida palveluja.

- [Azure salauksen](../security/azure-security-disk-encryption.md) avulla teollisuuden standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -versiota Windows ja Linux antaa aseman salauksen OS ja data-levyt [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -toiminnon. Ratkaisu integroituu Azure avain kassakaapin avulla voit valvoa ja hallita levyn salausavaimia ja salaisuudet avain kassakaapin Tilauksessasi, varmistaen VM-levyjen kaikki tiedot salataan levossa Azure tallennuspaikkaan.


## <a name="supporting-libraries"></a>Sitä tukevat kirjastot

- [Microsoftin Azure avain kassakaapin Core kirjasto](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) tarjoaa `IKey` ja `IKeyResolver` liittymät, jolla etsitään tunnisteet avaimet ja operaatioiden avaimilla suorittamisesta.

- [Microsoftin Azure avain kassakaapin laajennukset](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) tarjoaa laajan palveluvalikoiman Azure avain kassakaapin.

## <a name="other-key-vault-resources"></a>Avain kassakaapin muihin resursseihin
- [Avain kassakaapin blogi](http://aka.ms/kvblog)
- [Kassakaapin avain-Forum](http://aka.ms/kvforum)

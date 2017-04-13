<properties 
    pageTitle="Palvelun Bus todennus- ja | Microsoft Azure"
    description="Jaettu Access allekirjoituksen (SAS) todennusta yleiskatsaus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Palvelun Bus todennus- ja

Sovellusten voi todentaa Azure palvelun Bus joko jaettu Access allekirjoitus (SAS) todennusta tai Azure Active Directory käyttöoikeuksien hallinta (tunnetaan myös nimellä Access Control-palvelu tai ACS) kautta. Jaettu Access allekirjoituksen todennus mahdollistaa sovellusten todentaa palvelun Bus määritetty nimitila tai kohde, johon on liitetty oikeudet access-näppäintä. Voit sitten Luo jaettu Access allekirjoitus-tunnuksen, joka todentaa palvelun Bus asiakkaat voivat käyttää tätä näppäintä.

>AZURE. TÄRKEÄÄ SAS suositellaan ACS, päälle, kun se on yksinkertainen, joustava ja helposti käytettävällä todennusta värimallin palvelun Bus. Sovellukset voivat käyttää SAS tilanteissa, joissa niitä ei tarvitse hallinta käsite hyväksytylle "käyttäjälle."

## <a name="shared-access-signature-authentication"></a>Jaetun Access allekirjoitus-todennus

[SAS todennus](service-bus-sas-overview.md) mahdollistaa käyttöoikeuden palvelun Bus resurssien tietyn oikeuksilla. SAS todennus-palvelun Bus liittyy määritykset salausavaimen palvelun Bus resurssi liitetty käyttöoikeudet. Asiakkaiden sitten pääset käyttämään resurssia, jos SAS tunnus, joka koostuu resurssin URI käytetään ja määräajan allekirjoitettu määritetty avain.

Voit määrittää näppäimet suojaussidosten palvelun Bus nimitila. Avaimen koskee kyseisen nimitilan tekstiviesti kohteilla. Voit myös määrittää näppäimet palvelun Bus olevien ja ohjeita. SAS tuetaan myös palvelun Bus välittää.

Käyttämään SAS voidaan määrittää [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) objektin nimitilan, jonossa tai ohjeaiheen, joka koostuu seuraavasti:

- *Avain* , joka määrittää säännön.

- *PrimaryKey* on salausavaimen, kirjaudu ja vahvista SAS tunnusten avulla.

- *ToissijAvain* on salausavaimen, kirjaudu ja vahvista SAS tunnusten avulla.

- *Oikeuksien* edustava kuunnella, Lähetä tai Hallitse oikeuksien sivustokokoelman.

Käyttöoikeuksien myöntämissääntöjä määritetty nimitilan tasolla voit myöntää access-nimitilaa kaikkien kohteiden asiakkaiden kanssa tunnusten kirjautunut vastaavan avaimen avulla. Enintään 12 lupa säännöt voi määrittää palvelun Bus nimitilan, jonossa tai aihe. Oletusarvon mukaan [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) kaikki oikeudet määritetään jokaisen nimitilan, kun se on ensin valmistelun yhteydessä.

Voit käyttää kohdetta, asiakkaan edellyttää luotu tietyn [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)SAS-tunnuksen. SAS tunnuksen luodaan HMAC SHA256 resurssin merkkijono, joka koostuu, johon access on haettu resurssin URI ja määräajan käyttäminen salausavaimen, liittyvät käyttöoikeuksien sääntöä.

Palvelun Bus SAS todennus tuki on Azure .NET SDK-versioissa 2.0 ja uudemmissa. SAS sisältää [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)tuen. Kaikki, jotka hyväksyvät yhteysmerkkijonon parametrina ohjelmointirajapinnan sisältävät tuen SAS yhteyden merkkijonoja.

## <a name="acs-authentication"></a>ACS-todennus

Palvelun Bus todennusta ja ACS hallitaan avulla avustaja "-sb" ACS nimitilan. Avustaja-ACS nimitilan luodaan palvelun Bus nimitilan halutessasi ei voi luoda oman palvelun Bus nimitilan Azure perinteinen portaalissa; Sinun on luotava [Uusi AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) PowerShell-cmdlet-komennolla nimitilan. Esimerkki:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Luominen ACS nimitilan välttämiseksi myöntää seuraava komento:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Esimerkiksi jos luot kutsutaan **contoso.servicebus.windows.net**palvelun Bus nimitilan, kutsutaan **contoso sb.accesscontrol.windows.net** avustaja ACS nimitila on valmisteltu automaattisesti. Kaikki nimitilan, jotka on luotu ennen elokuussa 2014 luotiin myös mukana ACS-nimitilaa.

Oletusarvoisesti tämä avustaja ACS nimitila on valmisteltu oletusarvoisten palvelun käyttäjätietojen "omistaja", jolla kaikki oikeudet. Voit hankkia tarkasti rajattuja ohjausobjektin kautta ACS palvelun Bus kohteen määrittämällä haluamasi luottamussuhteet. Voit määrittää lisäpalvelu jäsenyyksiä palvelun Bus kohteiden käyttöoikeuksien hallinta.

Voit käyttää kohdetta, asiakkaan pyyntöihin SWT-tunnuksen ACS tarvittavat vaatimusten, jos sen tunnistetiedot. SWT tunnuksen sitten lähetettävä osana pyynnön palvelun Bus luvan kohteeseen access-asiakasohjelman käyttöön.

Palvelun Bus ACS todennus tuki on Azure .NET SDK-versioissa 2.0 ja uudemmissa. Todennus on [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx)tuki. Kaikki, jotka hyväksyvät yhteysmerkkijonon parametrina ohjelmointirajapinnan sisältyy ACS yhteyden merkkijonot tuki.

## <a name="next-steps"></a>Seuraavat vaiheet

Jatka lukeminen [jaettu Access allekirjoituksen todentaminen palvelun Bus](service-bus-shared-access-signature-authentication.md) lisätietoja Suojaussidokset.

Palvelun Bus SAS yleiskatsaus-kohdassa [Jaettu Access allekirjoitukset](service-bus-sas-overview.md).

Voit etsiä lisätietoja ACS tunnusten [Toimintaohje: pyytää tunnusta ACS OAuth RIVITYS-protokollan kautta](https://msdn.microsoft.com/library/hh674475.aspx).




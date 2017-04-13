<properties 
    pageTitle="Virhe todennus tunnistus" 
    description="Ohjatun active directory-yhteyden havaita ei ole yhteensopiva todennustyyppi" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Virhe todennus tunnistus

Ohjattu toiminto havaitsi etsittäessä Edellinen todennuskoodi ei ole yhteensopiva todennustyyppi.   

##<a name="what-is-being-checked"></a>Mitä tarkistetaan?

**Huomautus:** Tunnista projektin Edellinen todennuskoodi, jotta projekti on rakennettava.  Jos sinulla ei ole Edellinen todennuskoodi projektin tämä virhe, muodosta uudelleen ja yritä uudelleen.

###<a name="project-types"></a>Projektityypit

Ohjattu toiminto tarkistaa kehität, jotta se Lisää oikealle todennus logiikan projektiin projektin tyyppi.  Onko ohjauskoneen, joka johdetaan `ApiController` Projectissa, projektin pidetään WebAPI projektin.  Jos määritettynä on vain ohjaimet peräisin olevat `MVC.Controller` Projectissa, projektin pidetään MVC projektin.  Muu ei tue ohjatun toiminnon.  Löytyi projektien eivät ole tuettuja.

###<a name="compatible-authentication-code"></a>Yhteensopiva todentaminen koodi

Ohjatun toiminnon tarkistaa myös todennusasetukset, joka on määritetty aiemmin ohjatun haun avulla tai ovat yhteensopivia ohjatun haun avulla.  Jos kaikki asetukset ovat näkyvissä, viestiä pidetään re-entrant tapauksen ja ohjattu toiminto avautuu ja näkyviin asetukset.  Jos vain jotkin asetukset ovat näkyvissä, pidetään virhe-tapaus.

MVC projektin ohjattu toiminto tarkistaa jokin seuraavista asetuksista, mikä aiheuttaa edellisen ohjatun toiminnon käytöstä:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ohjattu toiminto tarkistaa jokin seuraavista asetuksista verkko-Ohjelmointirajapinnan Projectissa, mikä aiheuttaa edellisen ohjatun toiminnon käytöstä:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Ei ole yhteensopiva todennuskoodi

Lopuksi Ohjattu toiminto yrittää tunnistaa todennuskoodi-versiot, jotka on määritetty Visual Studio aiempien versioiden kanssa. Jos olet saanut tämän virheen, se tarkoittaa projektin sisältää yhteensopimaton todennustyyppi. Ohjattu toiminto havaitsee todennus Visual Studio aiempien versioiden seuraavanlaisia:

* Windows-todennus 
* Yksittäisten käyttäjätilien 
* Organisaation tili 
 

Tunnistaa Windows-todennuksen MVC projektissa, ohjattu toiminto etsitään `authentication` elementin **web.config** -tiedostosta.

<pre>
    &lt;määritysten&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;todennustila = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Windows-todennuksen tunnistaa verkko-Ohjelmointirajapinnan projektin, ohjattu toiminto etsitään `IISExpressWindowsAuthentication` elementin projektin **.csproj** tiedostosta:

<pre>
    &lt;Projektin&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;käytössä&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/projekti        &gt;
</pre>

Ohjatun toiminnon hakee esiintyvien yksittäisten käyttäjätilien todennus-paketti-elementin **Packages.config** -tiedostosta.

<pre>
    &lt;pakettien&gt;
        <span style="background-color: yellow">&lt;pakata id="Microsoft.AspNet.Identity.EntityFramework" version = "2.1.0" targetFramework = "net45" /&gt;</span>
    &lt;/paketteja&gt;
</pre>

Esiintyvien vanha lomakkeen Organisaatiotili todennusta ohjattu toiminto näyttää seuraavia elementtejä **Web.config-tiedostosta**:

<pre>
    &lt;määritysten&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;plusnäppäin = "HVT: alueen" arvo = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Voit muuttaa todennustyyppi, Poista yhteensopimaton todennustyyppi ja suorita ohjatun toiminnon uudelleen.

Lisätietoja on artikkelissa [Azure AD todennus tilanteita, joissa](active-directory-authentication-scenarios.md).
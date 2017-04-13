<properties 
    pageTitle="Aloittaminen Azure Active Directory ja Visual Studio yhdistetyt palvelut (MVC projektit) | Microsoft Azure" 
    description="Pikaviestien käyttäminen MVC projektien Azure Active Directory, kun luominen Visual Studiossa Azure AD tai liittämisestä yhdistetty palvelut" 
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

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Käytön aloittaminen Azure Active Directory- ja Visual Studio yhdistetyt palvelut (MVC projektit)

> [AZURE.SELECTOR]
> - [Käytön aloittaminen](vs-active-directory-dotnet-getting-started.md)
> - [Mitä tapahtui](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Access-ohjaimet todennusta 

Projektin kaikkien ohjainten on adorned **Hyväksy** -määrite. Tämän määritteen edellyttävät käyttäjä todennetaan ennen käyttöä nämä ohjaimet. Jotta voit käyttää anonyymisti ohjauskoneen, poista tämän määritteen ohjaimen. Jos haluat määrittää käyttöoikeudet eritellympiä tasolla, käytä määrite kummassakin menetelmässä, joka edellyttää sijaan soveltaminen controller-luokka.
 
##<a name="adding-signin--signout-controls"></a>Kirjautuminen lisääminen / uloskirjautuminen ohjausobjektit 

Jos haluat lisätä Sisäänkirjautuminen ja uloskirjautuminen ohjausobjekteja näkymään, voit tehdä **_LoginPartial.cshtml** osittainen näkymä toimintoja Lisää näkymiä. Tässä on esimerkki lisätään Vakio **_Layout.cshtml** näkymän ominaisuudet. (Huomaa kanssa luokan siirtymispalkki-Kutista jako viimeinen osa):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Lisätietoja Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 

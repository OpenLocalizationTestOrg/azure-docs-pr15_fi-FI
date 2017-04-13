<properties
    pageTitle="Azure AD-sovelluksen välityspalvelimen mukautettujen toimialueiden käsittelemisestä | Microsoft Azure"
    description="Kattaa käyttämisestä mukautettuja toimialueita välityspalvelimen Azure AD-sovelluksen kanssa."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Mukautettuja toimialueita välityspalvelimen Azure AD-sovelluksen käyttäminen

Oletus-toimialue avulla voit määrittää URL-Osoitetta sisäisten ja ulkoisten URL-Osoitteen käyttäminen sovelluksen niin, että käyttäjillä on vain yksi URL-Osoitteen muista käyttää sovellusta riippumatta siitä, missä ne ovat avaaminen. Näin voit myös yksittäisen pikakuvakkeen luominen Access-sovelluksen Ohjauspaneelin. Jos käytät Azure AD-sovelluksen välityspalvelimen myöntämä oletustoimialueen, ei ole lisämäärityksiä, sinun on otettava käyttöön toimialueen. Silloin, kun käytät mukautettua toimialuetta, on muutama seikka, sinun on suoritettava, varmista, että sovelluksen välityspalvelimen tunnistaa toimialueen ja vahvistaa sen varmenteet.

## <a name="selecting-your-custom-domain"></a>Mukautetun toimialueen valitseminen

1. Julkaise [Julkaise](active-directory-application-proxy-publish.md)-sovelluksissa sovelluksen välityspalvelimen sovelluksesi ohjeiden mukaisesti.
2. Kun sovellus näkyy sovellusten luettelossa, valitse se ja valitse **Määritä**.
3. Kirjoita mukautetun toimialueen **Ulkoisen URL-osoite**.
4. Jos ulkoisen URL-osoite on https-voit pyydetään lataamaan varmenteen niin, että Azure voit tarkistaa sovelluksen URL-osoite. Voit ladata yleismerkkivarmenne, joka vastaa sovelluksen ulkoisen URL-osoite. Toimialueessa on oltava oman [Azure vahvistettu toimialueiden](https://msdn.microsoft.com/library/azure/jj151788.aspx)luettelo. Azure on oltava varmenne toimialueen URL-osoite sovellus tai yleismerkkivarmenne, joka vastaa sovelluksen ulkoisen URL-osoite.
5. Varmista, että Lisää DNS-tietue, joka reitittää sisäinen URL-osoite sovellus, jonka avulla voi olla URL-Osoitetta sisäisten ja ulkoisten käyttöoikeus-ja sovelluksen pikakuvake on yhden käyttäjän sovellusten luettelossa.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Mukautettujen toimialueiden käsittelemisestä usein kysytyt kysymykset

K: Voinko valita on jo ladattu sertifikaatti ilman lataaminen uudelleen?  
A: aiemmin ladatut varmenteet automaattisesti sidottujen sovelluksen ja sovelluksen isäntänimi vastaavat täsmälleen yhden sertifikaattia.  

K: Miten voin lisätä sertifikaatin ja mihin tiedostomuotoon olisi viedyn varmenteen voi ladata?  
A: varmenteen olisi ladattuja sovelluksen määritys-sivulla. Varmenne on oltava PFX-tiedosto.  

K: Voinko ECC varmenteet voidaan käyttää?  
A: ei ei ole allekirjoituksen menetelmiä eksplisiittinen rajoitus.  

K: Voinko SAN varmenteet voidaan käyttää?  
V: Kyllä.  

K: Voinko varmenteet yleismerkkejä voidaan käyttää?  
V: Kyllä.  

K: eri varmenteen voidaan käyttää kunkin sovelluksen?  
V: Kyllä, jos kaksi sovellusten Jaa saman ulkoinen isäntä.  

K: Jos voin Rekisteröi uusi toimialue, voit käyttää kyseistä toimialuetta?  
V: Kyllä, toimialueiden luettelo syötetään vuokraajan vahvistama luettelosta.  

K: mitä tapahtuu, kun varmenteen päättyy?  
A: näyttöön tulee varoitus-sovelluksen määritys-sivulla varmenne-osassa. Kun käyttäjä yrittää käyttää sovellusta, Suojausvaroitus ponnahdusikkuna.  

K: mitä pitäisi tehdä, jos haluat korvata varmenteen tietyn-sovelluksen?  
Vastaus: lataa uutta varmennetta sovelluksen määritys-sivulla.  

K: Voinko varmenteen poistaa ja korvata sen?  
A: Kun lataat uutta varmennetta, jos vanha varmennetta ei ole käytössä toisessa sovelluksessa, se poistetaan automaattisesti.  

K: mitä tapahtuu, kun varmenne on kumottu?  
A: kumottujen varmenteiden tarkistukset, ei suoriteta varmenteet. Suojausvaroitus saattaa näkyä, kun käyttäjä yrittää käyttää sovellusta olevan selaimen mukaan.  

K: Voinko käyttää itse allekirjoitettua varmennetta?  
V: Kyllä, voivatko itse allekirjoitettua varmenteet. Huomaa, että jos käytät yksityinen varmenteen myöntäjä, CDP (varmenteen kumottujen varmenteiden pisteen jakauman piste) varmenteen tulee julkinen.  

K: onko paikka haluat nähdä kaikki varmenteet alihallintaan?  
A: Tämä nykyistä versiota ei tueta.  


## <a name="see-also"></a>Katso myös

- [Sovelluksen välityspalvelimen sovellusten julkaiseminen](active-directory-application-proxy-publish.md)
- [Ottaa käyttöön kertakirjautumisen](active-directory-application-proxy-sso-using-kcd.md)
- [Ehdollinen käytön käyttöön ottaminen](active-directory-application-proxy-conditional-access.md)
- [Mukautetun toimialuenimen lisääminen Azure AD](active-directory-add-domain.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)

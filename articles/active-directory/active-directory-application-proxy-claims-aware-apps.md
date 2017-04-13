<properties
    pageTitle="Huomioon Apps-sovelluksen välityspalvelimen saatavat käsitteleminen"
    description="Kerrotaan, kuinka pääset hyvin alkuun Azure AD-sovelluksen välityspalvelinta."
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



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Huomioon apps-sovelluksen välityspalvelimen saatavat käsitteleminen

Vaateet huomioon sovellusten suorittaa uudelleenohjauksen haluat suojauksen suojaustunnuksen Service (STS), joka puolestaan pyytää tunnistetietoja vastaan tunnusta käyttäjän ennen uudelleenohjausta käyttäjän sovelluksen. Jotta voit käyttää näitä Uudelleenohjaa välityspalvelinta seuraavat vaiheet on otettava.

## <a name="prerequisites"></a>Edellytykset
Ennen kuin käytät tätä toimintoa, tarkista, että saatavat huomioon sovelluksen ohjaa STS-ympäristö on käytettävissä paikallisen verkon ulkopuolella.

## <a name="azure-classic-portal-configuration"></a>Azure perinteinen portaalin määrittäminen

1. Julkaise sovelluksesi [Julkaise sovellusten sovelluksen välityspalvelimen](active-directory-application-proxy-publish.md)kuvattu ohjeiden mukaisesti.
2. -Sovellusten luettelossa valitse saatavat huomioon sovellus ja valitse **Määritä**.
3. Jos olet **läpivienti** **Preauthentication menetelmä**, varmista, että voit valita **HTTPS** **Ulkoisen URL** -malli.
4. Jos olet **Preauthentication menetelmä** **Azure Active Directory** -kohdassa **ei mitään** **Sisäinen todentamismenetelmän**.


## <a name="adfs-configuration"></a>ADFS: N määrittäminen

1. Avaa ADFS hallinta.
2. Siirry **Käyttäisit osapuolen luottaa**, napsauta hiiren kakkospainikkeella sovelluksen julkaistaan sovelluksen välityspalvelimen kanssa, ja valitse sitten **Ominaisuudet**.  
  ![Varmenteen käyttäjän osapuolen luottaa napsauttamalla sovelluksen nimi - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Valitse **WS Federation** **päätepisteet** -välilehden **päätepisteen tyyppi**-kohdassa.
4. Kirjoita **URL-Osoitteen luotettujen** sovelluksen välityspalvelin **Ulkoisen** URL-osoitteeseen kirjoittamasi URL-osoite ja valitse **OK**.  
  ![Lisää päätepisteen - määrittää luotettujen URL-osoite-arvo - näyttökuva](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Katso myös

- [Sovelluksen välityspalvelimen sovellusten julkaiseminen](active-directory-application-proxy-publish.md)
- [Yksi merkki ottaminen käyttöön](active-directory-application-proxy-sso-using-kcd.md)
- [Ilmenevien ongelmien sovelluksen välityspalvelimen ongelmien vianmääritys](active-directory-application-proxy-troubleshoot.md)
- [Ota käyttöön välityspalvelimen sovellusten vuorovaikutuksessa alkuperäisen asiakassovellukset](active-directory-application-proxy-native-client.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)

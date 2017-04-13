<properties 
    pageTitle="Voit korjata SAML-pohjaisen kertakirjautumisen sovellusten Azure Active Directoryn | Microsoft Azure" 
    description="Lue, miten voit korjata SAML-pohjaisen kertakirjautumisen Azure Active Directory-sovellukset " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Voit korjata SAML-pohjaisen kertakirjautumisen Azure Active Directory-sovellukset

Kun virheenkorjaus SAML-pohjaisen sovelluksen integrointi on usein hyödyllistä SAML pyynnön, SAML vastauksen ja todellinen SAML-tunnuksen, joka on myöntänyt sovelluksen [Fiddler](http://www.telerik.com/fiddler) kuten työkalun avulla. Tarkastelemalla SAML-tunnuksen voit varmistaa, että kaikki pakolliset määritteet, SAML aihe ja myöntäjä URI käyttäjänimi on tulossa odotetulla tavalla.

![][1]

Azure AD, joka sisältää SAML-tunnuksen vastaus on yleensä tapahtuu, kun HTTP-302 https://login.windows.net-uudelleenohjauksen ja sovelluksen määritetyn **Vastaa URL-osoite** lähetetään haluamasi vaihtoehto. 
 
Voit tarkastella SAML-tunnuksen rivillä ja valitsemalla sitten **tarkastajien > löytyi** välilehti, valitse oikeassa paneelissa. Sieltä Napsauta **SAMLResponse** arvoa hiiren kakkospainikkeella ja valitse **TextWizard lähettäminen**. Valitse **Valitse Base64** **Transform** -valikosta toistaa tunnuksen ja tarkastella sen sisältöä.
 
**Huomautus**: Tämä HTTP-pyynnön sisällön näkyviin Fiddler saattaa kehottaa sinua HTTPS-liikenne, jotka sinun on suoritettava salauksen määrittäminen.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Kertakirjautumisen sovelluksia, jotka eivät ole Azure Active Directory-sovelluksen valikoiman määrittäminen](active-directory-saas-custom-apps.md)
- [Vaateita, jotka on annettu etukäteen integroidut sovellukset SAML tunnus mukauttamisesta](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png

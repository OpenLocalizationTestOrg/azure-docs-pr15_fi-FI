<properties
pageTitle="Asenna ja määritä työkaluja GitHub yhteiskäyttö"
description="Työkalut ja toimia määritetty authoring GitHub Azure sisällön."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Asenna ja määritä työkaluja GitHub yhteiskäyttö

Noudattamalla tämän artikkelin määrittämään Azure teknisten käytettävät työkalut. Satunnainen ja ajoittaiset osallistujien käyttää todennäköisesti GitHub Käyttöliittymän vaiheen 2 ohjeiden mukaisesti.

Jos et tiedä Git, haluat ehkä tarkastella joitakin Git termejä: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Lisäksi StackOverflow säikeen sisältää sanasto Git termien selaat tämä toimintaohjeiden: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Sisältö

- [Luo GitHub tili ja profiilin määrittäminen](#create-a-github-account-and-set-up-your-profile)
- [Disqus rekisteröityminen](#sign-up-for-disqus)
- [Määrittää, onko todella täytyy noudata näitä ohjeita](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [GitHub käyttöoikeudet](#permissions-in-github)
- [Git Windows-version asentaminen](#install-git-for-windows)
- [Kaksiosainen todentamismenetelmä ottaminen käyttöön](#enable-two-factor-authentication)
- [Asenna korotuksia-editori](#install-a-markdown-editor)
- [Atom määrittäminen](#configure-atom)
- [Haarautuvat säilö ja kopioiminen tietokoneeseen](#fork-the-repository-and-copy-it-to-your-computer)
- [Määritä käyttäjänimi ja sähköpostin paikallisesti](#configure-your-user-name-and-email-locally)
- [Seuraavat vaiheet](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Luo GitHub tili ja profiilin määrittäminen

Osallistumisoikeudet Azure teknistä sisältöä, sinun on [GitHub](http://www.github.com) -tili.

Jos olet Microsoft osallistujan, haluat määrittää GitHub tilin, niin olet selkeästi Microsoft työntekijänä. Määritä profiiliasi seuraavasti:

- **Profiilikuvan**: kuva, jonka (pakollinen)
- **Nimi**: ensimmäisen ja viimeisen nimi (pakollinen)
- **Sähköposti**: (valinnainen) Microsoft-sähköpostiosoitteesi
- **Yrityksen**: Microsoft Corporation (pakollinen)
- **Sijainti**: luettelon sijaintisi (valinnainen)

Oman profiilin pitäisi muistuttaa profiilia:

<p align="center">
 ![GitHub Profiiliesimerkki](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Disqus rekisteröityminen

Jokaisen julkaistun Azure tekniset artikkelin on kommentti stream Disqus-palvelua varten.

 ![Discus-logo](./media/tools-and-setup/discus.png)

Jos olet Microsoftin työntekijöiden ja tekijän tai osallistujan artikkeleihin, sinun täytyy tilaa Disqus, jotta voit osallistua artikkelin kommentti-muodossa.

1. [Http://www.disqus.com/](http://www.disqus.com/) tiliä, Rekisteröidy
2. Täytä profiiliasi seuraavasti:

 - **KokoNimi**: koko nimesi on näkyvissä Microsoft osoite-kirja-luettelon sekä hakasulkeissa tiedot, jossa on tunnus plus @MSFT. Muoto: *Etunimi Sukunimi [alias@MSFT] *
 - **Sijainti**: sijainti
 - **Lyhyt Bio**: otsikko

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Määrittää, onko todella täytyy noudata näitä ohjeita

Voit joutua ei noudata tämän artikkelin ohjeita. Odotusaika riippuu sisällön osuuden haluat tai täytyy tehdä Lajittele.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Lähetä vain teksti-muutos aiemmin artikkeliin

Jos tarvitset vain tai muutosten tekstimuotoinen päivitykset aiemmin artikkeleihin, olet todennäköisesti tarvitse noudattamalla ohjeita. GitHub on verkkopohjainen korotuksia editorin avulla voit lähettää tekemäsi muutokset. Napsauta GitHub on artikkelissa, jota haluat muokata linkkiä:

 ![GitHub Profiiliesimerkki](./media/tools-and-setup/contributetogit.png)

 Valitse Muokkaa-kuvaketta, on artikkelissa GitHub-versiossa

 ![GitHub Profiiliesimerkki](./media/tools-and-setup/editicon.PNG)

 Näkyviin tulee helposti käytettävällä web-editorin, jolla on helppo Lähetä muutokset. Sinun ei tarvitse tehdä muita tämän artikkelin ohjeiden.

###<a name="all-other-changes"></a>Kaikki muut muutokset
GitHub-Käyttöliittymän tue luomista uusia tiedostoja ja vetämällä ja pudottamalla kuvia. Kun työskentelet Käyttöliittymän, haaroja hallinta voi kuitenkin sekava niin Suosittelemme yleensä työkalut asennetaan ja lue komentojen luomisen ja ylläpidon artikkelit. Jos haluat käyttää Käyttöliittymän, lue:

- [Github tiedostojen luominen](https://github.com/blog/1327-creating-files-on-github)
- [Tiedostojen lataaminen oman säilöjen tietoihin](https://github.com/blog/2105-upload-files-to-your-repositories)

Työn seuraavat lajittelee suositeltavaa asentaminen ja työkalujen käyttäminen:

 - Tehdään muutoksia artikkeliin
 - Luominen ja julkaiseminen uuden artikkelin
 - Lisää uusia kuvia tai päivittää kuvat
 - Päivittäminen artikkeliin päivän muuttamatta julkaisun kunkin näiden päivän ajan
 - Julkaisu, jossa on siirtyä tiettyjä päivänä tietyn ajan sisällön luominen

##<a name="permissions-in-github"></a>GitHub käyttöoikeudet

Kuka tahansa käyttäjä, GitHub tilillä voivat osallistua Azure teknistä sisältöä sekä julkinen säilöön osoitteessa [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content)kautta. Tarvitaan erityiset ei ole oikeuksia.

Jos olet Microsoft PM tai writer, kuka on muokkaamassa Azure sisältöä, sinun on työskenneltävä Microsoftin yksityinen sisällön säilöön, azure-sisältö-arvo. Käy [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) pyytää lukuoikeudet, jotta voit tehdä maksujen kautta yksityinen repo - painikkeella GitHub kirjautuminen > Valitse Azure > **Liity työryhmän** tai **liittyä toiseen ryhmän**, valitse Hae ja **azure-sisältö-luku** -ryhmän.

## <a name="install-git-for-windows"></a>Git Windows-version asentaminen

Asenna Windows Git [http://git-scm.com/download/win](http://git-scm.com/download/win). Tämän ladattavan asentaa Git versio käyttöjärjestelmää ja se asennetaan Git Bash komentorivin sovellus, jota käytät Käsittele paikallisen Git-säilön kanssa.

Voit hyväksyä oletusasetukset; Jos haluat komentoja voi käyttää Windows-komentoriviltä sisällä, valitse vaihtoehto, joka on sallinut sen käytön.

<p align="center">
 ![GitHub Profiiliesimerkki](./media/tools-and-setup/gitbashinstall.png)

(Huomautus: Tämä ei ole sama kuin "Github Windows". "Github Windows" on Graafisen perustuva toista työkalua, joka toimii myös, jos haluat itse Lue. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Kaksiosainen todentamismenetelmä ottaminen käyttöön

Sinun on otettava kaksi monimenetelmäinen todentaminen (2FA) GitHub tililläsi Jos käsittelet yksityinen sisällön säilössä. Yksityinen säilöön vaaditaan sitä.

Voit ottaa tämän noudata sekä seuraavissa GitHub ohjeaiheissa:

- [Tietoja kaksiosainen todentamismenetelmä](https://help.github.com/articles/about-two-factor-authentication/)
- [Access-tunnuksen komentorivin käytettäväksi luominen](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Kun luot tunnuksen, valitse kaikki alueet, jotka ovat käytettävissä tunnuksen luominen käyttöliittymässä ([kunkin alueen tiedot](https://developer.github.com/v3/oauth/#scopes))

Kun olet ottanut käyttöön 2FA, sinun on Anna GitHub salasana komentokehotteen sijaan käyttöoikeustietue, kun yrität käyttää GitHub säilön komentoriviltä. Access-tunnuksen ei ole todennus-tunnus, jolla saat tekstiviestillä, kun olet määrittänyt 2FA. Se on pitkä merkkijono, joka näyttää seuraavankaltaiselta: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Muutama huomautuksia näin:

- Kun luot access-tunnuksen, Tallenna asiakirja tekstitiedostoon, voit tehdä sen helposti tarvittaessa.

- Myöhemmin, kun haluat liittää tunnuksen, tiedät Liitä komentorivillä kahdella tavalla:

 - Napsauta komentorivi-ikkunan vasemmassa yläkulmassa olevaa kuvaketta > muokkaaminen > Liitä.
 - Ikkunan vasemmassa yläkulmassa olevaa kuvaketta hiiren kakkospainikkeella ja valitse ominaisuudet > Asetukset > Pikamuokkaus-tilassa. Tämä määrittää komentorivin, jotta voit liittää tiedot komentorivi-ikkunassa hiiren kakkospainikkeella.

## <a name="install-a-markdown-editor"></a>Asenna korotuksia-editori

Olemme author-sisällön liittämiseen yksinkertainen "korotuksia" merkintätapaa tiedostoissa mieluummin kuin kompleksiluvun "merkintä" (esimerkiksi HTML, XML). Näin on tarvitset korotuksia Editorin asentaminen.

- **Atom**: Useimmat us GitHub's Atom korotuksia editorin käyttäminen: [http://atom.io](http://atom.io). Se ei edellytä käyttöoikeuden yrityskäyttöön. Oikeinkirjoituksen tarkistus on.

- **Muistion**: Voit käyttää muistio hyvin kevyt vaihtoehto.

- **Prose**: Tämä on kevyt, klassinen, online ja Avaa lähde korotuksia-editori, joka tarjoaa esikatselu. Käy [http://prose.io](http://prose.io) ja määritä Prose säilöön, että.

- **[Visual Studio koodi](https://www.visualstudio.com/products/code-vs.aspx)** - Microsoftin tapahtuma tähän.

## <a name="configure-atom"></a>Atom määrittäminen

Jos käytät Atom, tarvitset määrittämään muutama seikka.

- Atom oletusarvo on 2 tilat käyttämällä välilehtiä, mutta korotuksia odottaa 4 välilyöntejä. Jos jätät kaksi oletusarvo-artikkelin näyttävät hyviltä paikallisen esikatselussa, mutta ei milloin se tuodaan Azure. Määrittäminen niin, käyttää 4 välilyöntejä Atom - löydät asetusta Valitse Tiedosto > Asetukset > Editorin asetukset > välilehti pituus.
- Sinun on todennäköisesti myös haluat ottaa käyttöön Pehmeä Rivitä sisältö, joka tekee saman nimellä "rivityksen" Muistiossa.
- Voit ottaa käyttöön korotuksia esikatselu valitsemalla pakettien > korotuksia esikatselu > Näytä tai piilota esikatselu. Voit käyttää Ctrl-VAIHTO-M Vaihda esikatselu HTML-näkymä.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Haarautuvat säilö ja kopioiminen tietokoneeseen

1. Rinnakkainen tietovaraston luominen GitHub - Siirry sivun oikeaan yläreunaan ja Haarautumissiirtymä-painiketta. Valitse pyydettäessä tilin sijainti, johon rinnakkainen luodaan. Tämä luo kopion Git toiminnossa tilisi säilö. Niinkään tekniset kirjoittajat ja ohjelma valvojat täytyy rinnakkainen azure-sisältö-arvo, yksityinen repo. Yhteisöosallistujat on rinnakkainen azure-sisältöä, julkisen repo. Tarvitset vain kerran, haarautuvat palvelun ensimmäisen määrityskerran jälkeen voit halutessasi oman rinnakkainen kopioiminen toiseen tietokoneeseen, vain sinulla suorittaa komentoja, joita noudattamalla voit kopioida repo tietokoneeseen.  Jos haluat luoda molemmat säilöjen tietoihin haarukat, sinun on rinnakkainen kunkin säilöön, luomiseen.

2. Kopioi Omat Access tunnuksen, joka on saatu [https://github.com/settings/tokens](https://github.com/settings/tokens). Voit hyväksyä tunnuksen oletusoikeudet.  Tallenna omat Access-tunnuksen tekstitiedostoon tulevaa käyttöä varten.

3. Kopioi säilön tietokoneen kanssa upotettu komento merkkijonon tunnistetiedot.  Avaa Git Bash ja Suorita järjestelmänvalvojana. Komentokehotteen Kirjoita seuraava komento.  Tämä komento luo azure-content(-pr) kansion tietokoneessa.  Jos käytät oletussijainti, se on osoitteessa c:\users<your Windows user name>\azure-content(-pr).

Julkinen repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Yksityinen repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Esimerkiksi Kloonaa komento voi näyttää suunnilleen tältä:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Aseta remote Tietuesäilön yhteys ja määritä tunnistetiedot

Luo viittaus päämyöntäjien säilöön kirjoittamalla näitä komentoja. Tämä määrittää yhteydet-GitHub säilöön niin, että voit saat uusimmat muutokset paikalliseen tietokoneeseen ja siirtää muutokset takaisin GitHub. Tämä komento määrittää oman tunnuksen myös paikallisesti niin, että sinun ei tarvitse kirjoittaa käyttäjänimi ja salasana aina, kun yrität käyttää ylöspäin repo ja että haarautumissiirtymä-GitHub.

Julkinen repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Yksityinen repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Tämä yleensä kestää jonkin aikaa. Kun teet näin, sinun ei tarvitse haarautuvat uudelleen ja anna tunnistetiedot uudelleen. Voit vain voi kopioida paikallisesti tietokoneeseen haarukat uudelleen Jos määrität työkaluja toisessa tietokoneessa.


## <a name="configure-your-user-name-and-email-locally"></a>Määritä käyttäjänimi ja sähköpostin paikallisesti

Jotta näkyvät oikein osallistujana haluat määrittää käyttäjänimi ja sähköpostiosoite paikallisesti Git.

1. Käynnistä Git Bash ja siirry azure sisältöä tai azure-sisältö-arvo:

   ````
   cd azure-content
   ````

 tai

   ````
   cd azure-content-pr
   ````

2. Määritä käyttäjänimi, jotta se vastaa nimesi, kun olet määrittänyt sen GitHub profiiliin:

    ````
    git config --global user.name "John Doe"
    ````
3. Määritä sähköpostin, jotta se vastaa sovelluksen GitHub; profiiliin ensisijainen sähköposti Jos ole MSFT työntekijän, sen pitäisi olla MSFT sähköpostiosoite:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Kirjoita `git config -l` ja tarkista paikallisen varmistamiseksi käyttäjänimi ja sähköpostin määritykset ovat oikein.

##<a name="next-steps"></a>Seuraavat vaiheet

- Tietoja, joihin kuuluu teknisen sisällön repo sisällön tyypin ja tietää, mitä ei kuulu. Lisätietoja [sisällön kanavan ohjeet](./content-channel-guidance.md)!
- [Voit luoda tai muokata artikkelia ja lähetä se julkaisun](./git-commands-for-master.md)seuraavasti.
- Kopioi [korotuksia mallin](../markdown templates/markdown-template-for-new-articles.md) uuden artikkelin perustana.
- Käytä [tätä tarkistusluetteloa vahvistamiseksi salaus puretaan pyyntö täyttävät laatu](./contributor-guide-pr-criteria.md) yhdistämistä varten.


###<a name="contributors-guide-navigation"></a>Osallistujat-opas siirtyminen

- [Yleistä artikkelissa](./../README.md)
- [Indeksi ohjeet artikkeleista](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps

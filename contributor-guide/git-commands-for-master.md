<properties pageTitle="Uuden artikkelin luominen tai päivittäminen aiemmin artikkelissa Git komennot" description="Tekniset Azure käyttämisen vaiheet sisällön GitHub säilöjen tietoihin." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Uuden artikkelin luominen tai päivittäminen aiemmin artikkelissa Git komennot


## <a name="standard-process-working-from-master"></a>Prosessin (toimi perustyylin)
Voit luoda paikallisen toimimasta haaran tietokoneen niin, että voit luoda uuden artikkelin azure.microsoft.com teknisten osan tai päivittää aiemmin artikkelissa tämän artikkelin ohjeiden.

1. Käynnistä Git Bash (tai komentorivin työkalu, jonka avulla saat Git).

 **Huomautus:** Jos käsittelet julkisen säilössä, siirry azure-sisällön kaikki komennot azure-sisältö-arvo.

2. Siirry azure-sisältö-arvo:

        cd azure-content-pr
3. Tutustu perusmuodon haaran:

        git checkout master

4. Luoda ajan tasalla paikallisen toimimasta haaran johdetun perusmuodon päätasolta:

        git pull upstream master:<working branch>


5. Siirrä uusi toimimasta haaran:

        git checkout <working branch>

6. Varmista, että tiedät luomasi paikallisen toimimasta haaran rinnakkainen:

        git push origin <working branch>

7. Luo uusi artikkeli tai muokata aiemmin artikkelissa. Avaa ja luoda uusia korotuksia tiedostoja käyttämällä Atom (http://atom.io) korotuksia editorina Windowsin Resurssienhallinnan avulla. Kun luodun tai muokatun artikkelissa ja kuvia, siirry seuraavaan vaiheeseen.

8. Lisää ja Vahvista tekemäsi muutokset:

        git add .
        git commit –m "<comment>"
        
   Vaihtoehtoisesti voit lisätä vain tietyt tiedostoja voit muokata:

        git add <file path>
        git commit –m "<comment>"

   Jos poistit tiedostoja, sinun on käytettävä näin:
   
        git add --all
        git commit -m "<comment>"

9. Päivittää paikallisen toimimasta-haara seuraavat muutokset:

        git pull upstream master

10. Siirtää muutokset oman haarautumissiirtymä-GitHub:

        git push origin <working branch>

12. Kun olet valmis lähettämään väliaikaisen kelpoisuuden tarkistaminen-ja/tai jaettu julkaisuoikeus, seuraavat perusmuodon haara sisällön GitHub käyttöliittymässä luominen salaus puretaan pyyntö oman rinnakkainen perusmuodon haaraa.

13. Jos olet työntekijän työskentelyä yksityinen säilöön muutoksia, voit lähettää automaattisesti vaiheistettu ja väliaikaisen linkin kirjoitetaan salaus puretaan pyynnön. Vaiheistettu sisällön tarkastelu ja kirjaudu salaus puretaan pyynnön kommenttien, lisäämällä **#sign käytöstä** kommentti.  Tämä ilmaisee muutokset ovat valmiit, miten live.  Jos et halua salaus puretaan pyynnön hyväksymisen -, jos ovat vain väliaikaisen muutoksia - Lisää **#hold käytöstä** Huomautus salaus puretaan pyynnön.

14. Salaus puretaan pyynnön acceptor tarkistaa salaus puretaan pyynnön, antaa palautetta ja/tai hyväksyy pyynnön salaus puretaan. 

15. Voit myös tarkistaa julkaistu artikkeli tai muutoksia

 http://Azure.microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Julkaiseminen

- Artikkelit julkaistaan noin 10:00 ja 3:00 PM aikaa, ma-pe. Saattaa kestää enimmillään 30 minuuttia artikkeleita näkyvän online julkaisemisen jälkeen. Muista yhdistettäviä salaus puretaan yhden tarkistajan, ennen kuin muutokset voidaan lisätä suorittamalla seuraava ajoitettu julkaisu on erotettu pyynnön. Haluat käyttää omaa salaus puretaan pyynnön tarkistajan etuajassa varmistamiseksi salaus puretaan pyyntö on yhdistetty tietyn julkaisun, suorita. Muussa tapauksessa laatuviiniä tarkistetaan ne lähetettiin järjestyksessä.

- Jos olet työntekijän yksityinen säilössä, kaikki salaus puretaan pyynnöt käsitellään kelpoisuussääntöjä, ennen kuin salaus puretaan pyynnön voidaan yhdistää pystyttävä vastaamaan. 

## <a name="working-with-release-branches"></a>Vapauta haaroja käsitteleminen

Kun käsittelet release haaran, paras tapa luoda paikallisen toimimasta haaran release päätasolta on komento seuraavalla syntaksilla:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Tämä luo paikalliset haaran suoraan päätasolta ylöspäin välttäminen minkä tahansa paikallisen yhdistäminen.


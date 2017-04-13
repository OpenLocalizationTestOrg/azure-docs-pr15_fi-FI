Jatkuvaa kehitystä, koska Android SDK-versio asennettu Android Studiossa eivät ehkä vastaa koodissa versio. Tässä opetusohjelmassa Viitattu Android SDK on versio 23 uusimmat kirjoittaminen aikaa. Versionumero saattaa kasvaa SDK uusimmat näkyviin, ja suosittelemme, että käytät käytettävissä uusimpaan versioon.

Kahden Versioristiriita oireita ovat seuraavat:

1. Kun luot tai projektin uudelleen, näyttöön voi tulla Gradle virhesanomista, kuten "**ei löydy kohde Google Inc.:Google APIs:n**".

2. Vakio-koodi, joka korjaa Android objektien perusteella `import` lauseet luodaan virhesanomia.

Jos jompikumpi näistä, versio on asennettu Android Studiossa Android SDK eivät ehkä vastaa ladatut projektin SDK kohde.  Voit tarkistaa versio, tee seuraavat muutokset:


1. Android Studiossa, valitse **Työkalut** => **Android** => **SDK hallinta**. Jos et ole asentanut uusimman version SDK-ympäristö, valitse Asenna. Pane merkille versionumero.

2. Avaa tiedosto Project Explorer-välilehden **Gradle komentosarjojen** **build.gradle (modeule: app)**. Varmista, että **compileSdkVersion** ja **buildToolsVersion** määritetään asennettu SDK uusin. Tunnisteet voi näyttää tältä:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Android Studio Project Explorer project-solmu, valitse **Ominaisuudet**hiiren kakkospainikkeella ja valitse vasemmassa sarakkeessa **Android**. Varmista, että **Projektin luominen kohde** on määritetty **targetSdkVersion**SDK versiota.

4. Android Studio luettelon tiedosto ei ole enää käytössä kohde SDK ja vähintään SDK-versio, toisin kuin Pimennys tapauksessa määrittämiseen.

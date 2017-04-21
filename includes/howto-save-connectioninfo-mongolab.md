Kun se on mahdollista MongoLab URI Liitä koodi, on suositeltavaa määrittäminen hallinnan helpottamiseksi ympäristössä. Tällä tavalla, jos URI muutoksia, voit päivittää sen palvelun Azure-portaalissa siirtymättä koodi.


1. Valitse Azure-portaalissa **Verkkosovelluksissa**.
1. Napsauta nimeä web Appin Web Apps-sovellusten luettelossa.  
![WebAppEntry][entry-website]  
Web App-koontinäytön näyttää.

1. Valitse **Määritä** valikkorivillä.  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Vieritä yhteysmerkkijonon-osaan.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Kirjoita **nimi**-MONGOLAB_URI.
1. Liitä **arvo**on saatu edellisessä osassa yhteysmerkkijonon.
1. Valitse **Mukautettu** **tyyppi** avattavasta luettelosta (sijaan oletusarvo **SQLAzure**).
1. Valitse työkaluriviltä **Tallenna** .  
![SaveWebApp][button-website-save]

**Huomautus:** Lisää Azure **CUSTOMCONNSTR\_ ** etuliite muuttuja, joiden vuoksi edellä viittaukset koodin **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png

# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Toimet, joiden Keskeytä tai teknisiä ACOM-artikkelissa nimen muuttaminen

Nämä ohjeet on pk, jotka on lueteltu artikkeliin, jossa on azure.microsoft.com teknisten kenttäosasta poistua tekijä. Vaiheet koskevat myös, jos tiedosto on nimetty uudelleen.

Jos arvelet, että artikkeliin pitäisi poistua jostakin syystä Azure yhteisön jäsen olet, jätä kommentin, jos haluat sallia tietää jotakin vikaa artikkelin tekijä on artikkelissa Disqus kommentti-muodossa.

PK tekijät suoritettava useita vaiheita Keskeytä sisällön tilanteen, jotta sivuston käyttäjien ole virheellinen kokemus Microsoft Keskeytä sisältö-sivustosta. Poistamisesta on artikkelissa tai muuttamalla sen nimen on oltava viimeiseksi, tapahtuu!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Vaihe 1: Arvoksi ei-hakemiston/ei-seuraa artikkelin ja julkaista sen (suositus)

Huomaa, toimi ei julkaista kuin ei-hakemiston/ei-seuraa artikkelin muutaman viikon, ennen kuin poistat sen itse asiassa. Tämä käsitellään paras käytäntö "edeltävä työ" poistamisesta sisällön. Tämä poistaa on artikkelissa ohjelma hakuindeksin että henkilöt eivät saada on artikkelissa haku. [Katso lisätietoja sisäinen wiki.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Vaihe 2: Hallita saapuvia linkkejä (pakollinen)

Määrittää, onko-Microsoft saapuvaa linkit sisältöön. Usein blogit, keskustelupalstat ja muuta sisältöä verkossa viittaa artikkelit. Usein voit käsitellä blogin omistajat voivat muuttaa näitä linkkejä ja voit poistaa tai päivittää linkit keskustelupalstalla viestit. Web analytics-Työkalut toimitusongelmaraportin ole mitään suuri tietoliikenne saapuvien linkkejä voit joutua muuttamaan hallinta tällä tavalla.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Vaihe 3: Poistaa kaikki crosslinks on artikkelissa teknisen sisällön tietovarasto (pakollinen)

Huolehtia crosslinks muita artikkeleita uudelleenohjausta ei riippuvaisia. Päivitä tai poista sitten ristiä viittaukset on artikkelissa ovat poistaminen tai nimeäminen uudelleen, mukaan lukien artikkeleissa omistaa muille.

1. Varmista työskentelyn ajan tasalla paikallisen haaran – Suorita `git pull upstream master` (tai tämän komennon tarvittavat muutokset.

2.  Skannaa azure-sisältö-hinta/artikkelit-kansioon ja azure-sisältö-Hinta/sisältää kansio mille tahansa artikkelit ja sisältää kyseisen linkin haluat Keskeytä artikkelin ja joko Poista crosslinks tai niiden tilalle haluamasi uusi crosslinks. Voit käyttää haun ja korvata apuohjelmaa löydät crosslinks, jos käytössäsi on jokin asennettuna. Jos et, voit käyttää Windows PowerShellin maksutta! Näin voit etsiä crosslinks PowerShellin avulla:

 a. Käynnistä Windows PowerShell.

 b. PowerShell-kehotteessa muuttaa azure-sisältö-pr\articles kansioon seuraavasti:

 `cd azure-content-pr\articles`

 c-näppäinyhdistelmää. Kirjoita seuraava komento, joka sisältää luettelon kaikki tiedostot, joissa haluat poistaa artikkelin viittaus:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Jos haluat lähettää tiedostonimien luettelossa tekstitiedostoon (tämä kirjainkoon, nimetyn psoutput.txt), voit:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Lisää kaikki tekemäsi muutokset, push-että rinnakkainen ja luoda salaus puretaan kokouspyynnön voit siirtää muutokset oman rinnakkainen tärkeimmät säilöön perusmuodon haara.

## <a name="step-4-update-the-fwlink-tool-required"></a>Vaihe 4: Päivitä FW-linkki-työkalu (pakollinen)

Tarkista FWLinks, joka ehkä osoittamalla artikkelin FW-linkki-työkalu. Osoita mitä tahansa FWLinks korvaavan sisällön; Jos et ole alias, joka omistaa linkkiä, yhdistä se. Jos omistajat ei päivitetä linkkiä, tiedoston lippu, linkki, muuttaa MSCOM kanssa. Lisätietoja - [sisäinen wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Vaihe 5: Poistaa kaikki crosslinks on artikkelissa muiden sivujen azure.microsoft.com ja luo uudelleenohjaus käytöstä poistettuja sivulle sopiva (tarvittaessa)

Sinun on henkilö, joka ylläpitää ja päivittää asiakirjat-aloitussivu, tämän osan palvelun käyttöä varten. Ota sisällön ryhmän kumppanin Jos et ole varma, että henkilö on. Henkilö, joka ylläpitää ja päivittää doc-aloitussivu on tehtävä kaksi asiaa:

1. Visual Studiossa Tarkista **koko** ACOM web ratkaisu viittaukset tiedoston Keskeytä. Poista ristiviittauksia tai niiden tilalle päivitetyt ristiviittaus. Sinun on poistettava HTML-linkkejä sekä liittyvät resurssin merkkijonojen HTML-linkkejä. Lisätietoja - kohdassa [sisäinen wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Jos korvaava-artikkelissa on olemassa, Luo uudelleenohjaus. Lisätietoja - kohdassa [sisäinen wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Tarkista muutokset säilö.

## <a name="step-6-retire-the-article"></a>Vaihe 6: Poista käytöstä on artikkelissa

Kun olet suorittanut edellisen vaiheet ja muutokset ovat live, voit poistaa artikkelin säilöstä. 

**Tärkeä:** Kun poistat tiedostoja, sinun on käytettävä `git add --all` komento.

## <a name="step-7-remove-links-from-msdn-required"></a>Vaihe 7: Poista linkit MSDN (pakollinen)

Tarkista katkenneet linkit käytöstä poistettuja tai nimetty uudelleen ohjeaiheen sisältö kysymysten ja vastausten työkalua ja poista/korjaus linkit vaikuttaa kaikki MSDN-aiheet.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Vaihe 8: Poista välimuistiin tallennetut sivut hakukoneet (vain, jos välttämättömiä)

Tee tämä vain, jos sisältö on poistettava nopeasti oikeudelliset tai vakavia asiakkaiden ongelmien vuoksi. Toimintaohjeita googlesta kohti Normaali prioriteetti sivun poistot vain on käsiteltävä luonnollinen Etsi ohjelma prosessit. Siirry nämä verkkosivut Poista välimuistiin tallennetut verkkosivujen hakukoneiden ulkopuolelle:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Osallistujat-opas linkit

- [Yleistä artikkelissa](./../README.md)
- [Indeksi ohjeet artikkeleista](./contributor-guide-index.md)

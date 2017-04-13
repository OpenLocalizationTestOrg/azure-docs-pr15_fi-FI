<properties
   pageTitle="Azure AD Connect: Versiohistoria Release | Microsoft Azure"
   description="Tässä artikkelissa on lueteltu kaikki versioiden Azure AD Connect ja Azure AD-synkronointi"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Release versiohistoria

Azure Active Directory-ryhmän päivittää säännöllisesti Azure AD Connect uusista ominaisuuksista ja toiminnoista. Kaikki lisäykset eivät koske kaikkia käyttäjäryhmien.

Tässä artikkelissa on suunniteltu avulla voit seurata versioita, jotka on julkaistu ja selvittääksesi, onko sinun on päivitettävä uusimmat vai ei.

Tämä on aiheeseen liittyvien aiheiden luettelo:

Aihe |  
--------- | --------- |
Voit päivittää Azure AD Connect vaiheet | [Päivittäminen aiemmasta versiosta uusimpien](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect versioon muilla tavoilla.
Tarvittavat käyttöoikeudet | Katso päivittäminen tarvittavat käyttöoikeudet, [Asiakkaat ja käyttöoikeudet](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Lataa| [Lataa Azure AD-muodosta](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Julkaissut: 2016 elokuussa

**Kiinteä ongelmat:**

- Muutosten synkronointi väli ei tapahdu vasta seuraavan synkronoinnin jakson päätyttyä.
- Azure AD Connect ohjattu toiminto ei hyväksy Azure AD-tilin käyttäjänimi, joiden alussa on alaviiva (\_).
- Azure AD Connect ohjattu epäonnistuu tarkistamiseen Azure AD-tili, jos salasana on liian monta erikoismerkkejä. Virhesanoma "ei voi vahvistaa tunnistetiedot. Odottamaton virhe on tapahtunut." palautetaan.
- Asennuksen poistaminen väliaikaisen palvelimen poistaa salasanan synkronointi Azure AD-vuokraajan ja aiheuttaa salasanan synkronointi voi epäonnistua aktiivinen-palvelimen kanssa.
- Salasanan synkronointi epäonnistuu epätavallisten tapauksissa, kun kyseessä on tallennettu käyttäjän salasana ei ole hash.
- Kun väliaikaisen tila on käytössä Azure AD Connect palvelimeen, salasana takaisinkirjoitusta ei käytöstä tilapäisesti.
- Azure AD Connect ohjattu toiminto ei näy todellinen salasanojen synkronoinnin ja salasanan takaisinkirjoituksen määritysten kun palvelin ei väliaikaisen tila. Se näyttää aina ne poistettuna.
- Määritysmuutoksia salasanojen synkronoinnin ja salasanan takaisinkirjoitusta ei säilytetä Azure AD Connect ohjatussa, kun palvelin ei väliaikaisen tilassa.

**Parannukset:**

- Päivitetty Käynnistä ADSyncSyncCycle cmdlet-komento, Määritä, onko käynnistää uuden synkronoinnin jakson onnistuneesti, vai ei.
- Lisätyn sarkainkohdan ADSyncSyncCycle cmdlet-komento lopettaa synkronoinnin kehä ja toiminta on parhaillaan käynnissä.
- Päivitetty Stop ADSyncScheduler cmdlet-komento lopettaa synkronoinnin kehä ja toiminta on parhaillaan käynnissä.
- Määritettäessä [Directory tunnisteet](active-directory-aadconnectsync-feature-directory-extensions.md) Azure AD Connect ohjatussa AD-määrite lajin "Teletex merkkijonon" nyt olla valittuna.

## <a name="111890"></a>1.1.189.0
Julkaistu: 2016 kesäkuussa

**Kiinteä ongelmat ja parannukset:**

- Azure AD Connect voidaan asentaa nyt FIPS-yhteensopiva palvelimessa.
    - Lisätietoja salasanojen synkronoinnin [salasanan synkronointi ja FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Kiinteä ongelma, jossa NetBIOS nimi ei onnistunut, Active Directory Connectorin FQDN.

## <a name="111800"></a>1.1.180.0
Julkaistu: 2016 toukokuussa

**Uudet ominaisuudet:**

- Varoittaa ja auttaa tarkistetaan toimialueet, jos se ei tee ennen kuin suoritat Azure AD Connect.
- Tukee nyt [Microsoftin Saksa](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Tukee nyt uusin [Microsoft Azure Government-pilvi](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) -infrastruktuuria uusi URL-Osoitteen vaatimusten kanssa.

**Kiinteä asentamisongelmat ja parannukset:**

- Lisätä suodattaminen synkronointi sääntö-editorin helposti löydät synkronointi säännöt.
- Parannettu suorituskyky poistettaessa yhdistimen välilyönti.
- Kiinteä ongelmia, kun samaan objektiin on sekä poistetaan ja lisätään saman suorittaminen (eli Poista/Lisää).
- Säännön käytöstä synkronointi ei enää uudelleen käyttöön sisällyttää objektien ja määritteiden päivityksen tai kansion rakenteeseen Päivitä.

## <a name="111300"></a>1.1.130.0
Julkaistu: 2016: n huhtikuun

**Uudet ominaisuudet:**

- Tukee nyt [Directory tunnisteet](active-directory-aadconnectsync-feature-directory-extensions.md)moniarvoinen määritteet.
- [Automaattisen päivityksen](active-directory-aadconnect-feature-automatic-upgrade.md) pidetään oikeutettu päivitykseen määritysten variaatioita lisätty tuki.
- Lisätä [mukautetun ajoituksen](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)joitakin cmdlet-komennot.

## <a name="111190"></a>1.1.119.0
Julkaistu: 2016 maaliskuussa

**Kiinteä ongelmat:**

- Tehty että Express asentaminen ei voi käyttää Windows Server 2008: n (R2 vuotta) jälkeen salasanan synkronointi ei tueta tässä käyttöjärjestelmässä.
- Päivitystä DirSync mukautetun suodattimen määritys ei ollut odotetulla tavalla.
- Kun päivität uudempaan versioon ja muutoksia työnkulkuun ei ole täydellinen tuominen ja synkronointi ei voida ajoittaa.

## <a name="111100"></a>1.1.110.0
Julkaistu: 2016 helmikuussa

**Kiinteä ongelmat:**

- Aiempien versioiden päivittäminen ei toimi, jos asennus ei ole oletusarvoisesti **C:\Program Files** -kansion.
- Jos asennat ja poista **aloittaa synkronoinnin prosessin …** Ohjattu asennus loppuun suorittamalla ohjattu asennus uudelleen kokoustyötilaa voi ajoitus.
- Scheduler ei toimi odotetulla tavalla, jossa päivämäärä ja kellonaika-muoto ei ole fi-fi-palvelimiin. Estää myös `Get-ADSyncScheduler` palauttaa oikean kertaa.
- Jos olet asentanut Azure AD Connect aiemman version ADFS-kirjautuminen vaihtoehto ja päivitys, et voi suorittaa ohjattu asennus uudelleen.

## <a name="111050"></a>1.1.105.0
Julkaistu: 2016 helmikuussa

**Uudet ominaisuudet:**

- [Automaattinen päivitys](active-directory-aadconnect-feature-automatic-upgrade.md) -toiminto Expressin asetukset-asiakkaille.
- MFA- ja PIM käyttäminen ohjattu asennus yleisen järjestelmänvalvojan tuki.
    - Sinun on sallittava sallimaan-liikenne paikalliseen https://secure.aadcdn.microsoftonline-p.com myös, jos käytät MFA välityspalvelimen.
    - Sinun on lisättävä https://secure.aadcdn.microsoftonline-p.com luotettujen sivustojen luetteloon, MFA toimi oikein.
- Salli käyttäjän kirjautuminen menetelmä muuttamista alkuperäisen asennuksen jälkeen.
- Salli [toimialueen ja OU suodattaminen](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) ohjatun asennuksen. Näin myös yhteyden metsien, joissa kaikki toimialueet eivät ole käytettävissä.
- [Ajoitus](active-directory-aadconnectsync-feature-scheduler.md) on valmiita synkronointi-ohjelma.

**Ylemmän tason esikatselu GA seuraavat ominaisuudet:**

- [Laitteen takaisinkirjoituksen](active-directory-aadconnect-feature-device-writeback.md).
- [Hakemiston tunnisteet](active-directory-aadconnectsync-feature-directory-extensions.md).

**Preview'n uudet ominaisuudet:**

- Uusi oletusmuoto Synkronoi jakson väli on 30 minuuttia. Kaikki aikaisempien versioiden 3 tuntia voi käyttää. Lisää tuen muuta [ajoitus](active-directory-aadconnectsync-feature-scheduler.md) -asetuksia.

**Kiinteä ongelmat:**

- Tarkista DNS toimialueet-sivulla ei aina tunnista toimialueet.
- Toimialueen järjestelmänvalvojan tunnistetietoja ADFS määritettäessä kehote.
- Paikallisen AD-tilejä ei tunnisteta ohjatun asennuksen avulla, jos eri kuin päätoimialueen DNS puun toimialueen sijaitsevat.

## <a name="1091310"></a>1.0.9131.0
Julkaissut: 2015 joulukuussa

**Kiinteä ongelmat:**

- Salasanan synkronointi ei ehkä toimi, kun muutat salasanat AD DS: ÄÄN, mutta toimii, kun määrität salasanan.
- Välityspalvelimen ollessasi Azure AD-todennus saattaa epäonnistua asennuksen tai poistaa aikana päivityksen määritys-sivulla.
- Edellinen versio Azure AD Connect täydellinen ja päivittää SQL Server epäonnistuu, jos et ole SA SQL.
- Edellinen versio Azure AD Connect kaukosäädintä ja päivittää SQL Server näkyy virhe "ADSync SQL-tietokannan muodostaminen".

## <a name="1091250"></a>1.0.9125.0
Julkaissut: 2015 marraskuun

**Uudet ominaisuudet:**

- Voit määrittää ADFS Azure AD-luottamus uudelleen.
- Voit päivittää Active Directory-rakenne ja luo synkronointi säännöt.
- Voit poistaa käytöstä synkronointi säännön.
- Voit määrittää "AuthoritativeNull" uusi literaalimerkit synkronointi säännön.

**Preview'n uudet ominaisuudet:**

- [Azure AD yhteyden kunto-synkronointia varten](active-directory-aadconnect-health-sync.md).
- [Azure AD-toimialueen palveluista](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) salasanojen synkronoinnin tuki.

**Uudet tuetut skenaario:**

- Tukee useita paikallisen Exchange organisaatioita. Lisätietoja on kohdassa [yhdistelmäympäristöä useiden Active Directory-puuryhmien kanssa](https://technet.microsoft.com/library/jj873754.aspx) .

**Kiinteä ongelmat:**

- Salasanan synkronointiongelmien:
    - Laajuus ohjelma siirtyi laajuus-objekti ei ole synkronoitu salasana. Tämä incudes OU ja määritettä suodatus.
    - Valitsemalla Uusi OU sisältämään synkronoinnissa ei edellytä koko salasanan synkronointi.
    - Kun käytöstä käyttäjä on otettu käyttöön salasana ei synkronoida.
    - Salasana uudelleen-jonossa on äärettömän ja 5 000 objekteja, jos haluat poistua edellisen enimmäismäärä on poistettu.
    - [Improved vianmääritys](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Ei pysty muodostamaan yhteyttä Active Directory ja Windows Server 2016 metsää toimintojen taso.
- Ei voi muuttaa käytetään ryhmän suodattaminen alkuperäisen asennuksen jälkeen.
- Uuden käyttäjäprofiilin luominen enää luo kaikille käyttäjille, jotka tekevät salasana Muuta salasana takaisinkirjoituksen käytössä ja Azure AD Connect-palvelimeen.
- Ei voi käyttää pitkä kokonaisluku arvot synkronoinnissa säännöt alueet.
- "Laitteen takaisinkirjoituksen"-valintaruutu on poissa käytöstä, jos ei ole käytettävissä toimialueen ohjaimet.

## <a name="1086670"></a>1.0.8667.0
Julkaissut: 2015 elokuussa

**Uudet ominaisuudet:**

- Ohjattu asennus Azure AD Connect on nyt lokalisoitu Windows Server kaikilla kielillä.
- Tukee nyt tilin lukituksen, kun käytät Azure AD salasanojen hallinta.

**Kiinteä ongelmat:**

- Azure AD Connect asennukseen kaatuu, jos toinen käyttäjä ei häviä asennuksen aloittanut asennuksen ensin henkilö sijaan.
- Jos poistettava puhtaasti Azure AD Connect synkronointi epäonnistuu Azure AD Connect Edellinen asennuksen, ei ole mahdollista uudelleen.
- Azure AD Connect käyttämällä Express-asennus, jos käyttäjä ei ole metsää päätoimialue tai jos käytössä on Active Directory kuin englanninkielisessä versiossa ei voi asentaa.
- Jos Active Directory-käyttäjätili FQDN ei voi ratkaista, harhaanjohtavia virhesanoma "Ei voi vahvistaa rakenteen" näkyy.
- Jos tili, jota käytetään Active Directory-yhdistimen muutetaan ohjatun toiminnon ulkopuolella, ohjattu toiminto ei onnistu myöhemmin suoritetaan.
- Azure AD Connect lakkaa joskus toimialueen ohjauskoneen asentaminen.
- Ei voi ottaa käyttöön ja "Väliaikaisen tilan" poistaminen käytöstä, jos tunniste määritteet on lisätty.
- Salasanan takaisinkirjoituksen epäonnistuu joidenkin määrityksessä Active Directory Connectorin virheellisen salasanan vuoksi.
- DirSync ei voi päivittää, jos dn määrite suodatus on käytössä.
- Ylimääräisen suorittimen käyttö, kun käytät salasanan.

**Poistetun esikatselu seuraavat ominaisuudet:**

- Esikatselu-toimintoa [käyttäjän takaisinkirjoituksen](active-directory-aadconnect-feature-preview.md#user-writeback) tilapäisesti poistaa esikatselu asiakkaat palautetta. Se uudelleen lisätään myöhemmin, kun Microsoft on korjannut annettu palautetta.

## <a name="1086410"></a>1.0.8641.0
Julkaistu: 2015 kesäkuussa

**Azure AD Connect alkuperäisessä versiossa.**

Azure AD Connect Azure AD-synkronointi-muutetut nimi.

**Uudet ominaisuudet:**

- Asennuksen [Expressin asetukset](./connect/active-directory-aadconnect-get-started-express.md)
- Voit [määrittää ADFS](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Voit [päivittää DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Estä vahingossa poistot](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- [Väliaikaisen tila](active-directory-aadconnectsync-operations.md#staging-mode) käyttöön

**Preview'n uudet ominaisuudet:**

- [Käyttäjän takaisinkirjoituksen](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Ryhmän takaisinkirjoituksen](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Laitteen takaisinkirjoituksen](active-directory-aadconnect-feature-device-writeback.md)
- [Hakemisto-laajennukset](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Julkaistu: 2015 toukokuussa

**Uusi vaatimus:**

- Azure AD-synkronointi edellyttää .net framework-versio 4.5.1 on asennettu.

**Kiinteä ongelmat:**

- Salasanan takaisinkirjoituksen Azure AD-epäonnistui tuntemattomasta servicebus connectivity-virheen.

## <a name="104910413"></a>1.0.491.0413
Julkaistu: 2015 huhtikuussa

**Kiinteä ongelmat ja parannukset:**

- Active Directory Connectorin ei käsittele poistaa oikein, jos Roskakori on otettu käyttöön ja metsää on useita toimialueita.
- Tuontitoimenpiteitä suorituskykyä on parannettu Azure Active Directory yhdistimen.
- Kun ryhmä on ylittänyt jäsenyyden (oletusarvoisesti rajoitus on määritetty 50 k objekteja), Azure Active Directory-ryhmän on poistettu. Uusi toiminta on ryhmän säilyy, virhe ilmenee, ja muutoksia jäsenyyden viedään.
- Uuden objektin ei voi valmistella, jos vaiheistettu Poista kanssa samaan DN on jo connector-tilaan.
- Jotkin objektit on merkitty synkronoitavissa delta synkronoinnin aikana, mutta ei ole muuttunut, vaiheistettu objektin.
- Ensisijainen Ohjauskoneen luettelon poistaa myös salasanan synkronointi pakottaminen.
- CSExportAnalyzer on joitakin objektien hyötyä ongelmia.

**Uudet ominaisuudet:**

- Liitoksen voi nyt yhdistää "ANY" objektilaji ma.

## <a name="104850222"></a>1.0.485.0222
Julkaistu: 2015 helmikuussa

**Parannukset:**

- Parannettu tuo suorituskyky.

**Kiinteä ongelmat:**

- Salasanan synkronointi merkkien cloudFiltered määrite käytössä määritteen suodattamalla. Suodatettu objekteja ei enää salasanojen synkronoinnin alueen.
- Harvinaisissa tilanteissa, joissa topologian oli erittäin paljon toimialueen ohjaimet salasanan synkronointi ei toimi.
- "Lakkasi palvelin" silloin, kun Azure AD-yhdistintä tuodaan jälkeen hallinta on käytössä Azure AD/Intune.
- Liittyminen ulkomaan suojausasetukset ansaitun (FSPs) saman metsää useiden toimialueiden aiheuttaa virheen moniselitteinen liity.

## <a name="104751202"></a>1.0.475.1202
Julkaissut: 2014 joulukuussa

**Uudet ominaisuudet:**

- Tee salasanojen synkronoinnin määritteiden perusteella suodattamisesta on nyt tueta. Lisätietoja on artikkelissa [salasanojen synkronoinnin suodattamisesta](active-directory-aadconnectsync-configure-filtering.md).
- Koska-ExternalDirectoryObjectID-määrite on kirjoitettu takaisin AD. Tämä lisää tuki Office 365-sovellusten käyttäminen OAuth2 molemmat verkossa, ja paikallisten sähköpostilaatikoiden Exchange-yhdistelmäympäristössä.

**Kiinteä päivitysongelmat:**

- Kirjautumisavustaja uudempi versio on saatavilla palvelimessa.
- Mukautetun asennuspolku asensi Azure AD-synkronointi.
- Virheellinen mukautetun liity-ehdon estää päivitys.

**Muut korjaukset:**

- Vahvistettu mallit Office Pro Plus.
- Kiinteä asennusongelmat aiheutuvat käyttäjien nimet, jotka alkavat yhdysmerkin.
- Kiinteä menettämättä sourceAnchor-asetus, kun käynnissä ohjattu asennus toisen kerran.
- Kiinteä tapahtumien seuranta-jäljityksen salasanan synkronointi

## <a name="104701023"></a>1.0.470.1023
Julkaissut: 2014 lokakuussa

**Uudet ominaisuudet:**

- Salasanan synkronoinnin useita paikalliset AD Azure AD.
- Asennuksen Käyttöliittymä lokalisoitu Windows Server kaikilla kielillä.

**AADSync 1.0 GA päivittäminen**

Jos sinulla on jo asennettu Azure AD-synkronointi, on vaatii siltä varalta, että olet muuttanut ruutuun synkronoinnin sääntöjen muita vaihe. Kun olet päivittänyt 1.0.470.1023 Vapauta, olet muokannut säännöt ovat päällekkäisiä synkronointi. Kunkin muokattu synkronointi säännön seuraavasti:

- Etsi synkronointi sääntö on muokannut ja huomioi muuttuu.
- Poista Synkronoi sääntö.
- Etsi uusi luotu Azure AD-synkronointi synkronointi-sääntö ja ota muutokset käyttöön uudelleen.

**AD-tilin käyttöoikeudet**

AD-tili on myönnettävä voivat lukea salasanan hajautusarvot AD lisäoikeuksia. Myönnä käyttöoikeudet ovat nimeltään "Replikoiminen Directory muutoksia" ja "Replikoiminen kansion muutokset kaikkiin". Molemmat käyttöoikeudet tarvitaan salasana hajautusarvot luettavaksi

## <a name="104190911"></a>1.0.419.0911
Julkaissut: 2014 syyskuu

**Azure AD-synkronointi alkuperäisessä versiossa.**

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

<properties
    pageTitle="Käyttöönoton salasanojen synkronoinnin Azure AD Connect synkronoinnin | Microsoft Azure"
    description="Tietoja salasanojen synkronoinnin toiminta ja kuinka se otetaan käyttöön."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Salasanojen synkronoinnin Azure AD Connect synkronoinnin toteuttaminen
Tämä artikkeli sisältää tiedot, joita tarvitset synkronoimaan käyttäjien salasanat-paikallisen Active Directory (AD) pilvipohjainen Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Mikä on salasanojen synkronoinnin
Todennäköisyys, että on estetty työnteon vuoksi unohtuneen salasanan liittyy eri salasanat, sinun on muistettava määrän. Muista, mitä suurempi unohdat jokin todennäköisyys tarvitset lisää salasanat. Kysymyksiä ja salasanan vaihtamisesta ja muita salasana liittyvien ongelmien puhelut vaatimus useimmat tukipalvelu resursseja.

Salasanojen synkronoinnin toimintoa synkronoimaan käyttäjien salasanat paikallisen Active Directorysta pilvipohjainen Azure Active Directory (Azure AD).
Tämän ominaisuuden avulla voit kirjautua Azure Active Directory-palveluita (kuten Office 365: ssä, Microsoft Intune, CRM Online ja Azure AD-toimialueen palvelut) käytät kirjautua paikallisen Active Directory saman salasanalla.

![Mikä on Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Käyttäjien on säilytettävä, yhden salasanojen vähentämällä salasanojen synkronoinnin avulla voit:

- Paranna tuottavuutta käyttäjät
- Pienentää tukipalvelu kustannuksia  

Myös, jos valitset Käytä [**Federation AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), voit halutessasi ottaa salasanojen synkronoinnin varmuuskopioina siltä varalta, että AD FS-infrastruktuurin epäonnistuu.

Salasanojen synkronoinnin on Azure AD Connect synkronointi directory-synkronoinnin ominaisuutta. Käyttämään salasanojen synkronoinnin ympäristösi on:

- Asenna Azure AD-muodosta  
- Määrittää paikallisen oman kansion synkronoinnin AD ja Azure Active Directory
- Salasanojen synkronoinnin ottaminen käyttöön

Lisätietoja on artikkelissa [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md)

> [AZURE.NOTE] Saat lisätietoja Active Directory-toimialueen palveluista, jotka on määritetty FIPS ja salasanan synkronointi [salasanan synkronointi ja FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Miten salasanan synkronointi toimii?
Active Directory-toimialueen palvelun tallentaa salasanat muodossa hash-arvon esittäminen todellinen käyttäjän salasana. Hash-arvo on saatu yksisuuntainen matemaattista funktiota (-"*hajautuksessa algoritmin*"). Ei ole menetelmää, jos haluat palata salasanan vain teksti-versioon yksisuuntainen funktion tulos. Et voi käyttää salasanan hash kirjautua paikalliseen verkkoon.

Azure AD Connect synkronointi poimii synkronoimaan salasana salasana-hash paikallisen Active Directory. Ylimääräistä käsittely otetaan käyttöön hash salasana, ennen kuin se on synkronoitu Azure Active Directory-Todentamispalvelu. Salasanojen synkronoidaan käyttäjäkohtainen välein ja aikajärjestyksessä.

Todellinen tiedonkulun salasanan synkronointiprosessia muistuttaa synkronointi käyttäjätietoja, kuten näyttönimi tai sähköpostiosoitteet. Kuitenkin salasanat synkronoidaan useammin määritteet vakio directory-synkronoinnin ikkunan. Salasanan synkronointi suoritetaan kahden minuutin välein. Et voi muokata tätä prosessia taajuus. Kun synkronoit salasanan, se korvaa nykyinen cloud salasana.

Ensimmäisen kerran, otat käyttöön salasanan synkronointi, se on suorittanut kaikki laajuus-käyttäjien salasanojen ensimmäinen synkronointi. Et voi määrittää erikseen alijoukkoa käyttäjien salasanat, jonka haluat synkronoida.

Kun muutat paikallisen salasanan, päivitetty salasana on synkronoitu, useimmin asiassa minuuttiin.
Salasanan synkronointi-ominaisuuden automaattisesti yrittää suorittaa uudelleen epäonnistui Synkronointiyritysten. Virhe yritettäessä synkronoida salasanan, virhe on kirjautunut oman Tapahtumienvalvonta.

Salasanan synkronointi ei ole vaikutusta kirjautuneena käyttäjä.
Nykyinen cloud palvelun istunto ei heti vaikuta synkronoidun salasanan muutoksen, joka ilmenee, kun olet kirjautunut sisään pilvipalveluun. Kuitenkin pilvipalvelussa sijaitsevaan edellyttää todennusta uudelleen, kun haluat antaa uudella salasanalla.

> [AZURE.NOTE] Salasanan synkronointi on tuettu vain objektin tyypin käyttäjän Active Directory. Se ei tueta inetOrgHenkilö objektilaji.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Tietoja salasanojen synkronoinnin toiminnasta Azure AD-toimialueen palveluista
Voit käyttää myös salasanan synkronointi-ominaisuuden synkronointi paikallisen salasanojen [Azure AD-toimialueen palveluista](../active-directory-domain-services/active-directory-ds-overview.md). Tässä skenaariossa sallii Azure AD-toimialueen palveluista käyttäjäsi pilveen todentamismenetelmä kaikki käytettävissä olevat paikallisen oman menetelmät AD. Tässä skenaariossa kokemus muistuttaa käyttäminen paikallisen ympäristön Active Directory Migration Tool (ADMT).

### <a name="security-considerations"></a>Suojausasiat
Salasanojen synkronoitaessa salasanasi vain teksti-versio ei näkyvät salasana-synkronointi-ominaisuuden avulla Azure AD-tai liittyvän palveluja.

Lisäksi ei tarvita paikallisen Active Directory-Tallenna salasana kaksisuuntaisesti salattuja muodossa. Active Directory-salasanan hash tiivistelmän käytetään tiedonsiirrossa välillä paikallista AD ja Azure Active Directorysta. Salasanan hash käsittely ei voi käyttää paikallisen ympäristön resurssien käytön.

### <a name="password-policy-considerations"></a>Salasanan käytännön
Salasanakäytännöt, jotka vaikuttavat salasanojen synkronoinnin ottaminen käyttöön on kahdenlaisia:

1. Salasanakäytäntö monimutkaisuus
2. Salasanan vanhenemiskäytännön

**Salasanakäytäntö monimutkaisuus**  
Kun otat salasanojen synkronoinnin, monimutkaisuus salasanakäytännöt paikallisen Active Directoryssa ohittaa monimutkaisuus käytännöt synkronoidut käyttäjät pilvipalvelussa. Voit käyttää kaikkia kelvollinen salasanojen paikallisen Active Directory Azure AD-palveluiden käyttämiseen.

> [AZURE.NOTE] Käyttäjät, jotka on luotu suoraan pilvipalvelussa salasanat ovat edelleen salasanakäytännöt veloittaa pilvipalveluun määritysten mukaisesti.

**Salasanan vanhenemiskäytännön**  
Jos käyttäjä on salasanojen synkronoinnin laajuus, cloud tilin salasana on määritetty*Aina voimassa"*.
Voit jatkaa pilveen palvelujen synkronoidun salasanalla, joka on vanhentunut paikallisen ympäristön kirjautuminen. Cloud salasana päivitetään seuraavan kerran, voit muuttaa paikallisen ympäristön salasana.

### <a name="overwriting-synchronized-passwords"></a>Korvaaminen synkronoida salasanat
Järjestelmänvalvoja voi manuaalisesti Vaihda salasana Windows PowerShellin avulla.

Tässä tapauksessa uutta salasanaa ohittaa synkronoidun salasanasi ja kaikki salasanakäytännöt määritetty pilvessä käytetään uutta salasanaa.

Jos muutat paikallisen salasanasi uudelleen, uusi salasana on synkronoitu pilveen ja ohittaa manuaalisesti päivitetty salasana.

## <a name="enabling-password-synchronization"></a>Salasanojen synkronoinnin ottaminen käyttöön
Salasanan synkronointi otetaan automaattisesti käyttöön, kun asennat Azure AD Connect **Pika-asetuksia**. Lisätietoja on artikkelissa [Azure AD Connect käyttämällä pika-asetuksia käytön aloittaminen](./connect/active-directory-aadconnect-get-started-express.md).

Jos käytät mukautettuja asetuksia, kun asennat Azure AD Connect, voit ottaa käyttöön salasanojen synkronoinnin, valitse sivulla Kirjaudu sisään. Lisätietoja on artikkelissa [Azure AD Connect mukautettu asennus](./connect/active-directory-aadconnect-get-started-custom.md).

![Salasanojen synkronoinnin ottaminen käyttöön](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Salasanojen synkronoinnin ja FIPS
Jos palvelin on lukittu alaspäin mukaan FIPS Federal Information Processing Standard (), MD5 on poistettu käytöstä.

**Ottaa käyttöön MD5 salasanojen synkronoinnin, toimi seuraavasti:**

1. Siirry **%programfiles%\Azure AD Sync\Bin**.
2. Avaa **miiserver.exe.config**.
3. Siirry **määritysten/runtime** -solmu (tiedoston lopussa).
4. Lisää seuraavan solmun:`<enforceFIPSPolicy enabled="false"/>`
5. Tallenna muutokset.

Tämän leikkeen on viittaus, miten se pitäisi näyttää:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Lisätietoja suojaus ja FIPS on [AAD salasanan synkronointi, salaus ja FIPS yhteensopivuus](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Salasanojen synkronoinnin vianmääritys
Jos salasana ei ole synkronoitu oikein, se voi olla joko alijoukkoa käyttäjille tai kaikille käyttäjille.

- Jos ongelma yksittäisten objektien kanssa, katso [vianmääritys yksi objekti, joka ei synkronoidu salasanat](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Jos sinulla on ongelma, jossa ei ole salasanoja synkronoidaan, katso [kohtaa, johon ei ole salasanoja synkronoidaan ongelmien vianmääritys](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Yksi objekti, joka ei synkronoidu salasanat vianmääritys
Voit tehdä synkronoinnin salasanaongelmien helposti tarkastelemalla objektin tila.

Käynnistä **Active Directoryn käyttäjät ja tietokoneissa**. Etsi käyttäjä ja varmista, että **käyttäjä on muutettava salasana seuraavalla kirjautumiskerralla** valitsematon.
![Active Directory tehokkaasti salasanat](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Jos se on valittuna, valitse Pyydä käyttäjää kirjautumaan ja salasanan vaihtaminen. Tilapäiset salasanat ei ole synkronoitu Azure AD.

Jos näkyy oikein Active Directoryssa, seuraava vaihe on sync Engine käyttäjän toimi. Noudattamalla seuraavia käyttäjän paikallisen Active Directorysta Azure AD-näet, onko objektin kuvaava virhe.

1. Aloita **[Synkronointi palvelun hallinta](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Valitse **yhdistimiä**.
3. Valitse **Active Directory Connectorin** käyttäjän sijaitsee.
4. Valitse **Etsi yhdistimen tilaa**.
5. Etsi käyttäjä, et.
6. Valitse **hierarkiatasolla** -välilehti ja varmista, että vähintään yksi synkronointi sääntö näyttää **Salasanan synkronointi** arvoksi **True**. Oletusarvoisessa kokoonpanossa synkronointi säännön nimi on **-AD - käyttäjän AccountEnabled-**.  
    ![Käyttäjän hierarkiatasolla tietoja](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. [Käyttäjän toimi](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) kautta metaverse Azure AD Connector-kohtaan. Yhdistimen tilaa objekti on lähtevällä säännöllä **Salasanan synkronointi** , joka on määritetty **Tosi**. Oletusarvoisessa kokoonpanossa synkronointi säännön nimi on **AAD - käyttäjien liittyminen ulos**.  
    ![Yhdistimen käyttäjän ominaisuuksiin](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Haluat nähdä edellisen viikon objektin salasanan synkronointi-tiedot, valitse **loki..**.  
    ![Objektin lokitiedot](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Tila-sarakkeessa on seuraavat arvot:

Tila | Kuvaus
---- | -----
Onnistui | Salasanan synkronointi epäonnistui.
FilteredByTarget | **Käyttäjän on muutettava salasana seuraavalla kirjautumiskerralla**Aseta salasana. Salasana ei ole synkronoitu.
NoTargetConnection | Ei ole-objekti metaverse tai Azure AD-yhdistin tilaa.
SourceConnectorNotPresent | Objektia ei löydy paikallisen Active Directory connector-tilaa.
TargetNotExportedToDirectory | Azure AD-yhdistin tilaa objektia ei vielä ole viedä.
MigratedCheckDetailsForMoreInfo | Lokitapahtuman on luotu ennen muodosta 1.0.9125.0, joka näkyy vanha tilasta.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Jossa ei ole salasanoja synkronoidaan ongelmien vianmääritys
Aloita suorittamalla komentosarja-kohdassa [salasana synkronointiasetuksia tilan](#get-the-status-of-password-sync-settings). Tällä tavalla saat yleiskatsauksen salasanan synkronointi-määritys.  
![PowerShell-komentosarja tulosteen salasanan synkronointiasetusten](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Jos ominaisuus ei ole otettu käyttöön Azure AD- tai kanava-synkronointitila ei ole otettu käyttöön, suorittamalla ohjattu asennus Yhdistä. Valitse **Mukauta synkronointiasetukset** ja poista salasanan synkronointi. Tämä muutos poistaa ominaisuuden tilapäisesti käytöstä. Valitse ohjatun toiminnon uudelleen ja salasanan synkronointi uudelleen käyttöön. Suorita komentosarja uudelleen ja varmista, että määritykset ovat oikein.

Jos komentosarja osoittaa, että ei ole uudelleenaktivoinnin yhteydessä, suorittamalla komentosarja [käynnistimen koko Synkronoi kaikki salasanat](#trigger-a-full-sync-of-all-passwords). Tämä komentosarja voidaan myös muissa tilanteissa, jossa määritykset ovat oikein, mutta salasanoja ei ole synkronoitu.

#### <a name="get-the-status-of-password-sync-settings"></a>Salasanan synkronointiasetuksia tilan

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Koko Synkronoi kaikki salasanojen käynnistäminen
Voit käynnistää koko Synkronoi kaikki salasanojen käyttämällä seuraavaa komentosarjaa:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Seuraavat vaiheet

* [Azure AD yhteyden synkronointi: Synkronoinnin asetusten mukauttamisen](active-directory-aadconnectsync-whatis.md)
* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

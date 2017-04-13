<properties
    pageTitle="Määrittää suojatun LDAP (LDAPS) Azure AD-toimialueen palveluihin | Microsoft Azure"
    description="Määrittää suojatun LDAP (LDAPS) Azure AD-toimialueen palveluista hallitun toimialueen"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Määrittää suojatun LDAP (LDAPS) Azure AD-toimialueen palveluista hallitun toimialueen
Tässä artikkelissa kerrotaan, miten voit ottaa käyttöön Azure AD-toimialueen palveluista hallitun toimialueen suojatun Lightweight Directory Access Protocol (LDAPS). Suojatun LDAP käytetään myös nimitystä ' Lightweight Directory Access Protocol (LDAP) kautta Secure Sockets Layer (SSL) / Transport Layer Security (TLS) ".

## <a name="before-you-begin"></a>Ennen aloittamista
Tässä artikkelissa luetellut tehtävien suorittamiseen tarvitset:

1. Kelvollinen **Azure tilauksen**.

2. **Azure Active directory** - joko synkronoitu paikallisesta hakemistosta tai vain pilvipalveluita hakemisto.

3. **Azure AD-toimialueen palveluista** on otettava käyttöön Azure AD-kansio. Jos et ole vielä tehnyt niin, noudata kaikki [Aloitusoppaasta](./active-directory-ds-getting-started.md)kuvatut tehtävät.

4. **Sertifikaatin avulla voidaan käyttää suojattuun LDAP**.
    - **Suositus** - sertifikaatin hankkiminen yrityksen CA tai julkinen myöntäjä. Kokoonpano-asetus on turvallisempi.
    - Vaihtoehtoisesti voit myös halutessasi [itse allekirjoitetun](#task-1---obtain-a-certificate-for-secure-ldap) varmenteen esitetyllä tämän artikkelin.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Suojatun LDAP-sertifikaatin vaatimukset
Hankkia varmennetta seuraavat ohjeiden mukaan, ennen kuin otat suojatun LDAP. Käytössä ilmenee virheitä, jos yrität käyttöön suojatun LDAP hallitun toimialueesi virheellinen virheellinen sertifikaatti.

1. **Luotetut myöntäjä** - todistus on annettava tietokoneissa, joissa on suojattu LDAP avulla toimialueelle luottaa myöntäjä. Tämä viranomainen voi olla organisaation myöntäjä tai julkinen myöntäjä luottaa näistä tietokoneista.

2. **Elinaika** - varmenne on oltava vähintään seuraavan 3 – 6 kuukautta kelvollinen. Hallitut toimialueen LDAP tietopalvelimet keskeytyy, kun varmenne vanhenee.

3. **Aihe** - sertifikaatin aihe on oltava hallitun toimialueen yleismerkkiä. Esimerkiksi jos toimialueen nimi on "contoso100.com"-sertifikaatin aihe nimen on oltava "*. contoso100.com". Määritä DNS-nimi (aihe vaihtoehtoinen nimi) yleismerkki nimi.

3. **Avaimen käyttö** - varmenne on määritettävä seuraavat käyttää - digitaaliset allekirjoitukset ja avaimen salaus.

4. **Varmenteen tarkoitus** - varmenne on voimassa SSL palvelimen todennuksen.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Tehtävä 1 - sertifikaatin hankkiminen suojatun LDAP varten
Ensimmäinen tehtävä käsittää hallitun toimialueen LDAP tietopalvelimet käytettävän varmenteen hankkiminen. Sinulla on kaksi vaihtoehtoa:

- Voit hankkia varmenteen myöntäjältä. Viranomainen voi olla organisaation yrityksen CA tai julkinen varmenteiden myöntäjä.

- Itse allekirjoitetun varmenteen luominen


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Vaihtoehto (suositus) - hankkia suojatun LDAP-varmenteen myöntäjältä
Jos organisaatiosi käyttää yrityksen julkisen avaimen infrastruktuuri (PKI), sinun on varmenteen hankkiminen organisaation enterprise-myöntäjältä (CA). Jos organisaatiosi hakee sen varmenteet julkisen myöntäjältä, on suojattu LDAP-sertifikaatin hankkiminen kyseisen julkisen myöntäjältä.

Sertifikaatin pyydettäessä varmistaa noudattamalla [suojattuun LDAP-varmenteeseen tarve](#requirements-for-the-secure-ldap-certificate)vaatimukset.

> [AZURE.NOTE] Asiakastietokoneiden, joka on suojattu LDAP avulla hallitun toimialueelle on luotettava LDAPS varmenteen myöntäjä.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Vaihtoehto B - suojatun LDAP itse allekirjoitetun varmenteen luominen
Voit halutessasi luoda itse allekirjoitetun varmenteen suojatun LDAP, jos:

- organisaation ei varmenteita yrityksen varmenteiden myöntäjä tai
- et aio käyttää varmenteen myöntäjältä julkinen.

**PowerShellin itse allekirjoitetun varmenteen luominen**

Windows-tietokoneessa Avaa PowerShell uudessa ikkunassa **järjestelmänvalvojana** ja kirjoita seuraavat komennot ja luo uusi itse allekirjoitettua varmennetta.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Korvaa "contoso100.com" DNS-toimialuenimeä Azure AD-toimialueen palveluista hallitun toimialueesi edellisessä esimerkissä.

![Valitse Azure AD-hakemisto](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Juuri luomasi itse allekirjoitetun varmenteen sijoitetaan paikallisen tietokoneen varmennesäilöön.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Tehtävä 2 - vienti suojatun LDAP salausvarmenteen,. PFX-tiedosto
Ennen kuin aloitat tämän tehtävän, varmista, että olet saanut suojatun LDAP-sertifikaatin yrityksen myöntäjä tai julkinen myöntäjä tai luonut itse allekirjoitettua varmennetta.

Suorita seuraavat vaiheet, vie LDAPS varmenteen. PFX-tiedosto.

1. Paina **Käynnistä** -painiketta ja kirjoita **R**. Kirjoita **Suorita** -valintaikkunaan **mmc** ja valitsemalla **OK**.

    ![Käynnistä MMC-konsolin](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Valitse **Käyttäjätilien valvonta** -kehotettaessa **Kyllä** käynnistää MMC (Microsoft Management Console) järjestelmänvalvojana.

3. Valitse **Tiedosto** -valikosta **Lisää tai poista Kohdista-apuohjelman..**.

    ![Laajennuksen lisääminen MMC-konsoliin](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. **Lisää tai poista laajennus** -valintaikkunassa valitse **Varmenteet** -laajennuksen ja valitse **Lisää >** painiketta.

    ![Varmenteet-laajennuksen lisääminen MMC-konsoliin](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. Valitse **Varmenteet laajennuksen** ohjatussa **tietokoneessa** -tili ja valitse **Seuraava**.

    ![Varmenteet-laajennuksen tietokonetilin lisääminen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Valitse **Valitse tietokone** -sivulla **paikallisen tietokoneen: (tietokone, jossa tämä konsoli suoritetaan)** ja valitse **Valmis**.

    ![Lisää Varmenteet laajennuksen – Valitse tietokoneeseen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. Valitse **Lisää tai poista laajennus** -valintaikkunan valitsemalla **OK** Varmenteet-laajennuksen lisääminen MMC: HEN.

    ![Varmenteet-laajennuksen lisääminen MMC: HEN - valmis](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. Laajenna **Pääkansion**napsauttamalla MMC-ikkuna. Raportissa pitäisi näkyä Varmenteet laajennuksen lataaminen. Valitse Laajenna **Varmenteet (paikallinen tietokone)** . Laajenna **Omat** solmun perään **Varmenteet** -solmu napsauttamalla.

    ![Avaa henkilökohtaisten sertifikaattien säilön](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Raportissa pitäisi näkyä luomaasi itse allekirjoitettua varmennetta. Voit tarkastella sertifikaatin avulla Varmista allekirjoitus, joka ilmoittaa Windows PowerShellin luodessasi varmenteen ominaisuudet.

10. Valitse itse allekirjoitettua varmennetta ja **Napsauta hiiren kakkospainikkeella**. Valitse **Kaikki tehtävät** ja valitse **Vie**kakkospainikkeen valikosta.

    ![Varmenteen vieminen](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. **Varmenteen vieminen-toiminnossa**Valitse **Seuraava**.

    ![Varmenteen Ohjattu vientitoiminto](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. **Vie yksityinen avain** sivulla Valitse **Kyllä, vie yksityinen avain**ja valitse **Seuraava**.

    ![Vie sertifikaatin yksityinen avain](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] Vietävä sekä sertifikaatin yksityinen avain. Jos annat PFX, jotka eivät sisällä yksityinen avain varmenteen, ottaminen käyttöön suojatun LDAP hallitun toimialueen epäonnistuu.

13. Valitse **Viennin tiedostomuoto** -sivulla **henkilökohtaisia tietoja Exchange - PKCS #12 (. PFX)** viedyn varmenteen tiedosto-muodossa.

    ![Vie varmenne-tiedostomuoto](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Vain. Tuetaan PFX-tiedostomuodossa. Vie sertifikaatin avulla. CER-tiedostomuodossa.

14. Valitse **Suojaus** -sivulla valitse siihen salasanan **salasana** -vaihtoehdon ja kirjoita. PFX-tiedosto. Muista salasana, koska siihen tarvitaan seuraavaan tehtävään. Valitse **Seuraava** Jatka.

    ![Varmenteen vieminen salasana ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Pane merkille salasana. Tarvitset otettaessa suojatun LDAP hallitun toimialueessa [tehtävän 3 – Ota käyttöön hallittu toimialueen suojatun LDAP](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Valitse **vietävä tiedosto** -sivulla Määritä tiedostonimi ja sijainti, johon haluat viedä varmenteen.

    ![Varmenteen vieminen polku](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Valitse seuraavalla sivulla **Valmis** varmenteen vieminen PFX-tiedostoon. Raportissa pitäisi näkyä vahvistusvalintaikkuna, kun varmenne on viety.

    ![Valmis varmenteen vieminen](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Tehtävä 3 – Ota käyttöön hallittu toimialueen suojatun LDAP
Ota suojattu LDAP-suorittamalla seuraavat määritysvaiheet:

1. Siirry **[Azure perinteinen portal](https://manage.windowsazure.com)**.

2. Valitse vasemmanpuoleisessa ruudussa **Active Directory** -solmu.

3. Valitse Azure AD hakemiston (niin sanottu "vuokraajan"), jossa on käytössä Azure AD-toimialueen palveluista.

    ![Valitse Azure AD-hakemisto](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Valitse **määritys** -välilehti.

    ![Määritä hakemisto-välilehti](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Vieritä sivua alaspäin kohtaan **toimialuepalveluita**. Vaihtoehto liittyvä **Suojatun LDAP (LDAPS)** , kuten seuraavassa näyttökuvassa näkyy:

    ![Toimialueen palveluiden Tietolähdemääritykset-osasta](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Noutaa **Suojatun LDAP määrittäminen sertifikaatti** -valintaikkuna valitsemalla **Määritä varmenteen...** .

    ![Varmenteen määrittäminen suojatun LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Napsauta kansiokuvaketta **Tiedoston PFX SERTIFIKAATIN avulla** voit määrittää PFX-tiedosto, jossa haluat käyttää hallittuja toimialueen LDAP tietopalvelimet varmenne on alla. Myös salasana kirjautumiskerralla varmenteen vieminen PFX-tiedostoon. Napsauta alareunassa Valmis-painiketta.

    ![Määritä suojatun LDAP PFX-tiedosto ja salasana](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. **Määritä** -välilehden **toimialuepalveluita** osa pitäisi saada harmaana ja ovat **odotetaan...** tilan muutaman minuutin. Tänä aikana LDAPS varmenne on vahvistettu tarkkuus ja suojatun LDAP on määritetty hallitun toimialueen.

    ![LDAP - suojatun Odottava tila](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Suojatun LDAP hallitun toimialueen ottaminen käyttöön vie noin 10 – 15 minuuttia. Jos annettu suojattuun LDAP-varmenteeseen vastaa vaatimukset, suojattu LDAP ei ole otettu käyttöön hakemistossa ja tuo näkyviin epäonnistui. Esimerkiksi toimialuenimi on virheellinen, varmenne on vanhentunut tai vanhenee pian jne.

9. Kun suojatun LDAP onnistuneesti käytössä hallitun toimialueen **odotetaan...** viesti pitäisi hävitä. Pitäisi tulla näkyviin varmenteen allekirjoitus.

    ![Suojatun LDAP käytössä](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Tehtävän 4 – Ota käyttöön turvallinen LDAP käyttäminen Internetin kautta
**Valinnaiset tehtävät** - Jos et aio käyttää hallitun toimialueen käyttäminen LDAPS Internetin välityksellä ja ohittaa määritysten tämän tehtävän.

Ennen kuin aloitat tämän tehtävän, varmista olet tehnyt [tehtävä 3](#task-3---enable-secure-ldap-for-the-managed-domain)kuvatut vaiheet.

1. Raportissa pitäisi näkyä **Käyttöön suojatun LDAP ACCESS PÄÄLLE INTERNET** - **toimialueen palveluiden** osa **määrittäminen** -sivulla haluamasi vaihtoehto. Tämä asetus on määritetty **ei** oletusarvoisesti jälkeen hallitun toimialueen suojatun LDAP päälle internet-yhteys on oletusarvoisesti pois käytöstä.

    ![Suojatun LDAP käytössä](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Näytä tai piilota **sen Internetin kautta suojatun LDAP-käytön käyttöön ottaminen** **Kyllä**. Napsauta alareunan paneeli **Tallenna** -painiketta.
    ![Suojatun LDAP käytössä](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. **Määritä** -välilehden **toimialuepalveluita** osa pitäisi saada harmaana ja ovat **odotetaan...** tilan muutaman minuutin. Hetken kuluttua hallitun toimialueen suojatun LDAP päälle internet-yhteys on käytössä.

    ![LDAP - suojatun Odottava tila](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Kestää noin 10 minuuttia haluat ottaa käyttöön suojattu LDAP hallitun toimialueen internet-yhteyttä.

4. Kun hallitun toimialueen Internetin välityksellä LDAP tietopalvelimet onnistuneesti käytössä, **odotetaan...** viesti pitäisi hävitä. Raportissa pitäisi näkyä ulkoisen IP-osoite, jonka avulla voidaan käyttää hakemiston kautta LDAPS **Ulkoinen IP ADDRESS FOR LDAPS käyttö**-kenttään.

    ![Suojatun LDAP käytössä](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Tehtävän 5 – käyttämään Internetistä hallitun toimialueen DNS-asetusten määrittäminen
**Valinnaiset tehtävät** - Jos et aio käyttää hallitun toimialueen käyttäminen LDAPS Internetin välityksellä ja ohittaa määritysten tämän tehtävän.

Ennen kuin aloitat tämän tehtävän, varmista olet tehnyt [tehtävän 4](#task-4---enable-secure-ldap-access-over-the-internet)kuvatut vaiheet.

Kun olet ottanut LDAP tietopalvelimet Internetin välityksellä hallitun toimialueen, haluat päivittää DNS niin, että asiakastietokoneiden löytävät hallitun toimialueessa. Tehtävän 4 lopussa ulkoinen IP-osoite näkyy **määrittäminen** **ULKOISTEN IP ADDRESS LDAPS Access-käyttöoikeudet**-välilehdessä.

Määritä ulkoisen DNS-palveluntarjoajan siten, että hallitut toimialueen (kuten contoso100.com) DNS nimi osoittaa tämän ulkoisen IP-osoitteeseen. Tässä esimerkissä annettava seuraavat DNS-tekstin luominen:

    contoso100.com  -> 52.165.38.113

Siinä – voit nyt hallitun toimialueelle suojatun LDAP avulla Internetin välityksellä.

> [AZURE.WARNING] Muista asiakastietokoneiden on luotettava LDAPS varmenteen myöntäjä avulla voi yhdistää onnistuneesti käyttämällä LDAPS hallitun toimialueeseen. Jos käytät yrityksen varmenteiden myöntäjä tai julkisesti luotetun varmenteiden myöntäjältä, sinun ei tarvitse tehdä mitään, koska asiakastietokoneiden luota nämä sertifikaatin myöntäjistä. Jos käytössäsi on itse allekirjoitettua varmennetta, sinun täytyy asentaa itse allekirjoitetun varmenteen julkisen osan asiakastietokoneen luotettu varmenteen säilöön.

<br>

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](active-directory-ds-admin-guide-administer-domain.md)

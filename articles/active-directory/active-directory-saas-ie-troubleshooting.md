<properties
    pageTitle="Access-Ohjauspaneelin osa vianmääritysohjeita Internet Explorerin | Microsoft Azure"
    description="Voit ottaa käyttöön Internet Explorer-lisäosan Omat sovellukset-portaalin ryhmäkäytännön avulla."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Access-Ohjauspaneelin osa vianmääritysohjeita Internet Explorerissa

Tämän artikkelin avulla voit vianmääritys seuraavista ongelmista:

- Ei voi käyttää sovelluksia Omat sovellukset-portaalin samalla, kun Internet Explorerissa.
- Näyttöön tulee "Asenna ohjelmisto"-sanoma, vaikka olet jo asentanut ohjelmiston.

Jos olet järjestelmänvalvoja, katso myös: [ottamisesta käyttöön Access Ohjauspaneelin osa Internet Explorerin ryhmäkäytännön avulla](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Diagnostiikan-työkalun suorittaminen

Voit selvittää asennusongelmat Access-paneelin tunniste lataamalla ja suorittamalla Access-paneelin diagnostiikkatyökalu:

1. [Lataa diagnostiikkatyökalu napsauttamalla tätä.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Avaa tiedosto ja valitse **Pura kaikki** -painike.

    ![Paina purkaa kaikki](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Paina Jatka **Pura** -painiketta.

    ![Paina Pura](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Voit suorittaa työkalun, nimeltä **AccessPanelExtensionDiagnosticTool**tiedostoa hiiren kakkospainikkeella ja valitse sitten **Avaa > Microsoft-Windows-pohjaisten Script Host**.

    ![Avaa > Microsoft Windows Script Host perusteella](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Näkyviin tulee Valitse seuraavat diagnostiikan ikkunan, jossa kuvataan, mitä voi olla väärä Asennusongelma.

    ![Esimerkki diagnostiikan ikkuna](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Valitse "**Kyllä**" haluat, että ohjelma korjaa ongelmat, jotka on löydetty.

7. Jos haluat tallentaa muutokset, Sulje kaikki Internet Explorer-ikkuna ja avaa Internet Explorerin uudelleen.<br />Jos et voi käyttää sovelluksia, kokeile seuraavia ohjeita.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Tarkista, että Access Ohjauspaneelin osa on käytössä

Voit varmistaa, että Access Ohjauspaneelin osa on otettu käyttöön Internet Explorerissa seuraavasti:

1. Napsauta Internet Explorer-ikkunan oikeassa yläkulmassa olevaa **hammaspyöräkuvaketta** . Valitse **Internet-asetukset**.<br />(Internet Explorerin aiemmissa versioissa tässä kohdassa voit etsiä **Työkalut > Internet-asetukset**.

    ![Valitse Työkalut > Internet-asetukset](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Valitse **Ohjelmat** -välilehti ja valitse sitten **Lisäosien hallinta** -painiketta.

    ![Valitse Lisäosien hallinta](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. Tässä valintaikkunassa valitse **Access Ohjauspaneelin osa** ja valitse sitten **Ota käyttöön** -painiketta.

    ![Valitse Ota käyttöön](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Jos haluat tallentaa muutokset, Sulje kaikki Internet Explorer-ikkuna ja avaa sitten Internet Explorerin uudelleen.

##<a name="enable-extensions-for-inprivate-browsing"></a>InPrivate-selaus laajennusten ottaminen käyttöön

Jos käytössäsi on InPrivate-selaus-tilassa:

1. Napsauta Internet Explorer-ikkunan oikeassa yläkulmassa olevaa **hammaspyöräkuvaketta** . Valitse **Internet-asetukset**.<br />(Internet Explorerin aiemmissa versioissa tässä kohdassa voit etsiä **Työkalut > Internet-asetukset**.

    ![Esimerkki diagnostiikan ikkuna](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Valitse **Tietosuoja** -välilehti, valitse **Poista** valintaruudun valinta, jossa **käytöstä työkalurivit ja laajennukset InPrivate-selaus käynnistyessä**</p>

    ![Poista käytöstä työkalurivit ja laajennukset InPrivate-selaus käynnistyessä](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Jos haluat tallentaa muutokset, Sulje kaikki Internet Explorer-ikkuna ja avaa sitten Internet Explorerin uudelleen.

##<a name="uninstall-the-access-panel-extension"></a>Poista Access Ohjauspaneelin osa

Asennuksen poistaminen Access-Ohjauspaneelin osa tietokoneesta:

1. Näppäimistöstä paina **Windows-näppäin** Avaa Käynnistä-valikko. Kun valikko on avoinna, voit kirjoittaa tekemistä haun. Kirjoita "Ohjauspaneelin" ja Avaa **Ohjauspaneeli** esiintyessään hakutulokset.

    ![Etsi Ohjauspaneeli](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. **Suuret**kuvakkeet **Näyttöperuste** -asetuksen muuttaminen Ohjauspaneelin oikeassa yläkulmassa. Etsi ja valitse **Ohjelmat ja toiminnot** -painiketta.

    ![Muut näkymän näyttämään suuret kuvakkeet](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Valitse luettelosta, **Access Ohjauspaneelin osa**ja valitse sitten **Poista** -painiketta.

    ![Valitse Poista](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Voit sitten yrität asentaa uudelleen, jotta näet ongelma on ratkaistu tunniste.

Jos kohtaat ongelmia laajennuksen asennuksen poistaminen, voit myös poistaa se [Microsoft korjaa It](https://go.microsoft.com/?linkid=9779673) -työkalun avulla.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Sovelluksen käyttöoikeuksien ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md)
- [Access-Ohjauspaneelin osa ottamisesta käyttöön Internet Explorerin ryhmäkäytännön avulla](active-directory-saas-ie-group-policy.md)

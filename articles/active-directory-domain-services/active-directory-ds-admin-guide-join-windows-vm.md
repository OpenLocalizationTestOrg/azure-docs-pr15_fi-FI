<properties
    pageTitle="Azure Active Directory-toimialueen palveluista: Liittää Windows Server-AM hallitun toimialueelle | Microsoft Azure"
    description="Windows Server-virtual machine liittäminen Azure AD-toimialueen palveluista"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Liittää Windows Server-virtual machine hallitun toimialueelle

> [AZURE.SELECTOR]
- [Azure perinteinen portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

Tässä artikkelissa kerrotaan liittyminen virtual koneen käytössä Windows Server 2012 R2 Azure AD-toimialueen palveluista hallitun toimialueeseen, Azure perinteinen-portaalissa.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Vaihe 1: Luo Windows Server virtuaalikoneen
Noudata kuvatut [Luo virtual machine käynnissä Windows Azure perinteinen portaalissa](../virtual-machines/virtual-machines-windows-classic-tutorial.md) opetusohjelman. On tärkeää varmistaa, että tämä vastaluotu virtuaalikoneen on yhdistetty samaan virtual verkkoon, jonka otit Azure AD-toimialueen palveluista. "Lyhyt Luo"-vaihtoehto ei ole avulla voit liittyä virtuaalikoneen virtual verkkoon. Tämän vuoksi haluat "-valikoima"-asetuksen avulla voit luoda virtuaalikoneen.

Seuraavien toimien luominen Windows virtual machine-liittyneet virtual verkkoon, jossa on otettu Azure AD-toimialueen palveluista.

1. Azure perinteinen portaalissa komentopalkista-ikkunan alareunassa valitsemalla **Uusi**.

2. Valitse **Laske**Valitse **virtuaalikoneen**ja valitse sitten **-Valikoima**.

3. Ensimmäisessä näkymässä voit **Valitse kuva** , virtuaalikoneen luettelosta, käytettävissä olevat kuvat. Valitse haluamasi kuva.

    ![Valitse kuva](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Toisessa näytössä voit tietokonenimi, kokoa, ja järjestelmänvalvojan käyttäjänimi ja salasana. Käytä taso ja sovelluksen tai työmäärää suorittamiseen vaadittava kokoa. Valitse tähän käyttäjänimi on paikallisen järjestelmänvalvojan käyttäjän tietokoneeseen. Älä kirjoita tähän toimialueen käyttäjätilin käyttäjän tunnistetiedot.

    ![Määritä virtuaalikoneen](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Kolmannessa näytössä voit määrittää verkko, varasto ja käytettävyyden resurssit. Varmista Valitse virtual verkkoon, jonka otit Azure AD-toimialueen palveluista **Alue/affiniteetti ryhmän/Virtual verkon** avattavasta valikosta. Määritä **Cloud palvelun DNS nimi** virtuaalikoneen tarvittaessa.

    ![Valitse virtuaalikoneen virtual verkosta](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Varmista, että liityt virtuaalikoneen samaan virtual verkkoon, jossa on otettu Azure AD-toimialueen palveluista. Tuloksena virtuaalikoneen voi nähdä toimialueen ja tehtäviä, kuten toimialueeseen. Jos haluat luoda virtuaalikoneen eri virtual verkossa, Yhdistä virtual verkon virtual verkkoon, jossa on otettu Azure AD-toimialueen palveluista.

6. Neljännessä näytössä voit AM-agentti asentaa ja määrittää joitakin käytettävissä tunnisteet.

    ![Valmis](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Kun virtuaalikoneen on luotu, perinteinen portaalin luettelo uusi virtuaalikoneen **näennäiskoneiden** solmun. Virtuaalikoneen ja pilvipalvelussa käynnistetään automaattisesti sekä niiden tilan lueteltujen **käynnissä**.

    ![Virtuaalikoneen on hyvin alkuun](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Vaihe 2: Yhteyden muodostaminen Windows Server virtuaalikoneen paikallisen järjestelmänvalvojatilin avulla
Nyt muodostetaan juuri luomasi Windows Server virtuaalikoneen, voit liittyä toimialueeseen. Käyttää määritit luotaessa virtuaalikoneen, jos haluat muodostaa paikallisen järjestelmänvalvojan tunnistetietoja.

Seuraavien toimien muodostaa virtuaalikoneen.

1. Siirry **näennäiskoneiden** solmu perinteinen-portaalissa. Valitse vaiheessa 1 luomasi virtuaalikoneen ja valitse **Yhdistä** komentopalkista ikkunan alareunassa.

    ![Yhteyden muodostaminen Windows virtuaalikoneen](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Perinteinen portaalin pyytää sinua avaamaan tai tallentamaan tiedosto ".rdp"-tunniste, jota käytetään yhteyden muodostamiseen virtuaalikoneen. Valitse Avaa tiedosto, kun se on ladannut.

3. Kirjaudu sisään tulee näkyviin Kirjoita **paikallisen järjestelmänvalvojan tunnistetiedot**, jotka olet määrittänyt virtuaalikoneen luomisen aikana. Esimerkiksi on tässä esimerkissä on käytetty "localhost\mahesh".

Tässä vaiheessa voit pitäisi olla kirjautunut virtuaalikoneen juuri luomasi Windows paikallisen järjestelmänvalvojan tunnistetiedoilla. Seuraavaksi liittymään virtuaalikoneen toimialueeseen.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Vaihe 3: Yhdistä Windows Server virtuaalikoneen AAD DS hallitun toimialueeseen
Seuraavien toimien liittymään Windows Server virtuaalikoneen AAD DS hallitun toimialueeseen.

1. Yhteyden muodostaminen Windows Server vaiheessa 2 esitetyllä tavalla. Avaa aloitusnäytössä **Serverin hallinta**.

2. Valitse **Paikallinen Server** Serverin hallinta-ikkunan vasemmassa reunassa.

    ![Käynnistää virtuaalikoneen Serverin hallinta](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. **TYÖRYHMÄN** kohdasta **Ominaisuudet** -osan. Valitse **Järjestelmän ominaisuudet** -ominaisuutta-sivulla **Vaihda** toimialueeseen.

    ![Järjestelmän ominaisuudet-sivun](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Määritä Azure AD-toimialueen palvelujen toimialuenimen hallitun toimialueen **toimialueen** tekstiruutuun ja valitse **OK**.

    ![Määritä toimialue on liitetty](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Sinua kehotetaan kirjoittamalla tunnistetietosi toimialueeseen. Varmista, voit **määrittää AAD Ohjauskoneen järjestelmänvalvojien kuuluvien käyttäjän tunnistetietojen** ryhmään. Tämän ryhmän jäsenet on oikeudet liittää koneet hallitun toimialueeseen.

    ![Määritä toimialueen liity tunnistetiedot](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Voit määrittää tunnistetiedot jommallakummalla seuraavista tavoista:

    - Täydellisen Käyttäjätunnuksen muoto: Määritä käyttäjätili, täydellisen Käyttäjätunnuksen jälkiliite Azure AD määritetty. Tässä esimerkissä täydellisen Käyttäjätunnuksen jälkiliitteen käyttäjän 'Teemu' on 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName muoto: Voit määrittää tilin nimen SAMAccountName-muodossa. Tässä esimerkissä käyttäjä 'Teemu' on "CONTOSO100\bob" Anna.

        > [AZURE.NOTE] **On suositeltavaa täydellisen Käyttäjätunnuksen muodossa Määritä tunnistetiedot.** SAMAccountName saattaa olla automaattisesti luodut Jos käyttäjän UPN etuliite on liian pitkä (esimerkiksi "joereallylongnameuser"). Jos useilla käyttäjillä on sama UPN etuliite (esimerkiksi Teemu) Azure AD-vuokraajan, niiden SAMAccountName-muodossa voi olla automaattisesti luodut palvelu. Tällaisissa tapauksissa UPN-muotoa voidaan luotettavasti kirjautua toimialue.

7. Kun toimialueen liity onnistuu, näyttöön tulee seuraava sanoma, jossa toivotetaan toimialueen. Viimeistele toimialueen join-toiminto virtuaalikoneen uudelleen.

    ![Tervetuloa käyttämään toimialueen](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Toimialueen liity vianmääritys
### <a name="connectivity-issues"></a>Yhteysongelmat
Jos virtuaalikoneen ei löydä toimialueen, voit tarkistaa seuraavien vianmääritysvaiheiden avulla:

- Varmista, että virtuaalikoneen on yhdistetty samaan virtual verkkoon, jotka on otettu toimialueen palvelut. Jos et virtuaalikoneen ei toimialueelle ja siksi voi liittyä toimialueeseen.

- Jos virtuaalikoneen on yhdistetty toiseen virtual verkkoon, varmista, että virtual verkoston on kytketty virtual verkkoon, jossa on otettu toimialuepalveluita.

- Käytä ping käyttämällä hallitun toimialueen (kuten ping contoso100.com) toimialuenimi toimialue. Jos ole epäonnistui, yritä ping-sivulla toimialueen otit Azure AD-toimialueen palveluista IP-osoitteet (esimerkiksi "ping 10.0.0.4"). Jos et pysty ping-IP-osoite, mutta ei toimialueen, DNS on määritetty väärin. Olet ehkä ole määrittänyt toimialueen IP-osoitteiden kuin virtual verkon DNS-palvelimet.

- Kokeile materiaalinottoon DNS-tulkintatoiminnon välimuisti-virtuaalikoneen ('ipconfig /-flushdns").

Jos saat-valintaikkuna, jossa kysytään, tunnistetiedot toimialueeseen, yhteysongelmat ei ole.


### <a name="credentials-related-issues"></a>Tunnistetietojen liittyvät ongelmat
Viittaavat seuraamalla näitä ohjeita, jos ilmenee ongelmia, joilla on valtuudet ja ei voi liittyä toimialueeseen.

- Kokeile UPN-muotoa Määritä tunnistetiedot. Tilin SAMAccountName voi olla luo automaattisesti, jos on useita käyttäjiä, joilla on sama UPN etuliite vuokraajan tai UPN etuliite on liian pitkä. Vuoksi tilissäsi SAMAccountName-muodossa voi olla eri kuin mitä voit odottaa tai paikallisen toimialueen.

- Yrität käyttää käyttäjätili, jolla kuuluu 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään liittymään koneet hallitun toimialueen tunnistetiedot.

- Varmista, että sinulla on [käytössä salasanojen synkronoinnin](active-directory-ds-getting-started-password-sync.md) mukaisesti käytön aloittaminen-oppaassa kuvatut vaiheet.

- Varmista, että voit käyttää käyttäjän UPN Azure AD määritetty (esimerkiksi 'bob@domainservicespreview.onmicrosoft.com') kirjautuminen.

- Varmista, että olet odottanut tarpeeksi kauan, jotta salasanojen synkronoinnin suorittamiseen Aloitusoppaasta mukaisesti.


## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Azure AD - toimialueen palvelut-aloitusopas](./active-directory-ds-getting-started.md)

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](./active-directory-ds-admin-guide-administer-domain.md)

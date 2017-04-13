<properties
    pageTitle="Azure Active Directory-toimialueen palveluista: Liittää RHEL AM hallitun toimialueelle | Microsoft Azure"
    description="Punainen on Enterprise Linux virtual koneen liittäminen Azure AD-toimialueen palveluista"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Liittää punainen on Enterprise Linux 7 virtual koneen hallitun toimialueelle
Tässä artikkelissa kerrotaan liittyminen punainen on Enterprise Linux (RHEL) 7 virtual koneen Azure AD-toimialueen palveluista hallitun toimialueeseen.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Punainen on Enterprise Linux virtual koneen valmistelu
Seuraavien toimien valmistelu RHEL 7 virtual machine-Azure-portaalissa.

1. Kirjautuminen [Azure portal](https://portal.azure.com).

    ![Azure portaalin koontinäyttö](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Valitse vasemmanpuoleisessa ruudussa **Uusi** ja kirjoita **Punainen on** etsintäpalkin, kuten seuraavassa näyttökuvassa. Punainen on Enterprise Linux merkintöjen hakutuloksissa. Valitse **Punainen on Enterprise Linux 7.2**.

    ![Valitse RHEL tulokset](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Hakutulosten **kaikki** -ruudussa pitäisi luettelon punainen on Enterprise Linux 7.2 kuva. Valitse **Punainen on Enterprise Linux 7.2** voit tarkastella lisätietoja virtuaalikoneen kuva.

    ![Valitse RHEL tulokset](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. **Punainen on Enterprise Linux 7.2** -ruudussa pitäisi näkyä lisätietoja virtuaalikoneen kuva. Valitse **käyttöönoton mallin valitseminen** avattavasta valikosta **perinteinen**. Valitse **Luo** -painiketta.

    ![Kuvan tarkasteleminen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Kirjoita uuden virtuaalikoneen **Isäntänimi** **Luominen AM** -ruudussa. Määritä myös paikallisen järjestelmänvalvojan käyttäjänimi **käyttäjänimi** ja **salasana**. Voit halutessasi myös todennetaan paikallisen järjestelmänvalvojan SSH avaimen avulla. Valitse myös **Hinnat taso** virtuaalikoneen.

    ![Luo AM - perustiedot](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Valitse **Valinnainen määritys**. Napsauta **verkon** **Valinnainen config** -ruudussa.

    ![Luo AM - VPN määrittäminen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Tämä näyttää liittyvä **verkon**ruutu. Napsauta **Verkko** -ruudussa **VPN** virtual verkosto, johon Linux AM käyttöön. **VPN** -ruutu avautuu. Valitse **VPN** -ruudussa **Käytä aiemmin virtual verkon** valinta. Valitse virtual verkkoon, jossa Azure AD-toimialueen palveluista on käytettävissä. Tässä esimerkissä on Valitse 'MyPreviewVNet' virtual verkkoon.

    ![Luo AM – Valitse VPN](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. **Valinnainen config** -ruudun Napsauta **OK** -painiketta.

    ![AM - virtual verkon luominen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Olet nyt valmis luomaan virtuaalikoneen. **Luo AM** -ruudun Napsauta **Luo** -painiketta.

    ![Luo AM - valmis](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Käynnistä käyttöönoton uusi virtuaalikoneen RHEL 7.2 kuvan perusteella.

  ![Luo AM - käyttöönotto aloitettu](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Muutaman minuutin kuluttua virtuaalikoneen käyttöön onnistuneesti ja valmis käytettäväksi.

  ![Luo AM - käyttöön](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Äskettäin valmistellun Linux virtuaalikoneen etäkäyttö
RHEL 7.2 virtuaalikoneen on valmisteltu Azure-tietokannassa. Seuraava tehtävä on virtuaalikoneen etäkäyttö.

**Yhteyden muodostaminen RHEL 7.2 virtuaalikoneen** Noudata artikkelissa [virtual koneeseen, jossa käytetään Linux kirjautuminen](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Loput vaiheet oletetaan, että RHEL virtuaalikoneen muodostaa painovärit, muste SSH-asiakasohjelman avulla. Lisätietoja on artikkelissa [painovärit, muste lataussivulta](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Avaa painovärit, muste ohjelma.

2. Kirjoita **Isäntänimi** juuri luomasi RHEL virtuaalikoneen. Tässä esimerkissä Microsoftin virtuaalikoneen on isäntänimi "contoso-rhel.cloudapp .net". Jos et ole varma, että AM isäntänimi viittaa AM Raporttinäkymät-ikkunan Azure-portaalissa.

    ![Painovärit, muste yhdistäminen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Kirjaudu virtuaalikoneen määrittämäsi virtuaalikoneen luotaessa paikallisen järjestelmänvalvojan tunnistetiedoilla. Tässä esimerkissä on käytetty paikallisen järjestelmänvalvojatilin "mahesh".

    ![Painovärit, muste kirjautuminen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Asenna tarvittavat paketit Linux virtuaalikoneen
Kun olet virtuaalikoneen muodostaa yhteyden, seuraava tehtävä on Asenna tarvittavat toimialueen liity virtuaalikoneen-paketit. Suorita seuraavat vaiheet:

1. **Asentaa realmd:** Realmd paketin käytetään toimialueen liity. Kirjoita Päätteen painovärit, muste seuraava komento:

    sudo yum asennuksen realmd

    ![Asenna realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Muutaman minuutin kuluttua realmd paketin pitäisi saada asennettu virtuaalikoneen.

    ![realmd asennettuna](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Asentaa sssd:** Realmd package määräytyy sssd suorittamiseen toimialueen join-toiminnot. Kirjoita Päätteen painovärit, muste seuraava komento:

    sudo yum asennuksen sssd

    ![Asenna sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Muutaman minuutin kuluttua sssd paketin pitäisi saada asennettu virtuaalikoneen.

    ![realmd asennettuna](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Asentaa kerberos:** Kirjoita Päätteen painovärit, muste seuraava komento:

    sudo yum Asenna krb5 työaseman krb5-kirjastoa

    ![Asenna kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Muutaman minuutin kuluttua realmd paketin pitäisi saada asennettu virtuaalikoneen.

    ![Kerberos asennettuna](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Liittää Linux virtuaalikoneen hallitun toimialueeseen
Kun tarvittavat paketit on asennettu Linux virtuaalikoneen, seuraava tehtävä on liittymään virtuaalikoneen hallitun toimialueen.

1. Tutustu AAD toimialuepalveluita hallitun toimialueen. Kirjoita Päätteen painovärit, muste seuraava komento:

    Tutustu sudo alueen CONTOSO100.COM

    ![Tutustu alueen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Jos **alueen Tutustu** on ei löydy hallitun toimialueen, varmista, että toimialue on käytettävissä virtuaalikoneen (kokeile ping). Varmista myös, että virtuaalikoneen varmasti otettu samaan virtual verkkoon, jossa hallitun toimialue on käytettävissä.

2. Alusta kerberos. Kirjoita seuraava komento päätteen painovärit, muste. Varmista, että määrität käyttäjälle, joka kuuluu 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään. Vain nämä käyttäjät voivat liittyä tietokoneita hallittua toimialueen.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Varmista, että määrität toimialuenimi isoin kirjaimin, muita kinit epäonnistuu.

3. Liittää koneen toimialueeseen. Kirjoita seuraava komento päätteen painovärit, muste. Määritä sama käyttäjä kuin edellisessä vaiheessa ('kinit').

    sudo alueen liity--yksityiskohtainen CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Alueen liitos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Näyttöön tulee sanoma ("onnistuneesti rekisteröityneet machine-alueen") kun tietokone on liittynyt hallitun toimialueeseen.


## <a name="verify-domain-join"></a>Tarkista toimialueen liitos
Voit nopeasti tarkistaa, onko koneen onnistui hallitun toimialueeseen. Yhteyden muodostaminen juuri toimialueen liittyneet RHEL AM käyttämällä SSH ja toimialueen käyttäjätili ja valitse käyttäjätili ratkennut oikein.

1. Kirjoita Päätteen painovärit, muste seuraavalla komennolla yhdistäminen juuri toimialueen liittyneet RHEL virtual tietokoneeseen käyttämällä SSH. Käytä domain-tili, joka kuuluu hallitun toimialueen (kuten 'bob@CONTOSO100.COM' tässä tapauksessa.)

    ssh -l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. Kirjoita seuraava komento nähdäksesi, jos pääkansion valmistellaan oikein päätteen painovärit, muste.

    pwd

3. Kirjoita seuraava komento nähdäksesi, jos kirjautuessa parhaillaan selvitetään oikein päätteen painovärit, muste.

    tunnus

Esimerkki tulosteen, näitä komentoja seuraavasti:

![Tarkista toimialueen liitos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Toimialueen liity vianmääritys
Lue lisätietoja artikkelista [vianmääritys toimialueen liity](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Aiheeseen liittyvää sisältöä
- [Azure AD - toimialueen palvelut-aloitusopas](./active-directory-ds-getting-started.md)

- [Liity Windows Server-virtual machine Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Käynnissä Linux virtual tietokoneeseen kirjautuminen](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Kerberos asentaminen](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Punainen on Enterprise Linux 7 – Windows integrointi opas](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)

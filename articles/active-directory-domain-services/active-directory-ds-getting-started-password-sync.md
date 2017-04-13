<properties
    pageTitle="Azure AD-toimialueen palveluista: Ota käyttöön salasanan synkronointi | Microsoft Azure"
    description="Azure Active Directory-toimialueen palveluiden käytön aloittaminen"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure AD-toimialueen palveluihin salasanojen synkronoinnin ottaminen käyttöön
Edeltävät tehtävät otit Azure AD-toimialueen palveluista Azure AD-vuokraajan. Seuraava tehtävä on ottaa käyttöön tunnistetiedon hajautusarvot synkronoimaan Azure AD-toimialueen palveluihin NTLM ja Kerberos-todennus vaaditaan. Kun tunnistetiedon synkronointi on määritetty, käyttäjät voivat kirjautua sisään käyttämällä yrityksen käyttöoikeutensa hallitun toimialueeseen.

Vaiheet ovat eri perusteella, onko organisaatiossasi on tarkoitus käyttää vain pilvipalveluita Azure AD vuokraaja tai on määritetty synkronoimaan käyttämällä Azure AD Connect paikallista hakemistoa.

<br>

> [AZURE.SELECTOR]
- [Vain pilvipalveluita Azure AD vuokraaja](active-directory-ds-getting-started-password-sync.md)
- [Synkronoitu Azure AD-vuokraajan](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Vaihe 5: Ota käyttöön salasanojen synkronoinnin AAD-toimialueen palveluihin vain pilvipalveluita Azure AD-vuokraajan
Azure AD-toimialueen palveluista on tunnistetietojen hajautusarvot muodossa, joka sopii NTLM ja Kerberos-todennus käyttäjien hallitun toimialueen tarkistamiseen. Ellei AAD toimialuepalveluita vuokraajan käyttöön Azure AD ei luo tai tallentaa tunnistetiedon hajautusarvot NTLM tai Kerberos-todennus vaaditaan muodossa. Ilmeisimmät tietoturvasyistä Azure AD myös Tallenna tunnistetietoja teksti-muodossa. Tämän vuoksi Azure AD ei ole vielä luo nämä NTLM tai Kerberos tunnistetiedon hajautusarvot käyttäjien aiemmin tunnistetiedot perusteella.

> [AZURE.NOTE] Jos organisaatiolla on pelkkää cloud Azure AD-vuokraajan, käyttäjät, joilla on käytettävä Azure AD-toimialueen palveluista on muutettava salasanansa.

Salasanan muuttaminen prosessia aiheuttaa tunnistetieto hajautusarvot vaatii Kerberos ja NTLM-todennusta luodaan Azure AD-Azure AD-toimialueen palvelut. Voit joko päättyy, joka käyttää Azure AD-toimialueen palveluihin tai opasta nämä käyttäjät voivat muuttaa salasanansa vuokraajan kaikkien käyttäjien salasanat.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Ota NTLM ja Kerberos tunnistetiedon hash koskevat vain pilvipalveluita Azure AD-vuokraajan
Seuraavassa on ohjeita, jotka haluat tarjota käyttäjille, jotta he voivat muuttaa salasanansa:

1. Siirry Azure AD Access-paneeli-sivulle osoitteessa [http://myapps.microsoft.com](http://myapps.microsoft.com)organisaation.

2. Valitse tällä sivulla **profiili** -välilehti.

3. Valitse tällä sivulla **Vaihda salasana** -ruutu.

    ![Luo virtuaalisia verkon Azure AD-toimialueen palveluista.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Jos Access-paneeli-sivulla **Vaihda salasana** -vaihtoehto ei ole näkyvissä, varmista, että organisaatio on määritetty [Azure AD-salasanojen hallinta](../active-directory/active-directory-passwords-getting-started.md).

4. **Vaihda salasana** -sivulle aiemmin (vanha) salasanasi ja kirjoita uusi salasana ja vahvista se. Valitse **Lähetä**.

    ![Luo virtuaalisia verkon Azure AD-toimialueen palveluista.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Kun olet muuttanut salasanan, uusi salasana on käytettävissä Azure AD-toimialueen palveluihin pian. Muutaman minuutin kuluttua (yleensä noin 20 minuuttia), voit kirjautua sisään tietokoneita hallittua toimialueen äskettäin muutetut salasanalla.

<br>

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Oman salasanan päivittäminen](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Salasanojen hallinta Azure AD-käytön aloittaminen](../active-directory/active-directory-passwords-getting-started.md).

- [Salasana-synkronointia AAD-toimialueen palveluihin synkronoidussa Azure AD-vuokraajan](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](active-directory-ds-admin-guide-administer-domain.md)

- [Liity Windows-virtual machine Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Liittyminen punainen on Enterprise Linux virtual koneen Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

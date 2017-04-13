<properties
    pageTitle="Azure AD Connect synkronointi: käynnissä ohjattu asennus toisen kerran | Microsoft Azure"
    description="Kerrotaan, miten ohjattu asennus toimii toisen kerran, se suoritetaan."
    keywords="Azure AD Connect ohjatun asennuksen avulla voit toisen kerran, se suoritetaan ylläpito-asetusten määrittäminen"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect synkronointi: käynnissä ohjattu asennus toisen kerran
Azure AD Connect ohjattu asennus ensimmäisen kerran se opastaa asennuksen määrittämisestä. Jos suoritat ohjattu asennus uudelleen, se on ylläpito vaihtoehtoja.

Löydät ohjatun asennuksen nimeltä **Azure AD Connect**Käynnistä-valikkoon.

![Käynnistä-valikko](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Kun käynnistät ohjatun asennuksen, näet sivu, jossa vaihtoehdoista:

![Sivu, jossa lisätoimien suorittamista luettelo](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Jos olet asentanut ADFS Azure AD Connect kanssa, sinun on entistä enemmän vaihtoehtoja. Sinulla on ADFS lisäasetuksia on kuvattu [ADFS hallinta](active-directory-aadconnect-federation-management.md#ad-fs-management).

Valitse jokin tehtävät ja jatka valitsemalla **Seuraava** .

> [AZURE.IMPORTANT] Kun sinulla on Avaa ohjattu asennus, kaikki toiminnot sync Engine on hyllytetty. Varmista, että voit sulkea ohjatun asennuksen heti, kun olet suorittanut määritysten muutokset.

## <a name="view-current-configuration"></a>Näytä nykyiset määritykset
Tämän asetuksen avulla voit nopeasti näkymää määritettyjen asetukset.

![Sivu, jossa on luettelo kaikkien asetuksista ja niiden tilaan](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Valitse **Edellinen** voit siirtyä takaisin. Jos valitset **Lopeta**, sulje ohjattu asennus.

## <a name="customize-synchronization-options"></a>Synkronoinnin asetusten mukauttaminen
Tämä asetus voidaan tehdä muutoksia työnkulkuun synkronointi. Näet alijoukkoa Mukautettu määritys asennuspolku-vaihtoehto. Näet tämän asetuksen, vaikka olet käyttänyt pika-asennus alun perin.

- [Lisää Lisää kansioita](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Poistaminen hakemisto-kohdassa [Poista yhdistin](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Muuta toimialueen ja OU suodatus](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Voit poistaa ryhmän suodattaminen.
- [Muuta valinnaisia ominaisuuksia](active-directory-aadconnect-get-started-custom.md#optional-features).

Muut vaihtoehdot-asennus ei voi muuttaa, ja ne eivät ole käytettävissä. Nämä asetukset ovat:

- Muuta userPrincipalName ja sourceAnchor.
- Vaihda objektien liittävä menetelmä eri metsää.
- Ota ryhmän perustuva suodatus.

## <a name="refresh-directory-schema"></a>Päivitä hakemiston rakenne
Tätä asetusta käytetään, jos olet muuttanut rakenteen yhdessä oman paikallisia AD DS: ÄÄN metsien. Esimerkiksi ehkä olet asentanut Exchange tai päivittänyt Windows Server 2012 rakenteeseen laitteen objektien kanssa. Tässä tapauksessa sinun täytyy opasta Azure AD Connect lukea AD DS: ÄÄN rakenne uudelleen ja Päivitä välimuistia. Tämä toiminto luo uudelleen myös synkronointi-sääntöjä. Jos lisäät Exchange-rakenteen esimerkkinä, Exchange synkronointi säännöt lisätään työnkulkuun.

Kun valitset tämän vaihtoehdon, näkyvät kokoonpanoa kaikki kansiot. Voit säilyttää oletusasetus ja Päivitä kaikki metsien tai poista osa.

![Sivu, jossa kaikki kansiot-ympäristössä luettelo](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Väliaikaisen tilan määrittäminen
Tämän asetuksen avulla voit käyttöön ja poistaminen käytöstä palvelimessa väliaikaisen tilassa. Lisätietoja väliaikaisen tila ja miten niitä käytetään löytyvät [toimintoja](active-directory-aadconnectsync-operations.md#staging-mode).

Vaihtoehto näkyy Jos väliaikaisen parhaillaan käytössä vai ei:  
![Vaihtoehto, joka näkyy myös nykyisen tilan väliaikaisen tila](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Jos haluat muuttaa tilan, valitse tämä vaihtoehto ja valitse tai kumoaa sen valinnan valintaruudun valinta.  
![Vaihtoehto, joka näkyy myös nykyisen tilan väliaikaisen tila](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Käyttäjien sisäänkirjautumisessa muuttaminen
Tämän asetuksen avulla voit vaihtaa salasanan synkronointi federation tai muulla tavalla ympärille. Et voi muuttaa **ei määritetä**.

Lisätietoja tämä asetus on artikkelissa [käyttäjien sisäänkirjautumisessa](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure AD Connect synkronointi [Tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md)käyttämä määritysmalli.

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

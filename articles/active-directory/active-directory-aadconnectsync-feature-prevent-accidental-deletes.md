<properties
   pageTitle="Azure AD Connect synkronointi: estää vahingossa poistaa | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan Azure AD Connect estä vahingossa poistaa (estäminen vahingossa poistot)-toiminnon."
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
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect synkronointi: Estä vahingossa poistot
Tässä ohjeaiheessa kerrotaan Azure AD Connect estä vahingossa poistaa (estäminen vahingossa poistot)-toiminnon.

Asentaminen Azure AD Connect estää vahingossa poistaa on oletusarvoisesti käytössä ja määritetty sallimaan ei vie yli 500 poistaa kanssa. Tämä toiminto on suunniteltu suojaamaan vahingossa määritys ja muutokset, jotka voivat vaikuttaa useiden käyttäjien ja muiden objektien paikallisen kansioon.

Yleiset skenaariot, kun näet sisältää useita poistaa:

- [Suodattaminen](active-directory-aadconnectsync-configure-filtering.md) , jossa koko [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) tai [toimialue](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) on valitsematon muutokset.
- Kaikissa objekteissa OU poistetaan.
- OU on nimetty uudelleen, jotta kaikki objektit siihen tiedostoilla synkronoinnin vaikutusalueen poissa.

500 objektien oletusarvon voi muuttaa PowerShellin avulla `Enable-ADSyncExportDeletionThreshold`. Määritä arvoksi organisaation koon. Koska synkronoinnin ajoitus suoritetaan 30 minuutin välein-arvo on 30 minuutin kuluessa nähdä poistaa määrä.

Jos määritettynä on liian monta poistaa Azure AD vietävän Vaiheistettu, vie pysähtyy ja näyttöön tulee sähköpostiviestin seuraavasti:

![Vahingossa poistaa estäminen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hei (tekninen yhteyshenkilö). (Aika) Identity-synkronointipalvelu havaita poistot määrä ylittää määritetyn poistaminen raja-arvon (organisaation nimi). Kokonaissumma (numero) objektien lähetettiin tämän Identity-synkronoinnin suorittaminen poistamista varten. Tämä täyttää tai ylittää määritetyn poistaminen raja-arvo (numero)-objekteja. Voit lähettää vahvistus annettava nämä poistot tulee käsitellään ennen jatkamista. Katso lisätietoja tämän sähköpostiviestissä virheen toiminnan estäminen vahingossa poistot.*

Voit myös nähdä tilan `stopped-deletion-threshold-exceeded` kun tarkastelet **Synkronoinnin palvelun hallinta** Käyttöliittymän vieminen profiilin.
![Estä vahingossa poistot synkronoinnin palvelun hallinta-Käyttöliittymä](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Jos odottamaton, tutkia ja ota korjaavat toimenpiteet. Jos haluat nähdä ollaan poistamassa objekteja, toimi seuraavasti:

1. Käynnistä **synkronointipalvelu** Käynnistä-valikosta.
2. Siirry **yhdistimiä**.
3. Valitse yhdistin sisältötyyppi **Azure Active Directory**.
4. Valitse **Toiminnot** oikealle, **Etsi yhdistimen tilaa**.
5. Valitse **laajuus**ponnahdusikkunassa Valitse **Yhteys katkaistu, koska** ja aiemman aika. Valitse **Etsi**. Tällä sivulla on kaikkien objektien ollaan poistamassa näkymän. Saat lisätietoja objekti napsauttamalla kunkin kohteen. Voit myös napsauttaa Lisää määritteitä olevan näkyvissä ruudukon **Sarakkeen asetus** .

![Etsi yhdistimen tilaa](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Tarvittaessa kaikki poistaa toimi seuraavasti:

1. Tilapäisesti käytöstä suojaus ja näiden poistaa läpi, suorita PowerShell cmdlet-komennon avulla: `Disable-ADSyncExportDeletionThreshold`. Anna Azure AD yleisen järjestelmänvalvojan tilillä ja salasana.
![Tunnistetiedot](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Valitse **suorittaa** toiminnon Azure Active Directory Connectorin ovat valittuina, ja valitse **Vie**.
3. Suojauksen käyttöön uudelleen suorittamalla PowerShell cmdlet-komennon: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Seuraavat vaiheet

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

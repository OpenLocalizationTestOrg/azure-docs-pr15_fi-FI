<properties
   pageTitle="Suojausvaroitusten määrittämisestä | Microsoft Azure"
   description="Opettele määrittämään koskevien suojausvaroitusten Azure sellaisten käyttäjätietojen hallinta-laajennus."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Suojausvaroitusten määrittämisestä Azure AD sellaisten jäsenyyksien hallinta

## <a name="security-alerts"></a>Suojausvaroitusten
Azure sellaisten käyttäjätietojen hallinta (PIM) luo ilmoituksia, kun ympäristösi on epäilyttävistä tai haitalliset toiminnan. Kun hälytyksen, se näkyy PIM koontinäytössä. Valitse Tarkastele raporttia, joka sisältää ne käyttäjät tai roolit, joilla on hälytyksen ilmoitusta.

![PIM Raporttinäkymät-ikkunan suojausvaroitusten - näyttökuva][1]



| Ilmoitus | Käynnistin | Suositus |
| ----- | ------- | -------------- |
| **Roolit määritetään PIM ulkopuolella** | Järjestelmänvalvoja on pysyvästi määritetty rooli-PIM-käyttöliittymän ulkopuolella. | Tutustu uuteen roolimääritys. Koska muiden palvelujen lisätä pysyvä Järjestelmänvalvojat, muuta se olevalla varauksen tarvittaessa. |
| **Roolien aktivoidaan liian usein** | Asetusten määräajassa on liian monta samaa roolin uudelleenaktivoinneista. | Ota käyttäjä näkee, miksi käyttäjä aktivoinut rooli niin monta kertaa. Määräajan on ehkä liian lyhenne ne niiden tehtävien suorittamiseen tai ehkä ne on käytössä komentosarjoja aktivoida automaattisesti rooli. |
| **Roolien ei vaadi monimenetelmäisen todentamisen aktivointi** | Sisältää roolia ilman MFA käytössä asetuksissa. | Olemme vaadi MFA mahdollisimman hyvin sellaisten roolit, mutta rohkaista erittäin käyttöön MFA rooleista aktivointiin. |
| **Järjestelmänvalvojat eivät ole käyttämällä sellaisten roolien** | On oikeutettu Järjestelmänvalvojat, jota ei ole aktivoitu roolien viimeksi. | Käynnistä access-tarkistus, voit määrittää käyttäjät, joita ei enää tarvita access. |
| **On liian monta Yleiset Järjestelmänvalvojat** | Lisää Yleiset järjestelmänvalvojat kuin suositellut on. | Jos sinulla on suuri määrä Yleiset Järjestelmänvalvojat, on todennäköistä, että käyttäjät jää enemmän käyttöoikeuksia kuin he tarvitsevat. Siirtää käyttäjät vähemmän sellaisten rooleille tai osaa niistä oikeuta sijaan rooli pysyvästi määritetyt. |

## <a name="configure-security-alert-settings"></a>Ilmoitusten tietosuoja-asetusten määrittäminen

Voit mukauttaa joitakin suojausvaroitusten-ympäristön ja suojan PIM. Saavuttamiseksi asetukset-sivu seuraavasti:

1. Kirjautuminen [Azure portal](https://portal.azure.com/) ja valitse koontinäyttö **Azure AD sellaisten jäsenyyksien hallinta** -ruutu.
2. Valitse **hallitut sellaisten roolien** > **asetukset** > **ilmoitusasetusten**.

    ![Ilmoitusten suojausasetukset][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Roolit aktivoidaan liian usein"-ilmoitus

Tämä ilmoitus käynnistää, jos käyttäjä aktivoi sama sellaisten rooli useita kertoja tietyn ajan kuluessa. Voit määrittää ajanjakson ja aktivointia määrä.

- **Aktivointi uusimisen ajankohta**: Määritä päivät, tunnit, minuutit ja toisen avulla voit seurata epäilyttävistä uusintoja ajanjakson.

- **Aktivointi uusintoja määrä**: määrittää minuuttimäärän, jonka aktivointia 2 100, joka ajattelet worthy ilmoituksen valitsemasi aikajakson sisällä. Voit muuttaa asetusta siirtämällä liukusäädintä tai kirjoittamalla luvun tekstiruutuun.


### <a name="there-are-too-many-global-administrators-alert"></a>"On liian monta Yleiset järjestelmänvalvojat-ilmoitus

PIM käynnistää tämä ilmoitus, jos kaksi eri ehdot täyttyvät, ja voit määrittää molemmat arvosarjat. Ensin sinun on tiettyjä Yleiset järjestelmänvalvojat raja-arvon saavuttamiseksi. Seuraavaksi prosentin yhteensä roolimäärityksiä on oltava Yleinen järjestelmänvalvojat. Jos olet vain täyttävät jonkin nämä mitat, ilmoitusta ei näy.  

- **Yleiset järjestelmänvalvojat vähimmäismäärä**: määrittää minuuttimäärän, jonka Yleiset Järjestelmänvalvojat, 2 100, harkitse haitalliset summan.

- **Yleiset järjestelmänvalvojat prosenttiosuus**: Määritä järjestelmänvalvojille, jotka järjestelmänvalvojia yleinen prosentteina, 0 %, 100 %, joka ei ole turvallinen ympäristössäsi.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"Järjestelmänvalvojat etkä käytä sellaisten roolien"-ilmoitus

Tämä ilmoitus käynnistää, jos käyttäjä aktivoimatta rooli tietyn ajanjakson aikana.

- **Päivien lukumäärä**: Määritä päivinä, 0 – 100, jotka käyttäjä voi siirtyä aktivoimatta rooli.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png

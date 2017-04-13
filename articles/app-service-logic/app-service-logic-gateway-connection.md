<properties
   pageTitle="Logiikan sovellusten paikallisen yhdyskäytävän tietoyhteyden | Microsoft Azure"
   description="Lisätietoja yhteyden paikallisen data Gateway logiikan-sovelluksesta."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Yhteyden muodostaminen paikalliseen tietojen yhdyskäytävän logiikan sovellusten

Tuetut logiikan sovellusten yhdistimien avulla voit määrittää yhteys access paikalliset tiedot paikallisen tietojen yhdyskäytävän kautta.  Seuraavat vaiheet opastaa asentaminen ja määrittäminen paikallista tietojen yhdyskäytävän logiikan-sovelluksen käyttöä varten.

## <a name="prerequisites"></a>Edellytykset

* On oltava käyttämällä on työpaikan tai oppilaitoksen sähköpostiosoitteen Azure paikallisen tietojen yhdyskäytävän liitettävä tilisi (Azure Active Directory mukaan tili)
    * Jos käytössäsi on Microsoft-Account (kuten @outlook.com, @live.com) voit luoda on työpaikan tai oppilaitoksen sähköpostiosoite noudattamalla [Tässä](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) Azure-tili

> [AZURE.WARNING] Tällä että paikallinen Gatewayn asennus kestää vain, kun käyttämällä tiliä, joka on rekisteröity Power BI rajoitus tällä hetkellä.  Tällä välin Rekisteröi tilien kanssa "Power BI vapaasti" Viimeistele asennus.

* Täytyy olla paikallisen tietojen yhdyskäytävän [paikalliseen tietokoneeseen asennettu](app-service-logic-gateway-install.md).
* Yhdyskäytävä on ei ole haettu mukaan toiseen Azure paikallisen tietojen gateway ([väittää tapahtuu vaiheen 2 ohjeiden luomisen](#2-create-an-azure-on-premises-data-gateway-resource)) - asennuksen voi olla vain yksi yhdyskäytävän resurssi liitetty.

## <a name="installing-and-configuring-the-connection"></a>Asentaminen ja määrittäminen yhteys

### <a name="1-install-the-on-premises-data-gateway"></a>1. Asenna paikallisen tietojen yhdyskäytävän

Tietoja paikallisen data gateway asennetaan löytyy [sisältö](app-service-logic-gateway-install.md).  Yhdyskäytävä on oltava asennettuna paikalliseen tietokoneeseen, ennen kuin voit jatkaa loput vaiheet.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. yritysresurssi Azure paikallisen yhdyskäytävän luominen

Kun asennettu, Azure-tilauksesi on liittämällä paikallisen tietojen yhdyskäytävän.

1. Kirjautuminen Azure käyttämällä samaa työpaikan tai oppilaitoksen sähköpostiosoite, joka on käytetty yhdyskäytävän asennuksen aikana
1. Valitse **Uusi** resurssi-painike
1. Etsi ja valitse **paikallisen tietojen yhdyskäytävän**
1. Täytä tiedot yhdyskäytävän liitettävä tilisi - myös valitsemalla haluamasi **Asennuksen nimi**

    ![Paikallinen tietojen yhdyskäytävän yhteys][1]
1. Valitse Luo resurssin **Luo** -painike

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. logiikan app yhteyden luominen suunnittelussa

Nyt kun Azure-tilauksesi on liitetty paikallisen tietojen yhdyskäytävän esiintymän, voit luoda yhteyden sen logiikan-sovelluksen sisällä.

1. Avaa logiikan-sovellus ja valitse yhdistin, joka tukee paikallisen connectivity (vuodesta tämän kirjoittaminen SQL Server)
1. Valitse valintaruutu, **Muodosta yhteys paikalliseen tietojen yhdyskäytävän kautta**

    ![Logiikan App Designer yhdyskäytävän luominen][2]
1. Valitse Muodosta yhteys ja suorita muut tarvittavat yhteystiedot **yhdyskäytävän**
1. Yhteyden **luominen**

Yhteyden pitäisi nyt olla onnistuneesti määritetty logiikan-sovelluksen käyttöä varten.  

## <a name="next-steps"></a>Seuraavat vaiheet
- [Yleisiä esimerkkejä ja tilanteita, joissa logiikan sovellukset](app-service-logic-examples-and-scenarios.md)
- [Yrityksen asiakasintegraatio-ominaisuuksia](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG
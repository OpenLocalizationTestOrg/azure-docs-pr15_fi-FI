<properties
   pageTitle="Määrittää SSL purku sovelluksen-yhdyskäytävän portaalissa | Microsoft Azure"
   description="Tällä sivulla on ohjeita voit luoda yhdyskäytävän sovelluksen SSL purku mukaan-portaalissa"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Määrittää SSL purku sovelluksen-yhdyskäytävän-portaalissa

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Resurssienhallinta PowerShellin Azure](application-gateway-ssl-arm.md)
-[Perinteinen PowerShellin Azure](application-gateway-ssl.md)

Azure sovelluksen yhdyskäytävän voidaan määrittää Lopeta välttämiseksi kallista SSL salauksen tehtävät ilmenee, WWW-klusterin yhdyskäytävässä Secure Sockets Layer (SSL)-istunto. SSL-purku yksinkertaistaa myös edusta palvelinasetukset ja web-sovelluksen hallinta.

## <a name="scenario"></a>Skenaario

Seuraava tilanne käy läpi määrittäminen SSL purku aiemmin luodun sovelluksen yhdyskäytävässä. Skenaario oletetaan, että olet jo tehnyt ohjeita voit [luoda Application Gateway](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Ennen aloittamista

Määrittää SSL purku sovelluksen gateway-varmenne on pakollinen. Tämän todistuksen on ladattu sovelluksen yhdyskäytävän ja salaaminen ja salauksen lähetetyissä kutsuissa SSL liikenne käytettävä. Varmenne on oltava henkilökohtaisten tietojen vaihtaminen (pfx)-muodossa. Tässä tiedostomuodossa avulla voidaan viedä yksityinen avain joka vaaditaan suorittamaan salausta ja salauksen liikenteen sovelluksen gateway.

## <a name="add-an-https-listener"></a>Lisää HTTPS-kuuntelutoiminnon

HTTPS-kuuntelutoiminnon näyttää sen määritysten liikenteen ja auttaa reitittää liikenteen Taustajärjestelmä jakavat.

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaaliin ja valitse aiemmin luotu sovelluksen yhdyskäytävän

### <a name="step-2"></a>Vaihe 2

Valitse kuuntelijoita ja valitse Lisää-painiketta lisätäksesi kuuntelija.

![sovelluksen yhdyskäytävän yhteenveto-sivu][1]

### <a name="step-3"></a>Vaihe 3

Täytä tarvittavat tiedot listener ja lataa .pfx-varmenne, kun olet valmis, valitse OK.

**Nimi** - arvo on kuuntelutoiminnon kutsumanimi.

**Edusta-IP-määritys** - arvoksi on frontend IP-määritys, jota käytetään kuuntelua.

**Edusta-portti (nimen/portti)** - sovelluksen yhdyskäytävän ja todellinen portin edusta-portin kutsumanimi.

**Protokolla** - valitsin, voit selvittää, jos https- tai http käytetään edustatietokanta.

**Varmenne (nimi ja salasana)** - Jos SSL purku käytetään, .pfx-varmenne, jota tarvitaan tämän asetuksen ja nimi ja salasana on pakollinen.

![Lisää listener sivu][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Luo sääntö ja liittää sen kuuntelutoiminto

Kuuntelutoiminto on luotu. Se on, kun haluat luoda säännön, joka käsittelee kuuntelua tietoliikenteen.

### <a name="step-1"></a>Vaihe 1

Sovelluksen yhdyskäytävän **säännöt** ja valitse sitten Lisää.

![sovelluksen yhdyskäytävän säännöt-sivu][3]

### <a name="step-2"></a>Vaihe 2

Kirjoita kutsumanimi säännön **Lisää basic sääntö** -sivu ja valitse edellisessä vaiheessa luotu listener. Valitse haluamasi Taustajärjestelmä resurssivarantoon ja http-asetus ja valitse **OK**

![HTTPS-asetukset-ikkunassa][4]

Asetukset nyt tallennetaan sovelluksen yhdyskäytävän. Tallenna käsitellä, näitä asetuksia voi kestää jonkin aikaa, ennen kuin ne ovat nähtävissä portaalin tai PowerShellin kautta. Kun sovellus yhdyskäytävän tallennettu käsittelee salausta ja salauksen liikenteen. Kaikki liikenne sovelluksen yhdyskäytävän ja Taustajärjestelmä web-palvelinten välillä käsitellään HTTP-yhteyden kautta. Minkä tahansa viestintä takaisin asiakkaalle, jos aloittanut https palautetaan salattu asiakkaalle.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure Application Gatewayn mukautetun kunto näytteenottimen määrittämisestä on artikkelissa [Luo mukautettu kunto näytteenottimen](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
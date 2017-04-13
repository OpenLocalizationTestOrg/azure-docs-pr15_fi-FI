<properties 
pageTitle="Käyttöönotto-Azure AM S/4 HANA tai Mustavalkoinen/4 HANA | Microsoft Azure" 
description="S/4 HANA tai Mustavalkoinen/4 HANA Azure AM-käyttöönotto" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>S/4 HANA tai Mustavalkoinen/4 HANA Microsoft Azure-käyttöönotto 

Tässä artikkelissa kuvataan Microsoft Azure kautta SAP Cloud laitteen kirjaston 3.0-S/4 HANA ottamisesta käyttöön.
Näyttökuvien näyttää prosessin vaihe vaiheelta. Käyttöönotto SAP HANA perustuva ratkaisuja, kuten Mustavalkoinen/4 HANA toimii samalla tavalla prosessin näkökulmasta. Kukaan vain Valitse eri ratkaisu.

Aloittaa SAP Cloud laitteen kirjaston (SAP-käyttö) Siirry [tähän](https://cal.sap.com/). Ei uusia [SAP Cloud laitteen kirjaston 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)tietoja SAP-blogin. 


Seuraavat näyttökuvat Näytä vaiheittaiset ottamisesta käyttöön Microsoft Azure HANA S/4. Prosessin toimii samalla tavalla kuin muiden ratkaisujen likeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Ensimmäinen kuva näyttää kaikki SAP käyttö HANA perustuvia ratkaisuja, jotka ovat käytettävissä Microsoft Azure.
Exemplarily "SAP S/4 HANA paikallisen version" prosessi kulkee valittiin (ratkaisu alalaidassa olevassa näyttökuvassa).

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

SAP-käyttö uusi tili on luotava ensin. Azure - kaksi vaihtoehtoa vakio Azure- ja Azure-kiina, kumppanien 21vianetin ylläpitämä Manner ovat tällä hetkellä.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Valitse kukaan Azure tilaus-tunnus, joka löytyy Azure-portaalissa – Katso myös muita hankkiminen alaspäin. Jälkeenpäin Azure hallinta-varmenteen on voitu ladata.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

Uusi Azure-portaalin yksi etsii "Tilaukset" vasemmassa reunassa. Voit näyttää kaikki käyttäjän active tilausten napsauttamalla sitä.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Valitsemalla jokin tilauksista ja valitsemalla sitten "Hallinta varmenteet" selitetään, joka on on uusi käsite, käyttämällä "palvelun ansaitun" Azure Resurssienhallinta uuden mallin.
SAP-käyttö ei ole vielä sovittamaan uusi malli, ja se vaatii edelleen "perinteinen" mallia ja entisen Azure-portaalin hallinta varmenteet-käyttöä varten.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Tässä jokin näkyvät entisen Azure portaalin. Lataa hallinta-varmenteen antaa SAP käyttö oikeudet luoda asiakas-tilauksen piiriin kuuluvien näennäiskoneiden. Valitse "Tilaukset-välilehdessä jokin löytää tilauksen tunnus, jolla on kirjoitettava SAP käyttö-portaalissa.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Toista-välilehdessä on sitten mahdollista ladata hallinta-varmenne, jota on ladattujen ennen SAP-käyttö.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Valitse ladatut sertifikaattitiedosto tulee pieni valintaikkuna.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Kun varmenne on ladattu palvelimeen SAP käyttö ja asiakkaan Azure tilauksen välinen yhteys voidaan testata sisällä SAP-käyttö. Pieni viestin tulee ponnahdusikkuna joka kertoo, että yhteys on kelvollinen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Tilin asennuksen jälkeen kukaan Valitse ratkaisu, joka voidaan ottaa käyttöön ja luoda esiintymää.
"Perustiedot"-tilassa, jossa on todella trivial. Anna esiintymänimi, valitse Azure alue ja voit määrittää ratkaisun perusmuodon salasanan.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Hetken kuluttua sen mukaan, koosta ja monimutkaisuudesta (arvio annetaan SAP käyttö) ratkaisu on osoitettu "aktiivinen" ja valmis käytettäväksi. On hyvin helppoa.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Jotkin tarkastelu-ratkaisun jokin näkee VMs millainen on otettu käyttöön. Tässä tapauksessa kolme Azure VMs erikokoisia ja tarkoitus on luotu.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Azure-portaalissa näennäiskoneiden löytyy esiintymän nimi, joka on määritetty SAP käyttö alkaen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

On nyt mahdollista yhteyden muodostaminen SAP käyttö portaalissa Muodosta-painiketta painamalla ratkaisu. Pieni valintaikkunan sisältää linkin käyttöoppaassa, joka kuvaa kaikki oletusarvon toimimaan ratkaisun tunnistetiedot.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Asiakkaan Windows AM kirjautua sisään ja Käynnistä esimerkiksi ennalta määritetyn SAP-Graafisen on jokin muu vaihtoehto.








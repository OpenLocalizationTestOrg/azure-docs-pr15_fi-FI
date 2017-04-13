<properties 
pageTitle="SAP IDES EHP7 SP3: n käyttöönotto-Microsoft Azure ERP SAP 6.0 for | Microsoft Azure" 
description="Microsoft Azure-ERP SAP 6.0 for SAP IDES EHP7 SP3 käyttöönotto" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>Microsoft Azure-ERP SAP 6.0 for SAP IDES EHP7 SP3 käyttöönotto 

Tässä artikkelissa käsitellään käyttöön SAP IDES käytössä SQL Server-ja Windows-Käyttöjärjestelmää Microsoft Azure SAP Cloud laitteen kirjaston 3.0 kautta. Näyttökuvien näyttää prosessin vaihe vaiheelta. Käyttöönotto-luettelossa ratkaisuja toimii samalla tavalla prosessin näkökulmasta. Kukaan vain Valitse eri ratkaisu.

Aloittaa SAP Cloud laitteen kirjaston (SAP-käyttö) Siirry [tähän](https://cal.sap.com/). Ei uusia [SAP Cloud laitteen kirjaston 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)tietoja SAP-blogin. 


Seuraavat näyttökuvat Näytä vaiheittaiset SAP IDES Microsoft Azure-ottamisesta käyttöön. Prosessin toimii samalla tavalla kuin muita ratkaisuja.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Ensimmäinen kuva näyttää kaikki ratkaisuja, jotka ovat käytettävissä Microsoft Azure. Korostettu Windows-pohjaisesta SAP IDES-ratkaisun, joka on käytettävissä Azure vain valittiin prosessi kulkee.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

SAP-käyttö uusi tili on luotava ensin. Azure - kaksi vaihtoehtoa vakio Azure- ja Azure-kiina, kumppanien 21vianetin ylläpitämä Manner ovat tällä hetkellä.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Valitse kukaan Azure tilaus-tunnus, joka löytyy Azure-portaalissa – Katso myös muita hankkiminen alaspäin. Jälkeenpäin Azure hallinta-varmenteen on voitu ladata.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

Uusi Azure-portaalin yksi etsii "Tilaukset" vasemmassa reunassa. Voit näyttää kaikki käyttäjän active tilausten napsauttamalla sitä.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Valitsemalla jokin tilauksista ja valitsemalla sitten "Hallinta varmenteet" selitetään, joka on on uusi käsite, käyttämällä "palvelun ansaitun" Azure Resurssienhallinta uuden mallin.
SAP-käyttö ei ole vielä sovittamaan uusi malli, ja se vaatii edelleen "perinteinen" mallia ja entisen Azure-portaalin hallinta varmenteet-käyttöä varten.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Tässä jokin näkyvät entisen Azure portaalin. Lataa hallinta-varmenteen antaa SAP käyttö oikeudet luoda asiakas-tilauksen piiriin kuuluvien näennäiskoneiden. Valitse "Tilaukset-välilehdessä jokin löytää tilauksen tunnus, jolla on kirjoitettava SAP käyttö-portaalissa.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Toista-välilehdessä on sitten mahdollista ladata hallinta-varmenne, jota on ladattujen ennen SAP-käyttö.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Valitse ladatut sertifikaattitiedosto tulee pieni valintaikkuna.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Kun varmenne on ladattu palvelimeen SAP käyttö ja asiakkaan Azure tilauksen välinen yhteys voidaan testata sisällä SAP-käyttö. Pieni viestin tulee ponnahdusikkuna joka kertoo, että yhteys on kelvollinen.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Tilin asennuksen jälkeen kukaan Valitse ratkaisu, joka voidaan ottaa käyttöön ja luoda esiintymää.
"Perustiedot"-tilassa, jossa on todella trivial. Anna esiintymänimi, valitse Azure alue ja voit määrittää ratkaisun perusmuodon salasanan.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Hetken kuluttua sen mukaan, koosta ja monimutkaisuudesta (arvio annetaan SAP käyttö) ratkaisu on osoitettu "aktiivinen" ja valmis käytettäväksi. On hyvin helppoa.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Jotkin tarkastelu-ratkaisun jokin näkee VMs millainen on otettu käyttöön. Tässä tapauksessa on yhden yksittäisen Azure-AM koon D12, joka on luotu SAP-käyttö.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Azure-portaalissa virtuaalikoneen löytyy esiintymän nimi, joka on määritetty SAP käyttö alkaen.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

On nyt mahdollista yhteyden muodostaminen SAP käyttö portaalissa Muodosta-painiketta painamalla ratkaisu. Pieni valintaikkunan sisältää linkin käyttöoppaassa, joka kuvaa kaikki oletusarvon toimimaan ratkaisun tunnistetiedot.
[Tässä](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) on linkki IDES-ratkaisun-oppaassa.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Toinen vaihtoehto on Windows-AM kirjautua sisään ja Käynnistä esimerkiksi ennalta määritetyn SAP-Graafisen.






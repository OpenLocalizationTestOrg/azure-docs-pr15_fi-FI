<properties
    pageTitle="Azure AD-toimialueen palveluista: Luo tai valitse virtual verkossa | Microsoft Azure"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Luo tai valitse virtual verkon Azure AD-toimialueen palveluista

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Valitse Azure virtual verkon ohjeita
> [AZURE.NOTE] **Ennen aloittamista**: viitata [Verkko Huomioitavaa Azure AD-toimialueen palveluista](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Tehtävä 2: Azure virtual verkoston luominen
Seuraava määritys tehtävä on luotava Azure virtual verkko- ja sen sisältämät aliverkon. Voit ottaa tämän aliverkon Azure AD-toimialueen palvelut virtual verkossa oleville. Jos sinulla on jo olemassa olevan virtual verkoston haluat käyttää, voit ohittaa tämän vaiheen.

> [AZURE.NOTE] Varmista, että Azure virtual verkoston luominen tai valita Azure AD-toimialueen palvelujen käyttämällä kuuluu Azure alue, jota tuetaan Azure AD-toimialueen palveluista. On tiedettävä Azure alueet, joiden Azure AD-toimialueen palveluista on käytettävissä [alueittain Azure palvelut](https://azure.microsoft.com/regions/#services/) -sivulla.

Huomautus alaspäin virtual verkon nimen, niin valitset oikealle virtual verkon kun otat käyttöön Azure AD-toimialueen palveluista myöhemmin määritys vaiheessa.

Suorita seuraavat määritysvaiheet, jossa haluat ottaa käyttöön Azure AD-toimialueen palveluista Azure virtual verkoston luominen.

1. Siirry **Azure perinteinen portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Valitse vasemmanpuoleisessa ruudussa **verkot** -solmu.

    ![Verkkojen solmu](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Valitse **Uusi** sivun alareunassa-tehtäväruudussa.

    ![Virtuaalinen verkkojen solmu](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Valitse **Verkkopalvelut** -solmu **VPN**.

5. Valitse virtual verkoston luominen **Nopea luominen** .

    ![Virtuaalinen verkko - nopea luominen](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Määritä virtual verkon **nimi** . Voit halutessasi myös määrittämiseksi verkoston **osoitetilaa** tai **Suurin AM määrä** . Voit jättää **DNS-palvelimeen** -asetus, ' ei mitään ' nyt. Voit päivittää DNS-palvelimen määrittäminen sen jälkeen, ota yhteyttä Azure AD-toimialueen palvelut.

7. Varmista, että valitset tuetut Azure alueen **sijainti** avattavasta valikosta. On tiedettävä Azure alueet, joissa Azure AD-toimialueen palveluista on saatavilla [alueittain Azure palvelut](https://azure.microsoft.com/regions/#services/) -sivulla.

8. Voit luoda virtuaalisen verkon valitsemalla **Luo virtuaalisia verkko** -painike.

    ![Luo virtuaalisia verkon Azure AD-toimialueen palveluista.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Virtual verkon luomisen jälkeen Valitse virtual verkko ja **Määritä** -välilehti.

    ![Luo aliverkon](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Siirry osaan, **VPN-osoitteen välilyöntejä** . Valitse **Lisää aliverkon** ja määritä aliverkon nimellä **AaddsSubnet**. Valitse Luo aliverkon **Tallenna** .

    ![Luo aliverkon Azure AD-toimialueen palveluista.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Tehtävä 3 – Ota käyttöön Azure AD-toimialueen palveluista
Seuraava määritys tehtävä on ottaa [käyttöön Azure AD-toimialueen palveluista](active-directory-ds-getting-started-enableaadds.md).

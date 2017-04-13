<properties
    pageTitle="Luo Azure-portaalissa Linux-AM | Microsoft Azure"
    description="Luo Linux-AM Azure-portaalissa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Luo Linux AM Azure-portaalissa


Tässä artikkelissa kerrotaan, miten [Azure portal](https://portal.azure.com/) avulla voit luoda Linux-Virtual Machine.

Vaatimukset ovat seuraavat:

- [Azure-tili](https://azure.microsoft.com/pricing/free-trial/)

- [SSH avaimen julkisista ja yksityisistä-tiedostot](virtual-machines-linux-mac-create-ssh-keys.md)


1. Kirjautunut Azure portaaliin henkilöllisyytesi Azure-tili, valitse **+ Uusi** vasemmassa yläkulmassa:

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Napsauta **näennäiskoneiden** **Marketplace** **Ubuntu palvelimen 14.04 l.** **Ajankohtaiset sovellukset** kuvien luetteloon.  Varmista, että käyttöönottomalli on alareunassa `Resource Manager` ja valitse sitten **Luo**.

    ![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Anna **Perustiedot** -sivulla:
    - AM nimi
    - Järjestelmänvalvoja-käyttäjän käyttäjänimi
    - Todennustyyppi asettaminen **SSH julkinen avain**
    - SSH-julkinen avain merkkijonona (-että `~/.ssh/` hakemisto)
    - resurssin ryhmän nimi tai valitse aiemmin luotuun ryhmään

    ja valitse **OK** , jos haluat jatkaa ja AM; koon valitseminen se pitäisi näyttää seuraavanlaiselta:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Valitse **DS1** kokoa, joka voidaan asentaa Ubuntu Premium Suoritettaessa, ja valitse **Valitse** asetusten määrittäminen.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. **Asetukset**jätä säilytys- ja arvot oletusarvot ja valitse **OK** , jos haluat tarkastella yhteenvetotietoja.  Ilmoitus levy on määritetty Premium Suoritettaessa valitsemalla DS1, **S** notates Suoritettaessa.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Vahvista uusi Ubuntu-AM asetukset ja valitse **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Avaa Portal Raporttinäkymät-ikkunan ja valitse **verkkoliittymät** lisääminen NIC: ssä

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Avaa NIC-asetukset-kohdassa julkiseen IP-osoitteet-valikko

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH kyselyjä julkiseen IP käyttämällä SSH-julkinen avain

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt luonut Linux-AM nopeasti, jota käytetään testaaminen tai esittelyä. Voit luoda Linux AM mukautetut infrastruktuurin, voit seurata mitä tahansa näistä artikkeleista.

- [Luo Linux AM Azure mallien käyttäminen](virtual-machines-linux-cli-deploy-templates.md)
- [Luo SSH suojattu Linux AM Azure mallien käyttäminen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Luo Linux-AM Azure-CLI käyttäminen](virtual-machines-linux-create-cli-complete.md)

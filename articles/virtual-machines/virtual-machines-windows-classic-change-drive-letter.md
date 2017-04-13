<properties
    pageTitle="Tee AM tietojen DVD-levyllä d-asema | Microsoft Azure"
    description="Tässä artikkelissa käsitellään muuttaminen Windowsin AM asemakirjaimia, niin, että voit käyttää tietojen asema d-asema."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Käytä Windowsin AM tietojen levyaseman d-asema 

Jos sovellus täytyy D-aseman avulla voit tallentaa tiedot, käyttää toinen kirjain tilapäinen levyn näiden ohjeiden mukaisesti. Ei koskaan tallentaa tiedot, jotka haluat säilyttää tilapäinen levyn avulla.

Jos kokoa on muutettu tai **Pysäytä (Deallocate)** virtual machine, tämä voi käynnistää, uusi hypervisor virtuaalikoneen sijaintia. Suunniteltu tai suunnittelematon ylläpitotapahtuman voi käynnistää myös tämän sijainnin. Tässä skenaariossa tilapäinen levyn uudelleen vapaana merkkijonon ensimmäistä kirjainta. Jos jokin sovellus, joka edellyttää erityisesti d-asema, sinun on väliaikaisesti siirtää pagefile.sys, Liitä uudet tiedot DVD-levyllä ja määrittää sen D-kirjainta ja siirtää pagefile.sys takaisin tilapäiset aseman seuraavasti. Kun valmiina, Azure otetaan käyttöön takaisin D: Jos AM siirtyy eri hypervisor.

Lisätietoja siitä, miten Azure käyttää tilapäinen levy on kohdassa [tietoa tilapäinen asema, Microsoft Azuren näennäiskoneiden](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Liitä tiedot levy

Ensin sinun on liitettävä tietojen levyn virtuaalikoneen. 

- Käyttää portaalia, katso, [miten voit liittää tiedot DVD-levyllä Azure-portaalissa](virtual-machines-windows-attach-disk-portal.md)
- Perinteinen portaalissa on artikkelissa [liittäminen tietojen DVD-levyllä Windows virtual koneeseen](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Siirrä tilapäisesti pagefile.sys C-asema

1. Muodosta yhteys virtuaalikoneen. 

2. Napsauta hiiren kakkospainikkeella **Käynnistä** -valikko ja valitse **Järjestelmä**.

3. Valitse vasemmalla olevasta valikosta **järjestelmän Lisäasetukset**.

4. Valitse **Suorituskyky** -kohdasta **asetukset**.

5. Valitse **Lisäasetukset** -välilehti.

5. Valitse **Muuta** **näennäismuistin** -osiossa.

6. Valitse **C** -asema ja valitsemalla sitten **järjestelmä määrittää koon** ja valitse sitten **Määritä**.

7. Valitse **D** -aseman ja valitse **sivutustiedostoa ei** ja valitse sitten **Määritä**.

8. Valitse Käytä. Näyttöön tulee ilmoitus, että tietokone on käynnistettävä uudelleen, jotta muutokset tulevat voimaan.

9. Käynnistä virtuaalikoneen.




## <a name="change-the-drive-letters"></a>Muuta asemakirjaimet 

1. Kun AM käynnistetään uudelleen, kirjaudu takaisin sisään AM.

2. Napsauta **Käynnistä** -painiketta ja kirjoita **diskmgmt.msc** ja Enter-näppäintä. Levyn alkaa.

3. **D**-väliaikaisten-levylle, napsauta hiiren kakkospainikkeella ja valitse **Muuta asemakirjain ja polkuja**.

4. Valitse asemakirjain Valitse **G** -asema ja valitse sitten **OK**. 

5. Tietoja levyn hiiren kakkospainikkeella ja valitse **Muuta-kirjain ja polku**.

6. Valitse asemakirjain Valitse **D** -asemassa ja valitse sitten **OK**. 

7. **G**, väliaikaisten-levylle, napsauta hiiren kakkospainikkeella ja valitse **Muuta asemakirjain ja polkuja**.

8. Valitse asemakirjain Valitse **E** -asema ja valitse sitten **OK**. 

> [AZURE.NOTE] Jos yhteyttä AM on muiden levyjen tai asemat, käytä samaa menetelmää voit määrittää asemakirjaimet levyille ja asemat. Haluat olevan levyn asetuksia:  
>- C: OS levy  
>- D: tietojen levy  
>- E tilapäinen levy



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Siirrä pagefile.sys takaisin väliaikaisten-asema 

1. Napsauta hiiren kakkospainikkeella **Käynnistä** -valikko ja valitse **Järjestelmä**

2. Valitse vasemmalla olevasta valikosta **järjestelmän Lisäasetukset**.

3. Valitse **Suorituskyky** -kohdasta **asetukset**.

4. Valitse **Lisäasetukset** -välilehti.

5. Valitse **Muuta** **näennäismuistin** -osiossa.

6. Valitse OS **C** -asema ja **sivutus ei ole** tiedosto ja valitse **Aseta**.

7. Valitse väliaikaisten asema **E** ja valitse **järjestelmä määrittää koon** ja valitse sitten **Määritä**.

8. Valitse **Käytä**. Näyttöön tulee ilmoitus, että tietokone on käynnistettävä uudelleen, jotta muutokset tulevat voimaan.

9. Käynnistä virtuaalikoneen.




## <a name="next-steps"></a>Seuraavat vaiheet
- Voit suurentaa käytettävissä tallennustilan virtuaalikoneen [liittäminen lisätietojen DVD-levyllä](virtual-machines-windows-attach-disk-portal.md).




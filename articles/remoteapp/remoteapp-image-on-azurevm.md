<properties
    pageTitle="Luo Azure-AM perusteella Azure RemoteApp kuvan | Microsoft Azure"
    description="Opi luomaan Azure RemoteApp kuvan käyttäen Azure virtuaalikoneen."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Luo Azure virtuaalikoneen perusteella Azure RemoteApp kuva

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Voit luoda Azure virtuaalikoneen Azure RemoteApp-kuvat (joka pidä jaat kokoelmasta sovellukset). Voi myös valita Käytä virtuaalikoneen kuvaa on lisätty Azure AM kuvavalikoima, joka vastaa kaikkia Azure RemoteApp kuva – voit käyttää AM kuvan lähtökohtana oman AM, voit halutessasi. Valitsemisen kirjastossa "Windows Server Remote Desktop istunnon isäntään"-kuva.

Sisältää kaksi vaihetta, voit luoda oman kuvan perusteella Azure-AM - kuvankäsittelyohjelmalla ja lataa se sitten Azure AM-kirjastosta, Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Azure-AM perusteella mukautetun kuvan luominen

Näiden ohjeiden avulla voit luoda kuvan Azure-AM perusteella.

1. Luo Azure virtuaalikoneen. Voit käyttää "Windows Server Remote Desktop istunnon isäntään" tai "Windows Server Remote työpöydän istunnon isännän kanssa Microsoft Office 365 ProPlus" kuvan Azure virtuaalikoneen kuva valikoimasta. Tämä kuva täyttää kaikki Azure RemoteApp mallin kuva vaatimukset.

    Lisätietoja on artikkelissa [Create AM-käyttöjärjestelmässä](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Yhteyden muodostaminen AM ja asenna ja määritä sovellukset, jotka haluat jakaa RemoteApp kautta. Varmista, että voit suorittaa muita Windows-määritykset, vaatii sovellukset.

    Lisätietoja on artikkelissa [miten Kirjaudu virtuaalikoneen käytössä Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Jos käytössäsi on jokin Windows Server Remote Desktop istunnon isäntään kuvista, ei sisällytetä vahvistus-komentosarjan, jolla varmistetaan, että AM täyttää RemoteApp pre-reqs. Voit suorittaa komentosarjan kaksoisnapsauttamalla **ValidateRemoteAppImage** työpöydälle. Varmista, että kaikki virheet ilmoittaa komentosarja on korjattu ennen seuraavaan vaiheeseen jatkamista.

4. SYSPREP generalize ja sieppaa kuva. Katso, [miten voit siepata Windows-Virtual Machine käyttöön mallina](../virtual-machines/virtual-machines-windows-classic-capture-image.md) ohjeita.



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Kuvan tuominen Azure RemoteApp kuvakirjastossa

Voit tuoda uusi kuva Azure RemoteApp seuraavasti:

1. **Sivustomallien kuvia** -välilehdessä:
    - Jos sinulla ei ole olemassa olevat kuvat, valitse **ladata tai tuoda suunnittelumallin kuva**.
    - Jos sinulla jo on vähintään yksi kuva, valitse **+** Lisää uusi kuva.

2. Valitse **Tuo oman näennäiskoneiden kuva** kirjasto ja valitse sitten **Seuraava**.

3. Valitse seuraavalla sivulla luettelosta mukautetun kuvan ja Vahvista ja vaiheiden kuvan luomisen jälkeen. Valitse **Seuraava**.
4. Uuden RemoteApp kuvan nimi ja valitse sijainti ja valitse sitten Aloita tuonti valintamerkki.

> [AZURE.NOTE] Voit tuoda kuvia tuettu mukaan Azuren näennäiskoneiden Azure RemoteApp tukemat Azure milloin Azure milloin. Sen mukaan, sijainnit tuonti voi kestää enintään 25 minuuttia.

Nyt olet valmis luomaan uuden kokoelmaa joko [cloud](remoteapp-create-cloud-deployment.md) kokoelman tai [hybrid](remoteapp-create-hybrid-deployment.md), tarpeidesi.

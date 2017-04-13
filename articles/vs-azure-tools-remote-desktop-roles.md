<properties 
   pageTitle="Etätyöpöytä käyttäminen Azure roolit | Microsoft Azure"
   description="Etätyöpöytä käyttäminen Azure roolit"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Etätyöpöytä käyttäminen Azure roolit

Azure SDK ja Etätyöpöytäpalvelut avulla voit käyttää Azure roolit ja näennäiskoneiden, joita isännöidään Azure mukaan. Voit määrittää Visual Studion Etätyöpöytäpalvelut Azure Projectista. Etätyöpöytäpalvelut käyttöön, joka sisältää vähintään yhden roolit työn projektin luominen ja julkaiseminen Azure.

>[AZURE.IMPORTANT] Kannattaa käyttää Azure rooli, vianmääritys tai vain kehittäminen. Kunkin virtuaalikoneen tarkoituksena on suorittaa tietyn roolin Azure sovelluksessa, et voi suorittaa muita asiakassovelluksissa. Jos haluat isännöidä virtual machine, joiden avulla voit tarkoituksiin Azure avulla, katso käyttäminen Azuren näennäiskoneiden-valintaruudun Server Explorerista.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Voit ottaa käyttöön ja Azure-roolin Etätyöpöydän käyttäminen

1. Napsauta ratkaisunhallinnassa Avaa pikavalikko projektin ja valitse sitten **Julkaise**.

    Ohjattu **Julkaiseminen Azure-sovellus** tulee näkyviin.

    ![Komennon pilvipalvelussa projektin julkaiseminen](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. Ohjatun toiminnon sivulla **Microsoft Azure Julkaisuasetukset** alareunassa Valitse **Ota käyttöön etätyöpöytä** kaikki roolit-valintaruudun valinta. 

    **Remote Desktop-määritys** -valintaikkuna.

1. Valitse **Remote Desktop Configuration** -valintaikkunan alareunassa **Lisää asetuksia** -painiketta. 
 
    Tämä näyttää avattavan luetteloruudun, jolla voit luoda tai valitse varmenne, jotta voit salata tunnistetiedot muodostettaessa etätyöpöydän kautta.

1. Valitse avattavasta luettelosta ** &lt;Luo >**, tai valitse olemassa olevasta luettelosta. 

    Jos valitset varmenne, voit ohittaa seuraavien ohjeiden mukaisesti.

    >[AZURE.NOTE] Sertifikaatteja, joita tarvitset Etätyöpöytäyhteys poikkeavat sertifikaatteja, joita voit käyttää Azure muihin toimintoihin. Etäkäyttö varmenteella on oltava yksityinen avain.

    **Luo sertifikaatti** -valintaikkuna.

    1. Anna uuden sertifikaatin nimi ja valitse sitten **OK** -painiketta. Uusi sertifikaatti näkyy avattavassa luettelossa.

    1. Ole **Remote Desktop-määritys** -valintaikkunassa käyttäjänimen ja salasanan.
    
        Et voi käyttää aiemmin luotu tili. Järjestelmänvalvoja määrittää uuden tilin käyttäjänimi ei.

        >[AZURE.NOTE] Jos salasana ei täytä monimutkaisuuden, salasanan tekstiruudun vieressä olevaa punaista-kuvaketta. Salasana on oltava isoilla kirjaimilla, pieniä kirjaimia ja numeroita tai symboleja.

    1. Valitse päivämäärän, johon tili vanhenee ja sen jälkeen mitä työpöydän etäyhteyksien estetään.

    1. Kun määritetty kaikki tarvittavat tiedot, valitse **OK** -painiketta.
    
        Useita asetuksia, jotka mahdollistavat Remote Access Services lisätään .cscfg ja .csdef tiedostot.

1. Valitse ohjatussa **Microsoft Azure Julkaisuasetukset** **OK** -painiketta, kun olet valmis julkaisemaan pilvipalvelussa sijaitsevaan.

    Jos et ole valmis julkaisemaan, valitse **Peruuta** -painiketta. Asetukset tallennetaan, ja voit julkaista pilvipalvelussa sijaitsevaan myöhemmin.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Yhteyden muodostaminen Azure-roolin Etätyöpöydän avulla

Kun julkaiset Azure cloud-palvelussa, voit kirjautua näennäiskoneiden Azure isännöivä palvelimen Resurssienhallinnan avulla. 

1. Palvelimen Resurssienhallinnassa Laajenna **Azure** -solmu ja laajenna sitten solmu pilvipalveluun ja toinen sen saat näkyviin luettelon esiintymien roolit.

1. Avaa pikavalikon esiintymä-solmu ja valitse sitten **Yhdistä käyttäen etätyöpöydän kautta**.

    ![Yhteyden muodostaminen etätyöpöydän kautta](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Kirjoita käyttäjänimi ja salasana, jonka loit aiemmin. Olet kirjautuneena nyt yhteyttä etäistunnon.



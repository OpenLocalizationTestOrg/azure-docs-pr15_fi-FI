<properties 
   pageTitle="Käytettäessä yksityinen Azure paveikslėlis Visual Studiossa | Microsoft Azure"
   description="Opettele yksityinen cloud resursseihin Visual Studion avulla."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Yksityinen Azure paveikslėlis Visual Studiossa käyttäminen

##<a name="overview"></a>Yleiskatsaus

Oletusarvon mukaan Visual Studio tukee julkisen Azure cloud muiden päätepisteet. Tämä voi olla ongelma, kuitenkin, jos käytössäsi on Visual Studio yksityinen Azure pilvestä. Varmenteiden avulla voit määrittää Visual Studio käyttämään yksityinen Azure cloud muiden päätepisteet. Saat nämä varmenteet oman Azure – Julkaise asetustiedosto.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Voit käyttää yksityinen Azure cloud Visual Studiossa

1. [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885) for yksityinen pilveen Lataa Julkaise asetustiedosto tai ota yhteyttä järjestelmänvalvojaan Julkaise asetustiedosto. Azure julkisen versiota Lataa tämä linkki on [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Voit ladata tiedoston pitäisi olla tunniste on .publishsettings.)

1. **Palvelimen Explorer** Visual Studiossa Valitse **Azure** -solmu ja hiiren kakkospainikkeella ja valita **Tilausten hallinta** -komento.

    ![Hallitse tilaukset-komento](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. **Microsoft Azure-tilausten hallinta** -valintaikkunassa valitse **Varmenteet** -välilehti ja valitse sitten **Tuo** -painike.

    ![Azure varmenteiden tuominen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. **Tuominen Microsoft Azure tilaukset** -valintaikkunassa Selaa kansioon, johon voit tallentaa Julkaise-asetustiedosto ja tiedoston, ja valitse sitten **Tuo** -painike. Julkaise-asetustiedosto varmenteet tuodaan Visual Studio. Pitäisi nyt olla vuorovaikutuksessa yksityinen cloud resurssit.

    ![Tuominen Julkaisuasetukset](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Seuraavat vaiheet

[Visual Studio Azure pilvipalvelussa julkaiseminen](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Toimintaohje: Lataa ja tuo asetukset ja Tilaustiedot julkaiseminen](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)


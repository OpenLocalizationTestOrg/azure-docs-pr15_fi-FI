<properties
   pageTitle="Tekniset edellytyksistä virtuaalikoneen näköistiedoston luominen Azure Marketplacen | Microsoft Azure"
   description="Tietoja luominen ja käyttöönotto virtuaalikoneen kuva Azure Marketplacesta muiden ostaa koskevat vaatimukset."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Tekniset edellytyksistä luominen Azure Marketplacen virtuaalikoneen kuva
Lue huolellisesti ennen kuin aloitat prosessin ja ymmärtää, jossa ja miksi jokaisen vaiheen suoritetaan. Mahdollisimman hyvin, sinun olisi yritystietojen ja muiden tietojen valmisteleminen, lataa tarvittavat työkalut ja/tai luo tekniset osat ennen aloittamista tarjouksen luontiprosessi. Nämä kohteet on oltava Tyhjennä-tarkistanut ohjeaiheen.  

## <a name="download-needed-tools--applications"></a>Lataa tarvittavat työkalut ja sovellukset
Sinulla on oltava valmiina ennen aloittamista prosessin seuraavia kohteita:

- Sen mukaan, mitä kohdennat käyttöjärjestelmää asentaa [Azure PowerShellin cmdlet-komennot](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) tai [Linux käyttöliittymä työkalun](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) [Azure](https://azure.microsoft.com/downloads/) -lataussivulta.
- Asenna Azure tallennustilan Explorer CodePlex.
- Lataa ja asenna varmenteiden testi-työkalua Azure sertifioitu:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Sinun on Windows-tietokoneella sertifikaatin-työkalun suorittaminen. Jos sinulla ei ole Windows-tietokoneella käytettävissä, voit suorittaa käyttämällä Windows-pohjaisesta AM Azure-työkalu.

## <a name="platforms-supported"></a>Tukemat Ympäristöt
Voit kehittää Azure-pohjaiset VMs Windowsin tai Linuxissa. Joitakin osia julkaisuprosessin – kuten luominen Azure-yhteensopiva virtual kiintolevyn (Näennäiskiintolevyn) – eri työkalujen ja ohjeet käytät käyttöjärjestelmän mukaan:  

- Jos käytät Linux, viittaa [virtuaalikoneen kuva julkaisun opas](marketplace-publishing-vm-image-creation.md)"Luominen (Linux-pohjainen) Azure-yhteensopiva-Näennäiskiintolevyn"-osio.
- Jos käytössäsi on Windows-viitata [virtuaalikoneen kuva julkaisun opas](marketplace-publishing-vm-image-creation.md)"Luominen (Windows-pohjainen) Azure-yhteensopiva-Näennäiskiintolevyn"-osio.

> [AZURE.NOTE] Sinun on käyttää Windows-pohjaisesta tietokonetta,:
- Sertifikaatin vahvistus-työkalun suorittaminen.
- Luoda jaetun Näennäiskiintolevyn access allekirjoituksen URL-Osoitteen Näennäiskiintolevyn sertifikaatin lähettämistä varten.

## <a name="develop-your-vhd"></a>Oman Näennäiskiintolevyn kehittäminen
Voit kehittää Azure näennäiskiintolevyjen cloud tai-ympäristöön:

- Pilvipohjainen kehitys tarkoittaa kehittäminen vaiheiden suorittamisesta etäyhteyden säilöttyihin asuvat Azure Näennäiskiintolevyn.
- Paikallisen kehittäminen edellyttää lataaminen Näennäiskiintolevyn ja kehittää käyttämällä paikallisen infrastruktuuria. Tämä on mahdollista, mutta suosittelemme ei sen. Huomaa, että kehittäminen Windows-tai SQL-ympäristöön edellyttää, että asiaa paikallisen käyttöoikeuden näppäimet. Ei voi lisätä tai asentaa SQL Serveriä AM luomisen jälkeen. Tarjous on perustuvan hyväksytyn SQL Azure-portaalista kuva. Jos päätät kehittää paikallisen, sinun täytyy suorittaa muutama eri tavalla kuin on kehittäminen pilveen. Löydät tarvittavat tiedot Create [paikallisen AM kuva](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet tarkistanut edellytykset ja valmiit tarvittavat tehtävät, voit siirtää eteenpäin luomisessa virtuaalikoneen kuva tarjous yksilöityjä [virtuaalikoneen kuva julkaisun opas](marketplace-publishing-vm-image-creation.md).

## <a name="see-also"></a>Katso myös
- [Aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
- [Windows Azure esikatselu-portaalissa on virtual koneen luominen](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md

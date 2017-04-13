<properties
    pageTitle="Tietyt Azure VMs RDP virhesanomat | Microsoft Azure"
    description="Tietoja virheilmoituksista, näyttöön voi tulla, kun yrität käyttää Etätyöpöytäyhteys Windows virtual koneeseen Azure-tietokannassa"
    keywords="Remote desktop-virhe, virhe Etätyöpöytäyhteys ei voi muodostaa yhteyttä AM, Etätyöpöydän vianmääritys"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Tietyt RDP virhesanomat Windows-AM Azure-tietokannassa, vianmääritys
Näyttöön voi tulla virhesanoma, käytettäessä Etätyöpöytäyhteys Windows virtual machine (AM) Azure-tietokannassa. Tämän artikkelin tiedot joitakin tavallisimmista virhesanomista havaitsi sekä niiden ratkaisemiseksi vianmääritysvaiheita. Jos on vaikeuksia muodostaa yhteyttä AM käyttämällä RDP ei havaitse virhesanomasta, on artikkelissa [vianmääritysoppaan etätyöpöydän](virtual-machines-windows-troubleshoot-rdp-connection.md).

Lisätietoja virheilmoituksista on seuraavissa artikkeleissa:

- [Etäistunto katkesi, koska ei ole Remote työpöydän palvelimet voi antaa käyttöoikeuden](#rdplicense).
- [Etätyöpöytä ei löydä tietokoneen "nimi"](#rdpname).
- [Todennus on virhe. Paikallinen suojaus viranomainen ei voi muodostaa yhteyttä](#rdpauth).
- [Windowsin suojaus-virhe: tunnistetietoja ei ollut](#wincred).
- [Tämä tietokone ei voi muodostaa etätietokone](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Etäistunto on katkaistu, koska ei ole Remote työpöydän palvelimet voi antaa käyttöoikeuden.

Syy: 120 päivän käyttöoikeuksien myöntämistä lisäajan työpöydän etäpalvelimeen roolin on päättynyt, ja sinun on asennettava käyttöoikeudet.

Voit kiertää tämän ongelman RDP-tiedoston paikallisen kopion tallentaminen-portaalista ja suorita tämä komento PowerShell komentokehotteeseen muodostaa. Tässä vaiheessa poistaa vain yhteyden käyttöoikeudet:

        mstsc <File name>.RDP /admin

Jos et tarvitse itse asiassa AM enemmän kuin kaksi samanaikaisesti etätyöpöydän yhteydet, voit poistaa työpöydän etäpalvelimeen rooli Serverin hallinta.

Lisätietoja on artikkelissa blogimerkinnässä [Azure AM epäonnistuu, jonka "Ei ole RDL: ää palvelimessa käytettävissä"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Etätyöpöytä ei löydä tietokoneen "nimi".

Syy: Etätyöpöydän asiakkaan tietokoneen ei voi selvittää tietokoneen asetusten RDP-tiedoston nimi.

Ratkaisuja:

- Jos olet organisaation intranetissä, varmista, että tietokone käyttää välityspalvelinta ja voit lähettää HTTPS-liikenne.

- Jos käytät RDP paikallisesti tallennetun tiedoston, kokeile portaalin luoma haluamasi vaihtoehto. Tällä vaiheella varmistetaan, että sinulla on virtuaalikoneen, tai pilvipalvelussa ja AM päätepisteportti oikea DNS-nimi. Näin portaalin luoma mallitiedostossa RDP:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

RDP-tiedoston osoite-osa on:
- Pilvipalvelussa, joka sisältää AM ("tailspin-azdatatier.cloudapp.net" tässä esimerkissä) täydellinen toimialuenimi.

- Ulkoisen tietoliikenteen Etätyöpöytä (55919) päätepisteen TCP-portin.

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Todennus on virhe. Paikallinen suojaus viranomainen ei voi muodostaa yhteyttä.

Syy: Kohde AM ei löydy suojaustoiminnon käyttäjän nimi yläosassa tunnistetiedot.

Kun käyttäjänimi on lomake *SecurityAuthority*\\*käyttäjänimi* (Esimerkki: CORP\User1), *SecurityAuthority* -osa on AM tietokonenimi (paikallisen suojaustoiminnon) tai Active Directory-toimialuenimi.

Ratkaisuja:

- Jos tili on paikallinen AM, varmista, että AM nimi on kirjoitettu oikein.

- Jos tili on Active Directory-toimialueen, oikeinkirjoituksen toimialuenimi.

- Jos se on Active Directory-toimialuetilin ja toimialuenimi on kirjoitettu oikein, varmista, että kyseisen toimialueen ohjauskoneen on käytettävissä. Se on yleisiä ongelma Azure virtual verkoissa, jotka sisältävät toimialueen ohjaimet, että toimialueen ohjauskoneen ei ole käytettävissä, koska se ei ole käynnistetty. Vaihtoehtoisesti voit paikallisen järjestelmänvalvojatilin toimialuetilin sijaan.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Windowsin suojaus-virhe: tunnistetietoja ei ollut.

Syy: Kohde AM ei voi vahvistaa tilin nimi ja salasana.

Windows-tietokoneella, voit vahvistaa paikallisen tilin tai toimialuetilin tunnistetiedot.

- Paikallisten käyttäjätilien käyttää *tietokoneen nimi*\\syntaksi*käyttäjänimi* (Esimerkki: SQL1\Admin4798).
- Käytä domain-tileissä *toimialuenimi*\\syntaksi*käyttäjänimi* (Esimerkki: CONTOSO\peterodman).

Jos olet muuttanut oman AM, uusi Active Directory-metsää toimialueen ohjauskoneen, paikallisen järjestelmänvalvojatilin, jotka olet kirjautunut sisään muunnetaan vastaavat tilille, jolla on sama salasana uusi metsää ja toimialueen. Paikallisen tilin poistetaan.

Esimerkiksi jos kirjautuneet paikallisen tilin DC1\DCAdmin ja virtuaalikoneen ylemmän tason nimellä-uusi-metsää corp.contoso.com toimialueen ohjauskoneen, DC1\DCAdmin paikallisen tilin saa poistetaan ja saman salasanan luodaan uusi toimialuetili (CORP\DCAdmin).

Varmista, että tilin nimi on nimi, joka tarkistaa virtuaalikoneen kelvollinen tiliksi ja salasana ovat oikein.

Jos tarvitset paikallisen järjestelmänvalvojan tilin salasanan, katso, [miten vaihtamaan salasanan tai Windowsin näennäiskoneiden Etätyöpöytä-palveluun](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Tämä tietokone ei voi muodostaa etätietokone.

Syy: Tiliä, jota käytetään yhteyden ei ole etätyöpöydän kirjautumisen oikeudet.

Jokaisella Windows-tietokoneella on Etätyöpöydän käyttäjät paikalliseen ryhmään, joka sisältää tilien ja ryhmiä, jotka voivat kirjautua siihen etäyhteyden välityksellä. Paikallisen Järjestelmänvalvojat-ryhmän jäsenillä on oikeudet, myös, vaikka tilit eivät näy Etätyöpöydän käyttäjät paikalliseen ryhmään. Toimialueen liittyneet tietokoneissa paikallisen järjestelmänvalvojaryhmän sisältää myös toimialueen toimialueen järjestelmänvalvojia.

Varmista, että käytät yhdistämään tili on Etätyöpöydän kirjautumisen oikeudet. Voit kiertää tämän ongelman liittää etätyöpöydän kautta toimialueen tai paikallisen järjestelmänvalvojatilin avulla. Voit lisätä haluamasi tilin Etätyöpöydän käyttäjät paikalliseen ryhmään käyttämällä Microsoft Management Console ‑laajennuksen (**Järjestelmätyökalut > Paikalliset käyttäjät ja ryhmät > Ryhmät > Etätyöpöydän käyttäjät**).


## <a name="next-steps"></a>Seuraavat vaiheet
Jos mikään näistä virheitä on tapahtunut ja yhteyden muodostaminen RDP Tuntematon ongelma, on artikkelissa [vianmääritysoppaan etätyöpöydän](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure IaaS (Windows) diagnostiikka-paketti](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Käytettäessä AM sovellusten vianmääritysohjeita on artikkelissa [vianmääritys Azure-AM käytössä-sovellukseen](virtual-machines-linux-troubleshoot-app-connection.md).
- Jos suojattu runko (SSH) avulla voit muodostaa yhteyden Linux-AM Azure-tietokannassa on vaikeuksia, lue [vianmäärityksen SSH yhteydet Linux-AM Azure-tietokannassa](virtual-machines-linux-troubleshoot-ssh-connection.md).
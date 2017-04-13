<properties
   pageTitle="Azure Tietoturvakeskus Vianmääritysoppaan | Microsoft Azure"
   description="Tämän asiakirjan auttaa Azure Tietoturvakeskus-ongelmien vianmääritys."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Azure Tietoturvakeskus vianmääritysopas
Tässä oppaassa on tietoja tietotekniikan asiantuntijoille, tietojen suojauksen analyytikot ja cloud järjestelmänvalvojat organisaatiot, joiden käytät Azure Tietoturvakeskuksessa ja vianmääritys Tietoturvakeskus liittyvät ongelmat.

## <a name="troubleshooting-guide"></a>Vianmääritysoppaan käyttäminen
Tässä oppaassa kerrotaan, kuinka voit tehdä vianmäärityksen Tietoturvakeskus liittyvät ongelmat. Useimmat valmis Tietoturvakeskuksessa vianmääritys tapahtuu [Valvontaloki](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) tietueet epäonnistui-osan ensimmäisen tarkistamalla. Valvontalokien, avulla voit määrittää:

- Mitä toimintoja on tapahtunut
- Kuka aloitti toimintoa
- Kun toiminto on tapahtunut
- Toiminnon tila
- Muita ominaisuuksia, jotka voivat auttaa arvojen tutkiminen toiminto

Valvontalokin sisältää kaikki kirjoitustoiminnot (HYLLYTETTY, Kirjaa poistaminen) suorittaa resursseja, mutta se ei sisällä luku toiminnot (HAE).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Windowsin seurantaa agent-asennuksen vianmääritys

Seurannan agentti Tietoturvakeskus avulla voit suorittaa tietojen kerääminen. Kun tietojen kerääminen on käytössä ja agentti on asennettu oikein kohde tietokoneen, nämä prosessit pitäisi olla suorittamisen:

- ASMAgentLauncher.exe - Azure agentti seuranta 
- ASMMonitoringAgent.exe - Azure suojauksen seuranta-tunniste
- ASMSoftwareScanner.exe – Azure tarkistuksen hallinta

Azure-suojauksen valvonta-tunniste eri asiaa suojausasetukset etsii ja tietoturva kerää virtuaalikoneen. Tarkistus-hallinnan käytetään korjaustiedoston skanneria.

Jos asennus suoritetaan onnistuneesti samanlainen kuin kohteen AM valvontalokien merkinnän pitäisi näkyä seuraavat:

![Valvontalokien](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Voit hankkia lisätietoja asentunut lukemalla agent-lokit, osoitteessa *%systemdrive%\windowsazure\logs* (Esimerkki: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Jos Azure Security Center-agentti virheellisestä toiminnasta sarjoituksen, sinun on käynnistämään kohde AM, koska komento Lopeta ja Käynnistä-agentti ei.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Seuranta-Linux agent-asennuksen vianmääritys
Kun AM Agent-asennuksen vianmääritys Linux-järjestelmään, varmista, että tiedostotunniste on ladattu/var/lib/waagent /. Voit suorittaa vahvistamiseksi, jos se on asennettu alla komennon:

`cat /var/log/waagent.log` 

Muut lokitiedostojen, voit tarkastella vianmäärityksen tarkoitus on: 

- /var/log/mdsd.ERR
- / var/lokin/azure /

Toimiva järjestelmä pitäisi näkyä mdsd prosessin yhteyden TCP 29130. Tämä on yhteydenpito mdsd prosessin syslog. Voit tarkistaa tämän ongelman suorittamalla komennon alla:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Ota yhteyttä Microsoftin tuotetukeen

Asioita, jotka voidaan tunnistaa ohjeiden edellyttäen tämän artikkelin muiden löytyvät myös kuvattu osoitteessa Tietoturvakeskus julkiseen [keskustelupalstalle](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Kuitenkin jos on edelleen vianmääritys, voit avata uusi tukipyyntö käyttämällä Azure Portal alla kuvatulla tavalla: 

![Microsoft-tuki](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Katso myös

Tässä asiakirjassa opit suojauskäytäntöjen määrittäminen Azure Tietoturvakeskuksessa. Saat lisätietoja Azure Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Security Center suunnittelu- ja käyttöoppaan](security-center-planning-and-operations-guide.md) – tietoja suunnitteleminen ja ymmärtää tyyliseikat antaa Azure Tietoturvakeskuksessa.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) – Katso, miten voit hallita ja vastata suojausvaroitusten
- [Kumppaniratkaisuihin ja Azure Tietoturvakeskus seuranta](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla
- [Azure Security-blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen blogit

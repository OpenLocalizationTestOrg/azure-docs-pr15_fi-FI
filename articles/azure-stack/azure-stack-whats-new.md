<properties
    pageTitle="Uudet Azure Pinotut | Microsoft Azure"
    description="Azure Pinotut uudet ominaisuudet"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Mitä uusia ominaisuuksia Azure pinon tekninen esikatselu 2
Tässä versiossa on uusia ominaisuuksia sekä alihallinnat, jotka järjestelmänvalvojia.

## <a name="network"></a>Verkon   
   - [IDN](azure-stack-understanding-dns-in-tp2.md) on sisäisen verkon nimi rekisteröinti ja toimialueen nimi järjestelmän (DNS)-ratkaisun DNS: n lisäinfrastruktuuri ei ole.
   - [Virtual verkon yhdyskäytävien](azure-stack-create-vpn-connection-one-node-tp2.md) on VPN-yhteysasetukset Azure tai paikalliseen resursseja.
   - Käyttäjän määrittämät tiet voit reitittää verkkoliikenteen palomuurit, suojaus, muut laitteet ja muiden palveluiden kautta.
   - Voit luoda verkkoresursseja Marketplacesta.   

## <a name="storage"></a>Tallennustilan
 - [Azure olevien](https://msdn.microsoft.com/library/dd179353.aspx) käyttöön luotettava ja pysyvä palvelun messaging.
 - [Tallennustilan analytics](https://msdn.microsoft.com/library/azure/hh343270.aspx) capture tallennustilan suorituskykytietoja. Voit jäljittää pyynnöt ja analysoida käyttö trendejä ongelmien vianmääritys tallennustilan-tilisi tiedot.
 - Blob-objektien tallennustilaan tukee [liittäminen lohko-toimintoa](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Premium tallennustilan API tilin tuki.
 - Vuokraaja palvelun tallennustilan yleisiä työkalut ja SDK: T, kuten Azure CLI, PowerShell, .NET, Python ja Java SDK tuki. 
 - [Tallennustilan tilin jaettu Access allekirjoituksen](https://msdn.microsoft.com/library/azure/mt584140.aspx) käyttöön hajautetun delegointi tallennustilan-palveluiden käyttäminen ilman, että voit jakaa koko tiliavain.  
 - Tallennustilan services nyt käyttää [Ryhmän hallittuja palvelutilejä](https://technet.microsoft.com/library/hh831477.aspx) vahva arvopaperille, jonka pienen hallinta katseltavan.
 - Voit vapauttaa käyttämättömät vuokraajan resurssien pyydettäessä.
 - Poistetun tallennustilan tilin säilytys enimmäispituus on määritettävä.
 - Voit palauttaa poistetun vuokraajan tallennustilan tilit.

## <a name="compute"></a>Laske
- Voit poistaa varauksen näennäiskoneiden.
- Voit käyttöön virtuaalikoneen tunnisteet vianmääritys ja määritysten hallintaa varten.
- Voit muuttaa virtuaalikoneen levyjä.
- Näennäiskoneiden voi olla useita Verkkoliittymät.

### <a name="portal-experience"></a>Portaalin kokemus
 - Azure pinon alueilla on looginen mittayksikön asteikko ja Azure pinon sisällä hallinta. Esikatselussa voit tarkastella tietoja palveluita suorittaminen, verkon ja tallennustilaa alueittain.
 - Nyt voit tarkastella (päivittää) [azure-pino-updates.md]-käyttöliittymän.

## <a name="key-vault"></a>Avaimen säilö
- [Avaimen säilö Azure Pinotut](azure-stack-kv-intro.md) on suojattu näppäimet ja salasanat cloud-sovellusten hallinta.
- Voit valvoa ja seurata sovellukset ja VMs avaimen käyttö.

## <a name="billing-and-usage"></a>Laskutus- ja käyttö
 - [Laskutus- ja kulutus ohjelmointirajapinnan](azure-stack-billing-and-chargeback.md) Näytä tiedot siitä, miten palvelujen on käytetty.  
 - Valtuutetun tarjoajien käyttöön jälleenmyyjät Azure pinon palvelujen tarjoaminen asiakkaidensa.

## <a name="monitoring-and-health"></a>Seuranta- ja kuntotietojen
 - Uusi valvontaan käytettävissä portaalin ja ohjelmointirajapinnan Salli itse tarkastella ja hallita ilmoitukset-ympäristöön.  

## <a name="next-steps"></a>Seuraavat vaiheet
- [Tietoja Azure pinon Käsitteiden arkkitehtuuri](azure-stack-architecture.md)      
- [Tietoja käyttöönoton edellytykset](azure-stack-deploy.md)
- [Azure pinon käyttöönotto](azure-stack-run-powershell-script.md)

  

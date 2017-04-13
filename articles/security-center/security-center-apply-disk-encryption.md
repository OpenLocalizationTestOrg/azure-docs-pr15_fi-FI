<properties
   pageTitle="Salaa Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **Käytä salauksen**täytäntöön."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Salaa Azure Tietoturvakeskuksessa

Azure Tietoturvakeskus suosittelee, että asennat salauksen, jos käytössäsi on Windows- tai Linux AM levyjä, jotka eivät ole salattuja käyttämällä Azure salauksen. Salauksen voit salata Windows ja Linux IaaS AM levyjen.  Salauksen suositellaan OS sekä tiedot että AM asemat.


Salauksen hyödyntää alan [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -vakio ominaisuus, Windowsin ja antamaan OS ja tietojen suojaaminen ja tietoja ja vastaa organisaation tietosuojaan ja vaatimustenmukaisuuteen sitoumukset salausta Linux [DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -ominaisuus. Salauksen on integroitu [Azure avaimen säilö](https://azure.microsoft.com/documentation/services/key-vault/) avulla voit hallita ja levyn salausavaimet ja tietoja avaimen säilö tilaukseesi, ja varmistaa, että, että kaikki tiedot AM kohtauksen salataan muiden [Azure-tallennustilan](https://azure.microsoft.com/documentation/services/storage/)hallinta.

> [AZURE.NOTE] Azure salauksen tuetaan Windows server käyttöjärjestelmät - Windows Server 2008 R2, Windows Server 2012: ssa ja Windows Server 2012 R2. Salauksen tuetaan Linux palvelimen käyttöjärjestelmät - Ubuntu, CentOS, SUSE ja SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Käytä salauksen** **suositukset** -sivu.
2. **Käytä salauksen** -sivu tulee näkyviin, jonka salauksen suositellaan VMs luettelo.
3. Salaa nämä VMs ohjeiden mukaan.

![][1]

Salaa Azuren näennäiskoneiden, joka on merkitty Tietoturvakeskus kuin hänen nimeään tarvitse salausta, on suositeltavaa seuraavasti:

- Asenna ja määritä PowerShellin Azure. Tämän avulla voit suorittaa tarvitse salata Azuren näennäiskoneiden edellytykset määrittämiseen tarvittavat PowerShell-komennoilla.
- Hanki ja suorita Azure levyn salaus edellytykset Azure PowerShell-komentosarjaa.
- Salaa oman näennäiskoneiden.

[Salaa Azure-virtuaalikoneen](security-center-disk-encryption.md) edetään ohjatusti vaiheittain seuraavasti.  Tässä artikkelissa oletetaan, että käytössäsi on Windows 10: ssä kuin asiakaskoneeseen, jonka määrität salauksen.

On monta tapaa, joilla voidaan käyttää edellytykset: n määrittäminen ja salauksen Azuren näennäiskoneiden määrittäminen. Jos olet jo hyvin versed PowerShellin Azure tai Azure CLI, saatat haluta käyttää vaihtoehtoinen tavoista. Näitä tietoja muista tavoista artikkelissa [Azure salauksen](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Katso myös

Tämän asiakirjan osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Käytä salauksen." Saat lisätietoja salauksen on seuraavissa artikkeleissa:

- [Salauksen ja hallintaa ja Azure avaimen säilö](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video-ja 36 pienin 39 sec) – Opi käyttämään levyn salaus hallinnan IaaS VMs ja Azure avaimen säilö suojaaminen ja tietoja.
- [Salaa Azure virtuaalikoneen](security-center-disk-encryption.md) (asiakirja) – Katso, miten voit salata Azuren näennäiskoneiden.
- [Azure salauksen](../security/azure-security-disk-encryption.md) (asiakirja) – Opi ottamaan Windows-ja Linux VMs salauksen.

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Katso, miten voit määrittää suojauskäytännöt.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png

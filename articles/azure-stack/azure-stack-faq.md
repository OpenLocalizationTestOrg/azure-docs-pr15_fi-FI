<properties
    pageTitle="Usein kysytyt kysymykset Azure pinon | Microsoft Azure"
    description="Azure pinon usein kysytyt kysymykset."
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
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Usein kysytyt kysymykset Azure pino

## <a name="deployment"></a>Käyttöönotto

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Pitääkö muotoon Omat tiedot-levyjä ennen aloittamista tai uudelleen asennuksen?

Levyjen on oltava raaka-muodossa. Jos asennat käyttöjärjestelmä, voit joutua Jos vanha tallennustilan varanto on yhä, valitse ja poista seuraavalla tavalla:

1. Avaa Serverin hallinta.
2. Valitse tallennustilan jakavat.
3. Katso, onko tallennustilan resurssivarantoon luettelossa.
4. Napsauta hiiren kakkospainikkeella **tallennustilan resurssivarantoon** , jos luettelossa ja ottaminen käyttöön luku / kirjoittaminen.
5. Napsauta hiiren kakkospainikkeella **Virtual kiintolevyä** (vasemmassa alakulmassa) ja sitten Poista.
6. **Tallennustilan resurssivarantoon** hiiren kakkospainikkeella ja valitse Poista.
7. Azure-pino komentosarjan Käynnistä uudelleen ja varmista levyn vahvistus välittää.

Vaihtoehtoisesti voit käyttää seuraavaa komentosarjaa:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Kaikkien Suoritettaessa levyjen käyttää tallennustilan resurssivarantoon Käsitteiden asennuksen varten

Tässä versiossa ei tueta tässä määrityksessä.  Lisätietoja on lisätietoja [vaatimukset opas](azure-stack-deploy.md) .

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>NVMe tietojen levyjen käyttää Microsoft Azure pinon Käsitteiden varten

Kun tallennustilan välilyöntejä suoraan tukee NVMe levyjä, Azure-pino tukee vain alijoukkoa mahdollista Asematyypit ja mahdollisten kombinaatioiden tallennustilan välilyöntejä suoraan. 

### <a name="how-can-i-reinstall-azure-stack"></a>Miten Azure pinon voi asentaa uudelleen?
Voit noudattaa [lukea-oppaan](azure-stack-redeploy.md)vaiheet.  

## <a name="tenant"></a>Vuokraajan

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Omien kuvien käyttöön kuin palvelutili

Kyllä, aivan kuten Azure-tietokannassa, palvelutili ladata kuvat Azure Pinotut lisäksi palvelun järjestelmänvalvoja kuvia. Yleisiä tietoja Katso [AM-kuva lisätään](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testaaminen

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Testaa Microsoft Azure pinon Käsitteiden ylemmän tason Virtualizationin avulla?

Sisäkkäiset virtualization ei tueta tai testattu Azure pinon tekninen esikatselu 2.

## <a name="virtual-machines"></a>Näennäiskoneiden

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Tukeeko Azure pinon dynaamisiksi näennäiskoneiden?

Azure pinoa ei tue dynaamisiksi.

## <a name="next-steps"></a>Seuraavat vaiheet

[Vianmääritys](azure-stack-troubleshooting.md)

<properties
    pageTitle="Palauta avain säilöön-näppäintä ja salaisen salattuja VMs Azure varmuuskopioinnista | Microsoft Azure"
    description="Opettele palauttamaan avaimen säilöön-näppäintä ja salaisen Azure varmuuskopion PowerShellin avulla"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Palauta avain säilöön-näppäintä ja salaisen salattuja VMs Azure varmuuskopioinnista
Tässä artikkelissa on lyhyt Azure AM varmuuskopion käytöstä suorittamiseen salattuja Azure VMs Palauta, jos avain ja salaisuus eivät ole avaimen säilö. Nämä vaiheet voidaan myös, jos haluat säilyttää kopion key (avain salausavaimen) ja salaisuus (BitLocker salausavaimen) palautettu AM.

## <a name="pre-requisites"></a>Vaatimukset

1. **Varmuuskopiointi salattu VMs** - salattu Azure VMs varmuuskopioidaan Azure varmuuskopion käytöstä. Lisätietoja [hallinta varmuuskopiointi ja palauttaminen Azure VMs PowerShellin avulla on](backup-azure-vms-automation.md) artikkelissa lisätietoja Varmuuskopioi salattuja Azure VMs.

2. **Määritä Azure avaimen säilö** – avaimen säilö, johon näppäimet ja tietoja on voi palauttaa, varmista, että on jo olemassa. Katso tiedot avaimen säilö hallinnasta on artikkelissa [Azure avaimen säilö käytön aloittaminen](../key-vault/key-vault-get-started.md) .

## <a name="setup-recovery-services-vault"></a>Asennuksen palautus services säilöön 
Kirjaudu sisään PowerShell ja Aseta palautus services säilö konteksti seuraavien vaiheiden avulla

### <a name="log-in-to-azure-powershell"></a>Azure PowerShell kirjautuminen 

Kirjaudu sisään käyttämällä seuraavan cmdlet-komennon Azure-tili

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Aseta palautus services säilö konteksti

Kun kirjautunut, saat luettelon käytettävissä tilausten seuraavan cmdlet-komennon avulla

```
PS C:\> Get-AzureRmSubscription
```

Valitse tilaus, johon resursseja on käytettävissä

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Aseta käyttämällä palautus Services säilö missä varmuuskopiointi on käytössä salattuja VMs säilö konteksti

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Hae palautuspiste 

Valitse säilö, joka edustaa salattuja Azure virtuaalikoneen säilöön

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Käytä tätä säilö, pääset takaisin vastaavan virtuaalikoneen kohteeseen

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Matriisin palautus asioista hankkiminen muuttujan rp valitun kohteen varmuuskopioinnin

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Palauttaa salattuja virtuaalikoneen
Seuraavien vaiheiden avulla voit palauttaa salattuja AM sen avain ja salaisuus.

### <a name="restore-key"></a>Palauttaa avaimen

Matriisin $rp edellä on lajiteltu käänteisessä järjestyksessä aika, jossa uusimmat palautuspiste, indeksissä 0. Esimerkki: $rp [0] valitsee uusimman palautuspiste.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Kun tämä cmdlet-komento on suoritettu onnistuneesti, blob-tiedosto saa muodostettu tietokoneeseen määritettyyn kansioon, johon se suoritetaan. Blob-tiedosto vastaa avaimen salatun avaimen salattuja lomakkeessa.

Palauta avaimen Edellinen avaimen säilö seuraavat cmdlet-komennolla. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Palauttaa salaisuus

Salainen tietojen palauttaminen palautuspiste saatu yläpuolella

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Teksti ennen vault.azure.net edustaa alkuperäinen avaimen säilö nimi. Tietoja jälkeen oleva teksti / salainen nimi. 

Pyydä salainen nimen ja arvon cmdlet-komento, suorita edellä tulosteen siltä varalta, jota haluat käyttää samaa salainen nimeä. Muussa tapauksessa $secretname alla on päivitettävä uusi salainen nimeä. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Määritä salainen, tunnisteet siltä varalta, että AM on palautettava paikan päällä. Tunnisteen DiskEncryptionKeyFileName arvon pitäisi näkyä aiot käyttää salaisuus nimi. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
DiskEncryptionKeyFileName arvo on sama kuin edellä saatu salainen nimi. DiskEncryptionKeyEncryptionKeyURL arvo saadaan avaimen säilöstä jälkeen takaisin näppäimet palauttaminen ja [Hae AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet-komennolla   

Määritä tärkeimmät säilö salainen takaisin

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Palauttaa virtuaalikoneen
Yllä PowerShell cmdlet-komentojen avulla voit palauttaa avain ja salainen takaisin avaimen säilö, jos ole varmuuskopioinut Azure AM varmuuskopioinnista salattuja AM. Jälkeen palauttamisesta, Lisätietoja on artikkelissa palauttaa salattuja VMs [hallinta PowerShellin Azure VMs palauttaminen ja varmuuskopiointi](backup-azure-vms-automation.md) .

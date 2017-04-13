<properties
   pageTitle="Säilö palautus-palvelujen usein kysytyt kysymykset | Microsoft Azure"
   description="Usein kysytyt tämä versio tukee Azure varmuuskopiointi-palvelun julkisen Preview-versio. Etsi vastauksia usein kysyttyihin kysymyksiin backup agentti, varmuuskopiointi ja säilytys, palautus, suojaus ja muut Azure varmuuskopion ratkaisun usein kysyttyihin kysymyksiin."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="varmuuskopion ratkaisu; Varmuuskopiointi-palvelu"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Palautus Services säilöön - usein kysytyt kysymykset


Tässä artikkelissa on tietoja palautus Services säilöön, ja se täydentää [Azure varmuuskopiointi usein kysytyt kysymykset](backup-azure-backup-faq.md). Azure varmuuskopiointi Kysytyissä on käyttää kaikkia käytettävissä olevia kysymyksiä ja vastauksia Azure varmuuskopiointi-palvelusta.  

Voit kysyä kysymyksiä Azure varmuuskopiointi tämän artikkelin tai liittyvässä artikkelissa Disqus-osassa. Voit myös lähettää kysymyksiä Azure varmuuskopiointi-palvelun [tuoteluettelon](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Palautus Services vaults ovat Resurssienhallinta perusteella. Tuetaan varmuuskopiointi vaults (perinteinen tila) edelleen? <br/>
Kyllä, varmuuskopiointi vaults tuetaan edelleen. Luo varmuuskopio vaults [Classic-portaalissa](https://manage.windowsazure.com). Luo palautus Services vaults [Azure portal](https://portal.azure.com). Mutta suositeltavaa luoda palautus services säilöön kuin kaikkien tulevien parannukset on saatavana vain palautus Services säilö.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Varmuuskopiointi-säilö siirtää, palautus palvelut-säilö <br/>
Valitettavasti ei tällä hetkellä ei voi siirtää varmuuskopiointi säilö sisällön palauttaminen palvelut-säilö. Yritämme lisäämisestä tätä toimintoa, mutta se ei ole käytettävissä Public Preview osana.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Palautus Services vaults tukevat perinteinen VMs tai Resurssienhallinta-pohjaiseen VMs? <br/>
Palautus Services vaults tukevat sekä malleja.  Voit varmuuskopioida AM, joka on luotu Classic-portaalissa (jotka ovat perinteistä VMs) tai AM luotu Azure-portaalissa (jotka ovat Resurssienhallinta mukaan) palauttaminen palvelut-säilö.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Voin varmuuskopioinut Omat perinteinen VMs varmuuskopion säilöön. Nyt haluan siirtää Omat VMs perinteistä Resurssienhallinta-tilassa.  Miten voin varmuuskopioida ne palautus services säilöön?
Perinteinen VMs varmuuskopion säilöön varmuuskopioita ei siirtää automaattisesti palautus services säilö siirtyessäsi VMs-perinteinen Resurssienhallinta-tilaan. Tarkista seuraavasti AM varmuuskopioiden siirtoa varten:

1. Varmuuskopion säilöön **Suojatut osat** -välilehti ja valitse AM. Valitse [Lopeta suojaus](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Jätä, *Poista liittyvät varmuuskopiotiedot* vaihtoehto **ole valittuna**.
2. Siirtää virtuaalikoneen perinteistä Resurssienhallinta-tilassa. Varmista, että säilytys- ja virtuaalikoneen vastaava verkon ovat myös asianmukaisesti Resurssienhallinta-tilaan.
3. Luo palautus-palveluiden säilö ja varmuuskopiointi määrittämistä siirretyt virtuaalikoneen **varmuuskopion** toiminnon säilö Raporttinäkymät-ikkunan päälle. Lisätietoja ottamisesta [käyttöön varmuuskopion palautus services säilöön](backup-azure-vms-first-look-arm.md)

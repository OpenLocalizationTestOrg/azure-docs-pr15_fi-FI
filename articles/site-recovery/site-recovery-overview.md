<properties
    pageTitle="Mikä on sivuston palauttaminen? | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus Azure palauttaminen-palvelun ja käyttöönoton tilanteita, joissa on yhteenveto."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Mikä on sivuston palauttaminen?

Tervetuloa käyttämään Azure sivuston palauttaminen! Tässä artikkelissa kuvataan lyhyesti palauttaminen-palvelun ja miten se edistää liiketoimintaasi.

Organisaatiosi on Liiketoiminnan jatkuvuus ja tietojen palauttaminen (BCDR) strategia, joka säilyttää sovellukset ja työmääriä turvallisten ja käytettävissä olevien aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. Sivuston palautus on Azure-palvelu prosentuaalista osuutta tästä strategia.

Sivuston palauttaminen orchestrates paikallisen fyysiset palvelimet ja näennäiskoneiden työmääriä replikoinnin. Voit toistaa palvelimia ja VMs-ensisijainen palvelinkeskuksen pilveen (Azure) tai toissijainen palvelinkeskukseen. Katkokset toteutuessa ensisijainen sivustossa voit epäonnistua päälle toissijainen sivustoon pitämään sovellukset ja työmääriä saatavissa ja käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin.

## <a name="site-recovery-in-the-azure-portal"></a>Sivuston palauttaminen Azure-portaalissa

Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit. Azure Resurssienhallinta-malli ja perinteinen palveluiden hallinta-malli. Azure on myös kaksi portaaleihin – [Azure perinteinen portal](https://manage.windowsazure.com/) , joka tukee perinteinen käyttöönoton mallia ja [Azure portal](https://portal.azure.com) perinteinen ja Resurssienhallinta mallien tuki.

- Sivuston palautus on käytettävissä sekä perinteinen portaalin ja Azure-portaalin.
- Azure perinteinen-portaalissa voit tukea palauttaminen perinteinen palveluiden hallinta mallia.
- Azure-portaalissa voit tukea perinteinen malli tai resurssi hallinnan käyttöönotto. 

Tämän artikkelin tiedot koskevat perinteinen- ja Azure portaalin asennuksia. Erot on merkitty tarvittaessa.


## <a name="why-deploy-site-recovery"></a>Miksi ottaa palauttaminen?

Näin palauttaminen miten yrityksesi:

- **Yksinkertaistaa BCDR**— voit käsitellä replikoinnin, automaattisesti ja palauttamista useita työmääriä yhteen paikkaan Azure-portaalissa. Sivuston palauttaminen orchestrates replikointi ja automaattisesti, mutta ei leikkauspiste-sovelluksen tietojen tai ole mitään tietoja.
- **Anna joustavia replikoinnin**– käyttämällä sivuston palauttamisen, voit toistaa tuetut Hyper-V VMs, VMware VMs ja Windows-tai Linux fyysiset palvelimet toiminnoista.
- **Suorita helposti replikoinnin testaaminen**– sivuston palautus on testi failovers tukemaan vaikuttamatta tuotannon ympäristöissä tietojen palauttaminen työkaluja.
- **Epäonnistua päälle ja palauttaa**— voit suorittaa odotettu katkokset nolla tietojen tappio varten suunnitellut failovers tai suunnittelematon failovers mahdollisimman vähän tietojen menettämisen (sen mukaan, replikoinnin taajuus) odottamattomia Luonnonvahinkoja koskevat kanssa. Automaattisesti, kun voivat tuntisesta ensisijainen sivustoihin. Sivuston palauttaminen on palautus suunnitelmat, jotka voivat sisältää komentosarjoja ja Azure automaatio työkirjojen niin, että voit mukauttaa automaattisesti ja monitasoisten sovellusten palauttaminen.
- **Taustaäänten toissijainen palvelinkeskukseen**, voit toistaa työmääriä, Azure toissijainen-sivuston sijaan. Näin kustannuksia ja ylläpito toissijainen palvelinkeskuksen monimutkaisuutta. Replikoitua tiedot tallennetaan Azuren tallennustilaan, joka sisältää toimintakykyyn kanssa. Replikoitua tiedot VMs luodaan automaattisesti yhteydessä.
- **Olemassa olevien BCDR tekniikoiden integroida**– palauttaminen integroituu BCDR muita ominaisuuksia. Voit esimerkiksi suojata yrityksen toiminnoista, kuten SQL Server AlwaysOn hallittavan käytettävyys ryhmien vikasietotilaa tuki SQL Server-taustatietokannan palauttaminen.

## <a name="what-can-i-replicate"></a>Mitä voin toistaa?

Näin voit toistaa yhteenveto käyttämällä palauttaminen.

![Paikalliseen paikalliseen](./media/site-recovery-overview/asr-overview-graphic.png)

**TOISTAA** | **JOS HALUAT TOISTAA** 
---|---
Paikallisen VMware VMs käytössä toiminnoista | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Toissijainen](site-recovery-vmware-to-vmware.md)
Paikallisen Hyper-V VMs käytössä työmääriä hallitaan VMM paveikslėlis  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Toissijainen](site-recovery-vmm-to-vmm.md) 
Paikallisen Hyper-V VMs käytössä työmäärää hallitaan VMM paveikslėlis, SAN tallennustilan kanssa|  [Toissijainen](site-recovery-vmm-san.md)
Paikallisen Hyper-V VMs ilman VMM käytössä toiminnoista | [Azure](site-recovery-hyper-v-site-to-azure.md)
Työmääriä käytössä paikallisen fyysinen Windows-tai Linux-palvelin | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Toissijainen](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Mitä työmääriä voit suojata?

Sivuston palauttaminen mahdollistaa sovelluksen tukeva BCDR niin, että toiminnoista ja sovellusten edelleen suorittaa yhdenmukaisesti katkokset toteutuessa. Sivuston palautus on:

- **Sovelluksen yhdenmukaisia tilannevedoksia**— koneet replikoida sovelluksen yhdenmukaisia tilannevedosten käyttäminen yhden tai usean tason. Lisäksi tallentaa levyn tiedot sovelluksen yhdenmukaisia tilannevedoksia sieppaus sieppaa kaikki tiedot muistiin ja kaikki tapahtumat-prosessin.
- **Lähellä-painikkeen replikoinnin**– sivuston palautus on replikoinnin korkojakso on mahdollisimman alhainen Hyper-V 30 sekuntia ja jatkuvan replikoinnin VMware varten.
- **Joustava palautus suunnitelmat**– voit luoda ja mukauttaa palautus suunnitelmien ulkoisen komentosarjoja sekä manuaalisen toiminnot. Integrointi Azure automaatio runbooks avulla voit palauttaa koko sovelluksen-pino yhdellä napsautuksella.
- **SQL Server AlwaysOn integrointi**– voit hallita käytettävyys ryhmien vikasietotilaa palauttaminen palautus-palvelupakettien.
- **Automaatio-kirjasto**– rich Azure automaatio-kirjasto sisältää tuotannon-sivuille, sovelluksen kielikohtaiset komentosarjoja, voit ladata ja integroitu palauttaminen.
- **Yksinkertaisia verkkoja hallinta**– Lisäasetukset Verkonhallinta palauttaminen ja Azure yksinkertaistaa sovelluksen Verkkovaatimukset, kuten varaaminen IP-osoitteet, kuormituksen tasoitusmääritykset määrittämisestä ja integrointi Azure liikenteen hallinta tehokkaat verkko switchovers varten.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lue lisää: [mitä työmääriä palauttaminen suojata?](site-recovery-workload.md)
- Lisätietoja sivustojen palauttamisen arkkitehtuurista [miten palauttaminen toimii?](site-recovery-components.md)
 

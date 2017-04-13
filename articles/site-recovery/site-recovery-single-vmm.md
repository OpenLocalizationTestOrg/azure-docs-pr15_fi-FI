
<properties
    pageTitle="Azure palauttaminen: Rinnakkaisten Hyper-V näennäiskoneiden yhteen VMM palvelimeen | Microsoft Azure"
    description="Tässä artikkelissa käsitellään replikoida Hyper-V näennäiskoneiden, kun sinulla on vain yhteen VMM palvelimeen."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Toistaa Hyper-V näennäiskoneiden yhteen VMM palvelimeen

Lue tästä artikkelista lisätietoja replikoida Hyper-V näennäiskoneiden palvelimessa Hyper-V host VMM pilvipohjaisia silloin, kun käyttöönoton on vain yhteen VMM palvelimeen.

Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönottomalli ja Azure-portaalin tukeen käyttöönoton sekä malleille. Tässä artikkelissa Azure-portaalissa replikoinnin määrittämisestä.


Jos sinulla on kysyttävää luettuasi tämän artikkelin, julkaise ne Disqus kommenttien alaosassa on tämän artikkelin tai [Azure palautus palvelut-keskustelupalstalla](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Yleiskatsaus

Jos haluat toistaa Hyper-V VMs Hyper-V isännät-VMM sijaitsee, ja sinulla on vain yhden VMM-palvelimen, voit [toistaa Azure](site-recovery-vmm-to-azure.md)- tai yksittäisen VMM palvelimessa paveikslėlis välillä.

On suositeltavaa replikoida Azure, koska automaattisesti ja palautus ei saumattomasti, kun replikoiminen paveikslėlis välillä ja manuaaliset vaiheet useita tarvitaan. Jos haluat kopioida vain VMM palvelimen avulla, toimi seuraavasti:

- Toistaa yhden erillinen VMM-palvelimen kanssa
- Toistaa yhden laajennetun Windows-klusterin käyttöön VMM-palvelimen kanssa


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Toistaa sivustoista, yksi erillinen VMM-palvelimen kanssa

![Erillisestä VMM virtuaalipalvelin](./media/site-recovery-single-vmm/single-vmm-standalone.png)

Tässä skenaariossa yksittäisen VMM Serverin käyttöönotto kuin ensisijaisen sivuston virtual kone ja replikoida tämän AM palauttaminen ja Hyper-V replikan toissijaisen sivustoon.

1. Määritä Hyper-V AM-VMM. Suosittelemme, että olet asentanut SQL Server-esiintymän käyttämä VMM saman AM, voit säästää aikaa. Jos haluat käyttää Laitteestaan ja SQL Server käyttökatkosta, sinun on palautettava kyseisen esiintymän, ennen kuin voit palauttaa VMM.
2. Varmista, että VMM palvelin on määritetty vähintään kaksi paveikslėlis. Yksi cloud sisältää VMs haluat replikoida ja muut pilveen on toissijainen sijaintiin. Pilveen, joka sisältää VMs, jonka haluat suojata pitäisi olla:

    - Yhden tai useamman VMM host ryhmiä, jotka sisältävät yhden tai usean Hyper-V host palvelimen isännän kunkin ryhmän.
    - Vähintään yksi Hyper-V virtual machine kunkin Hyper-V Host (isäntä)-palvelimeen.

3. Luo palautus palvelut-säilö, Luo ja lataa säilö rekisteröinti-näppäintä ja rekisteröi VMM palvelimen säilö. Rekisteröinnin yhteydessä Azure sivuston palauttaminen tarjoajan asentaminen VMM palvelimeen.
4. Määritä vähintään yksi paveikslėlis VMM AM- ja lisää nämä paveikslėlis Hyper-V isännät.
3. Määritä paveikslėlis suojausasetuksia. Voit määrittää yksittäisen VMM-palvelimen nimi lähde- ja sijainnit. Voit määrittää verkon määritys-Yhdistä pilvi ja haluat suojata pilven replikointi AM verkosta VMs AM verkosta.
4. Ota käyttöön ensimmäisen replikoinnin VMs suojattava verkossa, koska molemmat paveikslėlis sijaitsevat samassa palvelimessa varten.
4. Hyper-V hallinta-konsolin käyttöön Hyper-V replikan, joka sisältää VMM AM Hyper-V isännän ja ota käyttöön AM replikoinnin. Varmista, että Älä lisää VMM AM paveikslėlis, jotka on suojattu palauttaminen. Näin varmistat, että Hyper-V replikan asetukset eivät ole ohittaa palauttaminen.
5. Jos haluat luoda palautus suunnitelmia, Määritä lähde- ja saman VMM-palvelimeen.

Noudata [Tässä artikkelissa](site-recovery-vmm-to-vmm.md) säilö luoda ja rekisteröidä palvelimen suojauksen määrittäminen.

### <a name="what-to-do-in-an-outage"></a>Toimintaohjeet käyttökatkosta

Jos sinun on toimittava toissijainen-sivustosta valmis käyttökatkosta ilmenee, toimi seuraavasti:

1.  Toissijaisen sivuston Hyper-V hallinta-konsolin Suorita suunnittelematon automaattisesti, voi epäonnistua VMM-AM ensisijaisen toissijainen päälle.
2.  Varmista, että VMM AM on toissijainen Sivuston käytön.
3.  Palautus palvelut-säilöön Suorita suunnittelematon automaattisesti, voi epäonnistua työmäärää VMs-ensisijainen ja toissijainen paveikslėlis päälle. Viimeistele VMs suunnittelematon vikasietotilaa, Vahvista vikasietotilaa tai valita eri palautus-kohdan halutulla tavalla.
4.  Suunnittelematon vikasietotilaa päätyttyä, käyttäjät voivat käyttää työmäärää resurssien toissijainen-sivustossa.

Kun ensisijainen sivusto toimii normaalisti uudelleen, toimi seuraavasti:

1.  Ottaa käyttöön Hyper-V hallinta-konsolin käänteisen toistoa varten VMM AM, Aloita replikoiminen-ensisijainen ja toissijainen.
2.  Hyper-V hallinta-konsolin Suorita suunnitellun automaattisesti epäonnistuu takaisin VMM AM ensisijainen-sivustoon. Vahvista valmiiksi vikasietotilaa. Valitse Ota käyttöön Käänteiset replikoinnin Aloita replikoiminen VMM-ensisijainen ja toissijainen.
3.  Palautus palvelut-säilöön käyttöön käänteisen replikoinnin kuormituksen VMs Aloita replikoiminen ne-ensisijainen ja toissijainen.
4.  Palautus palvelut-säilöön Suorita suunnitellun automaattisesti epäonnistuu takaisin työmäärää VMs ensisijainen-sivustoon. Vahvista valmiiksi vikasietotilaa. Valitse Ota käyttöön Käänteiset replikoinnin Aloita replikoiminen työmäärää VMs-ensisijainen ja toissijainen.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Toistaa sivustoissa yhden laajennetun klusterin VMM palvelimen kanssa

![Liitetty VMM virtuaalipalvelin](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Sen sijaan, että käyttöönotto itsenäisen VMM palvelimen kuin AM, kopioi toissijaisen sivuston, voit määrittää VMM erittäin käytettäväksi ottamalla se Windows automaattisesti klusterin AM. Tämä on työmäärää toimintakykyyn ja suojautumista laitteistovirheen. Sivuston palauttaminen käyttöönotosta VMM AM kannattaa ottaa käyttöön Venytys klusterin maantieteellisesti erilliset sivustoissa. Toiminto:

1. Asenna Windows automaattisesti klusterin virtual tietokoneeseen VMM ja valitse asetus, joka suoritetaan palvelimessa käytettävissä asennuksen aikana.
2. SQL Server ‑esiintymä, jota käytetään VMM olisi replikoida SQL Server AlwaysOn käytettävyys ryhmiin, niin, että on toissijainen sivuston tietokannan.
3. Noudata [Tässä artikkelissa](site-recovery-vmm-to-vmm.md) säilö luoda ja rekisteröidä palvelimen suojauksen määrittäminen. Sinun on rekisteröitävä VMM kunkin palvelimen klusterin palautus palvelut-säilöön. Voit tehdä tämän palvelun asentaminen aktiivisen solmun ja rekisteröi VMM palvelimeen. Valitse palvelun asentaminen solmujen.

### <a name="what-to-do-in-an-outage"></a>Toimintaohjeet käyttökatkosta

Käyttökatkosta toteutuessa VMM palvelin ja sen vastaava SQL Server-tietokanta epäonnistui päälle ja käyttää toissijainen-sivustosta.


## <a name="next-steps"></a>Seuraavat vaiheet

[Lue lisää](site-recovery-vmm-to-vmm.md) siitä yksityiskohtaiset palauttaminen käyttöönoton VMM VMM replikointi.

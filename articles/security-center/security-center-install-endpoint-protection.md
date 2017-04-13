<properties
   pageTitle="Asenna Endpoint Protection Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **Asentaa Endpoint Protection**täytäntöön."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Asenna Endpoint Protection Azure Tietoturvakeskus

Azure Tietoturvakeskus suosittelee valmistella antimalware-ohjelman Azuren näennäiskoneiden (VMs) Jos antimalware ei ole jo käytössä. Tämä suositus koskee vain Windows VMs.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Asenna Endpoint Protection** **suositukset** -sivu.
![Valitse asennuksen Endpoint Protection][1]

2. **Asenna Endpoint Protection** -sivu avautuu ja siinä näkyvät VMs luettelo ilman antimalware käytössä. Valitse luettelosta, jonka haluat asentaa antimalware ja valitse **Asenna VMs**VMs.
![Valitse VMs antimalware asentaminen][2]

3. **Valitse Endpoint Protection** -sivu avautuu, jotta voit valita antimalware-ratkaisun, jota haluat käyttää. Tässä esimerkissä Valitse **Microsoft Antimalware**.
![Valitse Endpoint Protection][3]

4. Lisätietoja antimalware-ratkaisun tulee näkyviin. Valitse **Luo**.
![Luo antimalware ratkaisu][4]

5. Lisätä tarvittavat asetukset **Lisää tunniste** -sivu ja valitse sitten **OK**. Lisätietoja asetukset-kohdassa [oletusarvoiset ja mukautetut Antimalware määritys](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) on nyt aktiivinen valitun VMs.

## <a name="see-also"></a>Katso myös

Tässä artikkelissa osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Asenna Endpoint Protection." Saat lisätietoja ottaminen käyttöön Azure antimalware-ohjelmassa on seuraavissa artikkeleissa:

- [Microsoft Cloud Services ja näennäiskoneiden Antimalware](../azure-security-antimalware.md) – Microsoft antimalware opetteleminen.

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Katso, miten voit määrittää suojauskäytännöt.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png

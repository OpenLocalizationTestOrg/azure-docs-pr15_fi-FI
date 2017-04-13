<properties
    pageTitle="Azure AD-toimialueen palveluista: Päivitä DNS-asetukset Azure virtual verkossa | Microsoft Azure"
    description="Azure Active Directory-toimialueen palveluiden käytön aloittaminen"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure AD - toimialueen palvelut-Azure virtual verkon Update DNS-asetukset

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Vaihe 4: Päivitä Azure virtual verkon DNS-asetukset
Edellisten määritystehtävät ottanut onnistuneesti Azure AD toimialuepalveluita hakemistossa. Seuraava tehtävä on varmistaa, että virtual verkon tietokoneita yhteyden ja käyttää näitä palveluita. Päivitä DNS palvelinasetukset virtual verkon osoittamaan kahden IP-osoitteet, jolla Azure AD-toimialueen palveluista on käytettävissä virtual verkossa.

> [AZURE.NOTE] Huomautus alaspäin IP-osoitteiden Azure AD-toimialueen palveluiden näkyviin hakemistossa, **Määritä** -välilehdessä, kun olet ottanut Azure AD-toimialueen palveluista kansion.

Suorita seuraavat määritysvaiheet päivittää virtual verkossa, jossa on käytössä Azure AD-toimialueen palveluista DNS server-asetus.

1. Siirry **Azure perinteinen portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Valitse vasemmanpuoleisessa ruudussa **verkot** -solmu.

    ![Virtuaalinen verkkojen solmu](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Valitse **Virtual verkot** -välilehden virtual verkkoon, jonka otit Azure AD-toimialueen palveluista voit tarkastella sen ominaisuuksia.

4. Valitse **määritys** -välilehti.

    ![Virtuaalinen verkkojen solmu](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. Kirjoita IP-osoitteet Azure AD-toimialueen palveluiden **DNS-palvelimet** -osassa.

6. Varmista, että annat sekä IP-osoitteet, jotka näkyvät hakemistossa **määrittäminen** -välilehden **Toimialuepalveluita** -osasta.

7. Voit tallentaa virtual verkoston DNS-palvelinasetukset valitsemalla tehtäväruudussa sivun alareunassa **Tallenna** .

   ![Päivitä DNS-palvelinasetukset virtual verkossa.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Kun olet päivittänyt DNS-palvelinasetukset virtual verkossa, saat päivitetyn DNS-määrityksiä verkossa voi kestää hetken näennäiskoneiden. Jos virtual machine on yhteyttä toimialueen, tyhjennä DNS-välimuisti (esimerkiksi. ' ipconfig/flushdns')-virtuaalikoneen. Tämä komento pakottaa päivityksen virtuaalikoneen DNS-asetukset.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Tehtävän 5 – Ota käyttöön salasanojen synkronoinnin Azure AD-toimialueen palveluihin
Seuraava määritys tehtävä on [salasana](active-directory-ds-getting-started-password-sync.md)-synkronointia Azure AD-toimialueen palveluihin.

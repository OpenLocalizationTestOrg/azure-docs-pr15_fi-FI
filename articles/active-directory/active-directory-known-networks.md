<properties 
    pageTitle="Tunnetut verkkojen | Microsoft Azure" 
    description="Tunnetut verkkojen määrittäminen voit vältä IP-osoitteet, jotka kuuluvat organisaatiosi sisältyy useita paikkojen-merkki-apuohjelmat ja kirjaudu ins IP-osoitteista epäilyttävistä käyttöraportteja." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Tunnetut verkot


Voit käyttää Azure Active Directoryn käytön ja käyttöraportit ja myönnä näkyvyys eheys ja organisaation hakemistoon turvallisuus. Nämä ohjeet directory järjestelmänvalvojan paremmin selviää missä mahdollisia tietoturvauhkia saattaa ovat niin, että ne suunnittelevat riittävästi pienentämään näitä riskejä.

On mahdollista, että "*Kirjaudu apuohjelmat-useita paikkojen*" ja "*Kirjaudu ins IP-osoitteista epäilyttävistä aktiviteettiin*"-raporttien merkitseminen virheellisesti IP-osoitteet, jotka ovat todella omaisuutta organisaation. 

Tämä voi johtua esimerkiksi kun: 

- Käyttäjän lisääminen oppimiasi office on kirjautunut etäyhteyden tietojen Center San Francisco-käynnistimet "Kirjaudu ins useita paikkojen-"-raportti 

- Organisaation käyttäjä yrittää sign-on useita kertoja väärän salasanan käynnistimien kanssa "Kirjaudu IP-osoitteiden ins epäilyttävistä aktiviteettiin"-raportti 

Jos haluat estää tällaisissa tapauksissa harhaanjohtavia suojaus-raporttien luominen, tunnetut IP-osoitealueet kannattaa lisätä organisaation julkisen IP-osoitteen luettelo.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Jos haluat lisätä organisaation julkiseen IP-osoitealueet, toimimalla seuraavasti: 

1.  Kertakirjauksen [Azure hallinta-portaalin](https://manage.windowsazure.com).

2.  Valitse vasemmanpuoleisessa ruudussa **Active Directorysta**. <br><br>![Cloud App etsiminen toiminta](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Valitse **kansio** -välilehdessä hakemistossa.

4.  Valitse ylä-valikossa valitsemalla **Määritä**. <br><br>![Cloud App etsiminen toiminta](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Siirry Määritä-välilehdessä **että organisaatiot julkiseen IP-osoitealueet** <br><br>![Cloud App etsiminen toiminta](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Valitse **Lisää tunnetut IP-osoitealueet**.

7.  Lisää osoitealueiden tulevasta valintaikkunasta ja valitse sitten Tarkista-painiketta, kun olet valmis. <br><br>![Cloud App etsiminen toiminta](./media/active-directory-known-networks/known-netwoks-04.png)


**Lisäresursseja**


* [Kopioimasta raportin tarkasteleminen](active-directory-view-access-usage-reports.md)
* [Kirjaudu ins IP-osoitteista epäilyttävistä aktiviteettiin](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Kirjaudu useita paikkojen-laajennukset](active-directory-reporting-sign-ins-from-multiple-geographies.md)



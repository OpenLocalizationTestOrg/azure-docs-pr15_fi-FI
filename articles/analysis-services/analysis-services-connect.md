<properties
   pageTitle="Tietojen noutaminen Azure Analysis Services | Microsoft Azure"
   description="Opettele muodostamaan ja tietojen noutaminen Analysis Services-palvelimen Azure-tietokannassa."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Tietojen noutaminen Azure Analysis Servicesistä
Kun olet luonut palvelimen Azure ja käyttöön Analysis Services-palvelimen, organisaation käyttäjät voivat muodostaa ja aloita tietoja.

Azure Analysis Services tukee asiakasyhteydet käyttämällä [päivittää objektimallit](#client-libraries). TOM AMO, Adomd.Net tai MSOLAP, välityksellä xmla palvelimeen. Esimerkiksi Power BI-, Power BI Desktopin Excel-tai kolmannen osapuolen asiakassovellus, joka tukee objektimallit.

## <a name="server-name"></a>Palvelimen nimi
Kun luot Azure Analysis Services-palvelimen, Määritä yksilöllinen nimi ja alue, jossa palvelin on luotava. Määritettäessä palvelimen nimi yhteys palvelimeen nimeämisestä mallia on:
```
<protocol>://<region>/<servername>
```
 Protokolla on merkkijono **asazure**, jos alue on alue, jossa palvelin on luotu Uri (esimerkiksi Länsi Yhdysvaltain westus.asazure.windows.net) ja palvelimen nimi on yksilöllinen Serverin alueen nimi.

## <a name="get-the-server-name"></a>Hae palvelimen nimi
Ennen kuin yhdistät, sinun on palvelimen nimen. **Azure** -portaalissa > server > **Yleiskatsaus** > **palvelimen nimi**, kopioi koko palvelimen nimi. Jos organisaation muiden käyttäjien muodostamassa liian tähän palvelimeen, kannattaa Jaa tämä palvelimen nimi heidän kanssaan. Kun määrität palvelimen nimi on käytettävä koko polku.

![Hae palvelimen nimi Azure-tietokannassa](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Power BI Desktopin yhdistäminen

> [AZURE.NOTE] Tämä ominaisuus on esikatselu.

1. [Power BI Desktopin](https://powerbi.microsoft.com/desktop/)valitsemalla **Nouda tiedot** > **tietokantojen** > **Azure Analysis Services**.

2. Liitä **Server**-palvelimen nimi Leikepöydältä.

3. **Tietokannan**, jos tiedät Analysis Services-palvelimen tietokannan tai perspektiivin haluat muodostaa yhteyden nimen, liitä se tähän. Muussa tapauksessa voit voit jätä tämä kenttä tyhjäksi. Voit valita tietokannan tai perspektiivin Valitse seuraavassa näytössä.

4. Jätä oletusasetus **Yhdistä live** valittuna ja paina **Yhdistä**. Jos sinua pyydetään antamaan tiliä, kirjoita organisaation tiliä.

5. **Etsintä**-palvelimeen, laajenna sitten malli tai haluat muodostaa yhteyden perspektiivi ja valitse sitten **Yhdistä**. Mallin tai perspektiivin yhdellä napsautuksella näyttää kaikki näkymän objektit.


## <a name="connect-in-power-bi"></a>Power BI: n Yhdistäminen
1. Luo Power BI Desktopin tiedosto sisältää mallin yhteys palvelimeen.

2. Valitse [Power BI](https://powerbi.microsoft.com)- **Nouda tiedot** > **tiedostot**. Etsi ja valitse tiedosto.


## <a name="connect-in-excel"></a>Excelin yhdistäminen
Yhteyden muodostaminen Azure Analysis Services-palvelimen Excelin tuetaan käyttämällä Nouda tiedot Excel 2016- tai Power Queryn aiemmissa versioissa. [MSOLAP.7 tarjoajan](https://aka.ms/msolap) tarvitaan. Ohjatun taulukon tuonnin avulla PowerPivot yhteyden ei tueta.

1. Excel 2016- **tiedot** , valitse valintanauhan **Nouda ulkoiset tiedot** > **Muista lähteistä** > **Analyysipalveluista**.

2. Ohjattu tietoyhteyden muodostaminen- **palvelimen nimi**Liitä Leikepöydältä palvelimen nimi. Valitse **kirjautumisen tunnistetietoja**, **Käytä seuraavaa käyttäjänimeä ja salasanaa**, ja kirjoita organisaation nimi, esimerkiksi nancy@adventureworks.com, ja salasana.

    ![Yhdistä Excel-kirjautuminen](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. **Valitse tietokanta ja taulukko**Valitse tietokanta ja mallin tai perspektiivi ja valitse sitten **Valmis**.

    ![Valitse Excel-mallin yhteyden](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Yhteysmerkkijono
Muodostettaessa Azure Analysis Servicesin taulukkomuotoisen objektimallia, käytä seuraavaa yhteysmerkkijonoa tiedostomuodot:

###### <a name="integrated-azure-active-directory-authentication"></a>Azure Active Directory-todennusta
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Azure Active Directory-tunnistetiedon välimuistin mahdollisesti noutaa todennusta. Muussa tapauksessa Azure kirjautumisikkuna on näkyvissä.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory käyttöoikeuksien käyttäjänimellä ja salasanalla
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Asiakas-kirjastot
Kun muodostat Azure Analysis Services Excelistä tai muita liittymät esimerkiksi TOM AsCmd, ADOMD.NET, joudut ehkä asentaa uusimman tarjoajan asiakas-kirjastot. Hae uusin versio:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Seuraavat vaiheet
[Palvelimen hallinta](analysis-services-manage.md)

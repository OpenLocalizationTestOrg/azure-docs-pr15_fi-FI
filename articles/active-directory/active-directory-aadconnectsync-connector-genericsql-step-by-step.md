<properties
   pageTitle="Yleinen SQL Connector step by step | Microsoft Azure"
   description="Tässä artikkelissa on ominaisuussäilöjen vaiheittaiset yleinen SQL-Connectorin yksinkertainen HR järjestelmän kautta."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Yleinen vaiheittaiset SQL-yhdistin
Tässä artikkelissa on vaiheittaiset ohjeet. Se luo yksinkertaisia mallitietokanta HR ja käyttää sitä vienti joillakin käyttäjillä ja Ryhmäjäsenyyden.

## <a name="prepare-the-sample-database"></a>Valmistele mallitietokannan
SQL Server-palvelimessa suorittamalla SQL-komentosarjan [Liite A](#appendix-a)-kohdan. Tämä komentosarja luo mallitietokanta nimellä GSQLDEMO. Objektimalli luodun tietokannan näyttää seuraavan kuvan:  
![Objektimalli](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Luo myös haluat muodostaa yhteyden tietokantaan käyttäjä. Tätä vaiheittaista käyttäjä on nimeltään FABRIKAM\SQLUser ja toimialueen sijaitsee.

## <a name="create-the-odbc-connection-file"></a>ODBC-yhteystiedoston luominen
Yleinen SQL-yhdistin yhteyden etäpalvelimeen ODBC avulla. Ensin annettava Luo tiedosto, jossa ODBC-yhteystietoja.

1. Käynnistä ODBC-hallinta-apuohjelma-palvelimeen:  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Valitse **Tiedosto-DSN**-välilehti. Valitse **Lisää …**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. Ruutu ohjain toimii Hieno, niin napsauttamalla sitä ja valitse **Seuraava >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Anna tiedostolle nimi, kuten **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Valitse **Valmis**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Aika määrittämää yhteyttä. Anna tietolähteen hyvä kuvaus ja SQL Server-palvelimen nimi.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Valitse, miten SQL todentamismenetelmä. Tässä tapauksessa emme Käytä Windows-todennusta.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Anna mallitietokannan **GSQLDEMO**nimi.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Pidä kaikki oletus tässä näytössä. Valitse **Valmis**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Voit tarkistaa kaikki toimii odotetulla tavalla, valitsemalla **Testaa tietolähdettä**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Varmista, että testi onnistuu.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. ODBC-kokoonpanotiedosto pitäisi nyt näkyä-tiedosto (DSN).  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Tiedoston syy on, ja voit aloittaa yhdistimen luominen on nyt.

## <a name="create-the-generic-sql-connector"></a>Luo yleinen SQL-yhdistin

1. Valitse synkronointi-palvelun hallinta-Käyttöliittymä, **yhdistimien** ja **Luo**. Valitse **Yleinen SQL (Microsoft)** ja anna sille nimi.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Etsi edellisessä osassa luomasi DSN-tiedosto ja lataa palvelimeen. Anna tunnistetiedot tietokantayhteyden muodostamisessa.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. Tätä vaiheittaista ovat henkilöistä, us ja ilmoittavat, että on kaksi objektityypit, **käyttäjän** ja **ryhmän**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Voit etsiä määritteitä, haluamme yhdistimen esiintyvien itse taulukon katsomalla nämä määritteet. **Käyttäjien** on varattu sana SQL, annettava antamaan hakasulkeisiin [].  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Voit määrittää ankkuri-määrite ja DN-määritteen aika. **Käyttäjät**Käytämme kaksi määritteet käyttäjänimi ja työntekijätunnus yhdistelmä. **Ryhmän**Käytämme Ryhmän_nimi (ei realistisia real aika, mutta se toimii tämän vaiheittaisen kuvauksen suorittamiseksi).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Kaikki määritetyypit voidaan havaita SQL-tietokantaan. Määritteen viittaustyyppi erityisesti ei. Ryhmän objektin tyyppi-annettava OwnerID ja MemberID haluat viitata.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. On valittu kuin edellisessä vaiheessa määritteet viittaus edellyttävät objektityyppi määrite nämä arvot ovat viittaus. Tässä tapauksessa kirjoita käyttäjäobjekti.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Valitse Yleiset-parametrit-sivulla Valitse **vesileima** delta strategia. Kirjoita päivämäärä ja kellonaika-muoto **yyyy-MM-dd hh**myös.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Valitse molemmat objektityypit **määrittäminen osiot ja hierarkiat** -sivulla.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. Valitse **Valitse objektityypit** ja **Valitse määritteitä**, objektityypit ja kaikki määritteet. Valitse **Määritä ankkurit** -sivulla **Valmis**.

## <a name="create-run-profiles"></a>Suorita profiilien luominen

1. Valitse synkronointi-palvelun hallinta-Käyttöliittymä, **yhdistimien**ja **Suorita-profiileista määrittäminen**. Valitse **Uusi profiili**. Olemme Aloita **Täysi tuominen**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Valitse tyyppi **Täysi tuominen (vain vaihe)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Valitse osio **OBJEKTIN = käyttäjä**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Valitse **taulukko** ja kirjoita **[USERS]**. Siirry moniarvoinen objektin laji-osioon ja kirjoita tiedot, kuten seuraavassa kuvassa. Valitse Tallenna vaiheen **Valmis** .
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Valitse **Uusi vaihe**. Tällä hetkellä, valitse **OBJEKTIN = ryhmän**. Viimeiselle sivulle Käytä määritystä, kuten seuraavassa kuvassa. Valitse **Valmis**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Valinnainen: Jos haluat, voit määrittää lisää Suorita profiileja. Näiden vaiheiden käytetään koko Tuo.
7. Valitse lopuksi muuttaminen Suorita-profiileista **OK** .

## <a name="add-some-test-data-and-test-the-import"></a>Lisää testitiedot ja testaa tuominen
Täytä testin otoksen tietokannan tietoja. Kun olet valmis, valitse **Suorita** ja **täysi tuominen**.

Ohessa on käyttäjä, jolla on kaksi puhelinnumerot ja ryhmän joitakin jäsenten kanssa.  
![cs1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Liite A
**Voit luoda mallitietokannan SQL-komentosarja**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```

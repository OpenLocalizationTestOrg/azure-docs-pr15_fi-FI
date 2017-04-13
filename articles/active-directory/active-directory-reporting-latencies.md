<properties
   pageTitle="Azure Active Directory raportoinnin viiveitä | Microsoft Azure"
   description="Ajan lisääminen Azure Active Directory kestää tapahtumat-raportointia varten"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Azure Active Directory-raportin viiveitä

*Näissä ohjeissa on osa [Azure Active Directory raportoinnin Guide](active-directory-reporting-guide.md).*

Raportti                                                  | Pienin  | Keskiarvo    | Enimmäismäärä
------------------------------------------------------- | -------- | ---------- | ----------
**Suojaus-raportit**                                    |          |            |
Epäsäännöllinen kirjautumisen tehtävä                              | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Kirjaudu apuohjelmien tuntemattomista lähteistä                           | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Kirjaudu apuohjelmien useita virheitä jälkeen                        | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Kirjaudu apuohjelmien useita paikkojen kohteesta                      | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Kirjaudu apuohjelmien IP-osoitteista epäilyttävistä aktiviteettiin     | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Kirjaudu apuohjelmien mahdollisesti tartunnan laitteilla                 | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Käyttäjät, joilla on erheellisiin kirjautumisen tehtävä                   | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Käyttäjät, joilla on vuodetun tunnistetiedot                           | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
**Sovelluksen raportit**                                 |          |            |
Tilin tehtävän valmistelu                           | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Tilin valmisteluvirheitä                             | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Sovelluksen käyttö                                       | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Salasanan palauttaminen tila                                | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
**Valvonta- ja käyttöraportit**                            |          |            |
Raportti                                            | 1 minuutti | 15 minuutin | puolessa tunnissa
Salasanan aktiviteetin (Azure AD)                      | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Salasanan tehtävän (käyttäjätietojen hallinta)              | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Salasanan rekisteröinti tehtävän (Azure AD)         | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Salasanan rekisteröinti tehtävän (käyttäjätietojen hallinta) | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Itse palvelun ryhmät tehtävän (Azure AD)                 | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Itse palvelun ryhmät tehtävän (käyttäjätietojen hallinta)         | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
**RMS-raportit**                                         |          |            |
Aktiivisimmat RMS-käyttäjät                                   | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
RMS käyttö                                               | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
RMS-laitteen käyttö                                        | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
Käytössä RMS-sovelluksen käyttö                           | 2 tuntia  | 4 tuntia    | kahdeksan tuntia
**Yksityinen Esikatsele raportteja**                             |          |            |
Kaikkien käyttäjien sisäänkirjautumisessa tehtävä                               | 2 tuntia  | 4 tuntia    | kahdeksan tuntia

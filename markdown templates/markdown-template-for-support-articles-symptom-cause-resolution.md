<properties
   pageTitle="Sivun otsikon, joka näyttää selaimessa-välilehti ja etsinnän tulokset"
   description="Artikkelin kuvaus, joka näytetään purkamisen sivuja ja useimmat hakutulokset"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""
   tags="comma-separates-additional-tags-if-required"/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="title-maximum-120-characters-target-the-primary-keyword"></a>Otsikko (enintään 120 merkkiä, kohde ensisijainen avainsana)

_Käyttämällä 2-3 toissijainen avainsanoja kuvaus._

_Valitse jokin seuraavista kerrotut mukaan käyttämässäsi skenaariossa. Jos artikkeli on käyttöönoton mallin ympäristöstä riippumattomalla tavalla, Ohita tämä._

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../learn-about-deployment-models-both-include.md)]

## <a name="summary-optional-especially-when-the-article-is-short"></a>Yhteenveto (valinnainen, erityisesti silloin, kun on artikkelissa on lyhyt)

- _Kuvaile ongelma._
- _Yhteenveto-osassa on hyvä eri avainsanoja poikkeavat otsikko, mutta muista ei helpottavat hyvin wordy. Virkkeet olisi flow hyvin ja on helppoa ymmärtää._
- _Poikkeukset (valinnainen) - luettelosta haluamasi tilanteita, jotka eivät kuulu tämän artikkelin. Esimerkiksi "Linux/OSS tilanteita, joissa ei ole kattaa tämän artikkelin"._

_Laskutuksen aiheeseen artikkeli on lisätä seuraavat viestin (alla oleva huomautus on hieman eri kuin tässä artikkelissa alareunassa):_
> [AZURE.NOTE] Jos tarvitset apua milloin tahansa tämän artikkelin, ota [yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ongelmaa saat ratkaista nopeasti.

_Jos se ei ole laskutuksen ohjeartikkeliin, käytä seuraavaa viittausta:_
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Oire

- _Mitä käyttäjä yrittää suorittaa?_
- _Mitä epäonnistui?_
- _Mitä järjestelmien ja ohjelmiston käyttäjän olet käyttänyt?_
- _Mitä virhe viestit on voitu osoitettu?_
- _Lisää näyttökuva Jos mahdollista._

## <a name="cause"></a>Syy

- _Mikä aiheuttaa ongelman._

## <a name="solution"></a>Ratkaisu

- _Näyttökuvat Jos mahdollista._
- _Jos määritettynä on useita ratkaisuja tallennettava monimutkaisuus järjestyksessä ja antaa ohjeita siitä, miten voit valita näytössä näkyviä ne._

| <em>Versio 1: Artikkelin käyttöönoton mallin ympäristöstä riippumattomalla tavalla tulee</em> | <em>2-versio: Resurssienhallinta ja perinteinen ovat pääosin samat</em> | <em>3-versio: Resurssienhallinta ja perinteinen tehdään enimmäkseen eri tavalla. <br />Tässä tapauksessa käyttää <a href="https://github.com/Azure/azure-content-pr/blob/master/contributor-guide/custom-markdown-extensions.md#simple-selectors">Github yksinkertainen valitsimien-tekniikkaa</a>. <br />Huomautus: AM artikkeleita KÄDESSÄ poikkeukset ja ARM/Classic-valitsin ei kannata käyttää.</em> |
|:------------------------------------------------------|:-----------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <p><h3>Ratkaisu 1</h3><em>(helpoin ja mahdollisimman tehokasta)</em></p><ol><li>[Vaiheen 1]</li><li>[Vaiheessa 2]</li></ol><p><h3>Ratkaisu 2</h3><em>(mitä vähemmän yksinkertainen tai tehokas)</em></p><ol><li>[Vaiheen 1]</li><li>[Vaiheessa 2]</li></ol><br /><br /><br /><br /><br /><br /><br /><br /> | <p><h3>Ratkaisu 1</h3><em>(helpoin ja mahdollisimman tehokasta)</em></p><ol><li>[Vaiheen 1]</li><li>Jos käytät perinteistä käyttöönotto-mallin [tehdä tämän].<br />Jos käytät Resurssienhallinta käyttöönotto-mallin [tehdä tämän].</li><li>[Vaihe 3]</li></ol><p><h3>Ratkaisu 2</h3><em>(mitä vähemmän yksinkertainen tai tehokas)</em></p><ol><li>[Vaiheen 1]</li><li>Jos käytät perinteistä käyttöönotto-mallin [tehdä tämän].<br />Jos käytät Resurssienhallinta käyttöönotto-mallin [tehdä tämän].</li><li>[Vaihe 3]</li></ol> | <img src="media/markdown-template-for-support-articles-symptom-cause-resolution/rm-classic.png" alt="ARM-Classic"><p><h3>Ratkaisu 1</h3><em>(helpoin ja mahdollisimman tehokasta)</em></p><ol><li>[Vaiheen 1]</li><li>[Vaiheessa 2]</li></ol><p><h3>Ratkaisu 2</h3><em>(mitä vähemmän yksinkertainen tai tehokas)</em></p><ol><li>[Vaiheen 1]</li><li>[Vaiheessa 2]</li></ol><br /><br /><br /><br /> |

## <a name="next-steps"></a>Seuraavat vaiheet
_Esimerkiksi tämän osan Jos betonin on 1 – 3, käyttäjän on otettava erittäin olennaisia vaiheisiin. Poista, jos ei ole vaiheisiin. Tämä ei ole linkkejä luettelo sijainti. Jos lisäät vaiheisiin linkkejä, varmista, että tekstin kerrotaan, miksi seuraavat vaiheet ovat asiaa / tärkeä._

_Laskutuksen aiheeseen artikkeli on lisätä seuraavat viestin (alla oleva huomautus on hieman erilainen kuin sen alussa on tämän artikkelin):_
> [AZURE.NOTE] Jos on edelleen muita kysymyksiä, ota [yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ongelmaa saat ratkaistu nopeasti.

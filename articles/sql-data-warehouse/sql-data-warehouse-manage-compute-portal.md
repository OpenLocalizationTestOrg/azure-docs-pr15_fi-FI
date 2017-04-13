<properties
   pageTitle="Hallitse Laske virtaa Azure SQL-tietovarasto (Azure portaalin) | Microsoft Azure"
   description="Azure portaalin tehtävien hallintaan Laske power. Skaalaa Laske resurssien säätämällä DWUs. Tai keskeyttäminen ja jatkaminen Laske resurssien kustannusten tallentamiseen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Laske virtaa Azure SQL-tietovarasto (Azure portaalin) hallinta

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShellin](sql-data-warehouse-manage-compute-powershell.md)
- [MUILLE KÄYTTÄJILLE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaalaa suorituskyvyn käyttämällä laajentaminen Laske resurssit ja muistin täyttämään havainnollistamiseen muuttaminen tarpeisiin. Tallenna kustannukset skaalauksen takaisin resurssien mukaan huippu aikoja tai keskeyttäminen Laske aikana kokonaan. 

Tämä joukko tehtäviä käyttää Azure-portaalissa:

- Skaalaa suorittaminen
- Keskeytä suorittaminen
- Jatka suorittaminen

Lisätietoja on artikkelissa [Hallitse Laske yleiskatsaus][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skaalaa Laske power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Voit muuttaa Laske resurssien seuraavasti:

1. Avaa [Azure portal][], avaa ensin tietokanta ja **asteikko**.

    ![Asteikko][1]

1. Skaalaa-sivu-liukusäädintä vasemmalle tai oikealle muuta DWU.

    ![Siirrä liukusäädin][2]

1. Valitse **Tallenna**. Näyttöön tulee vahvistussanoma. Valitse **Kyllä** Vahvista tai **ei** voi peruuttaa.

    ![Valitse Tallenna][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Keskeytä suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Voit pysäyttää tietokannan:

1. Avaa [Azure portal][] ja avaa tietokanta. Huomaa, että tila on **Online**. 

    ![Online-tila][6]

1. Voit keskeyttää suorittaminen ja muistin resursseja, valitse **Keskeytä**ja sitten tulee vahvistussanoma tulee näkyviin. Valitse **Kyllä** Vahvista tai **ei** voi peruuttaa.

    ![Vahvista keskeyttäminen][7]

1. **Kun SQL-tietovarasto käynnistyy tietokannan, tila keskeytetään.**
2. Kun tila on **keskeytetty**, Keskeytä-toiminto on valmis ja olet peritään enää DWUs varten.

    ![Keskeytä-tila][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Jatka suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Kun haluat jatkaa tietokannan:

1. Avaa [Azure portal][] ja avaa tietokanta. Huomaa, että tila on **keskeytetty**. 

    ![Keskeytä-tietokanta][4]

1. Kun haluat jatkaa tietokannan napsauttamalla **Käynnistä-painiketta**ja sitten näyttöön tulee vahvistussanoma. Valitse **Kyllä** Vahvista tai **ei** voi peruuttaa.

    ![Vahvista ansioluettelo][5]

1. SQL-tietovarasto aloittaa tietokannan, kun tila on "Resuming".
2. Kun tilana on **online**-tietokanta on valmis.

    ![Online-tila][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja on artikkelissa [hallinnan yleiskatsaus][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Hallinnan yleiskatsaus]: ./sql-data-warehouse-overview-manage.md
[Laske yleiskatsaus hallinta]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

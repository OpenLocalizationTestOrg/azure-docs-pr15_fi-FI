<properties
    pageTitle="Muistivirhe (OOMAA) - rakenteen asetukset ulos | Microsoft Azure"
    description="Korjaa Hadoop HDInsight poissa-rakennetiedoston kyselystä muistivirhe (OOMAA). Asiakkaan skenaariossa on useita suuria taulukoita kyselyn."
    keywords="Virhe, OOMAA, rakenne asetuksia ulos"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Korjata poissa muistia (OOMAA)-virheen Azure Hdinsightiin Hadoop rakenteen muistiasetukset

Yksi yleisiin ongelmiin asiakkaille sekä hymiö on näyttöön tulee poissa muistia (OOMAA) virheilmoitus käyttämällä rakenne. Tässä artikkelissa kuvataan asiakkaan tilanne ja rakenne-asetuksia, on suositeltavaa voit korjata ongelman.

## <a name="scenario-hive-query-across-large-tables"></a>Skenaario: Rakenne kyselyn suurten taulukoiden välillä

Asiakasta suorittanut kyselyn jäljempänä rakenne.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Jotkin nuances kyselyn:

* T1 on suuri taulukkoon, table1-taulukon, jossa on paljon MERKKIJONON saraketyypit alias.
* Muiden taulukoiden eivät ole, suuri, mutta se on suuri määrä sarakkeita.
* Kaikki taulukot ovat liittyminen toisiaan joissakin tapauksissa useita sarakkeita table1-taulukon ja muiden kanssa.

Kun asiakkaan suoritettiin rakenteen käyttäminen MapReduce 24 solmun A3 klusterin kyselyn, kysely suoritettiin noin 26 minuuttia. Asiakkaan huomannut varoitus seuraavista ilmoituksista, kun kysely on suoritettu rakenteen käyttäminen MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Kyselyn valmis suoritat noin 26 minuuttia, koska asiakas ohittaa nämä varoitukset ja sen sijaan aloittaminen keskittyminen parantamisesta tämän kyselyn suorituskykyä edelleen.

Asiakkaan kuullut [Hadoop HDInsight optimointi rakenne kyselyt](hdinsight-hadoop-optimize-hive-query.md)ja päättänyt Tez suorittamisen ohjelma käyttää. Kun saman kyselyn suoritettiin Tez-asetus käytössä kyselyn suorituksen 15 minuutin ja palautti seuraavan virheilmoituksen:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Asiakas päättää käyttämään suurempi AM (eli D12) suunnittelemassa isommaksi AM on keon tilaa. Asiakkaan jatkuu lisäksi voit tarkastella virheen. Asiakkaan saavuttanut pyydä apua virheenkorjaus ongelman HDInsight-ryhmän.

## <a name="debug-the-out-of-memory-oom-error"></a>Poissa muistia (OOMAA)-virheen korjaaminen

Sekä tuen ja suunnitteluryhmät ryhmiä yhdessä löysi vähintään yhden poissa muistia (OOMAA)-virheen vuoksi ongelmista on [tunnettu ongelma Apache JIRA kuvattu](https://issues.apache.org/jira/browse/HIVE-8306). Valitse JIRA kuvaus:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Olemme varmistaneet, **hive.auto.convert.join.noconditionaltask** on varmasti määrittämällä arvoksi **Tosi** katsomalla rakenteen site.xml tiedosto:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Varoitus ja JIRA perusteella Microsoftin hypoteesien oli kartan liittyä Java keon tilaa OOMAA virheen syy. Jotta olemme dug tarkempaa ongelman kyselyjä.

Blogimerkinnän [HDInsight Hadoop kuitenkaan muistiasetukset](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)esitetyllä kun Tez suorittamisen ohjelma on käyttänyt keon tilaa kuuluu Tez säilö. Katso jäljempänä kuvaava Tez säilö muistin kuva.

![Tez säilö muistin kaavion: muistivirhe OOMAA ulos rakenne](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Blogimerkinnän ehdotetaan, kun seuraavat kaksi muistin asetukset Määritä keon säilö muisti: **hive.tez.container.size** ja **hive.tez.java.opts**. Tutustu kokemus-OOMAA poikkeuksen ei tarkoita säilö on liian pieni. Se tarkoittaa Java keon koko (hive.tez.java.opts) on liian pientä. Jotta aina, kun näet OOMAA, voit aloittaa niin, että **hive.tez.java.opts**. Tarvittaessa voit joutua lisäämään **hive.tez.container.size**. **Java.opts** -asetus on oltava noin 80 % **container.size**.

> [AZURE.NOTE]  Asetus **hive.tez.java.opts** on aina oltava pienempi kuin **hive.tez.container.size**.

Koska D12 koneen on 28 Gigatavun muistia, on päättänyt säilön koko on 10 Gigatavua (10240 Mt) ja määrittää 80 % java.opts. Tämä määritetty rakenne-konsolissa alla tämän asetuksen avulla:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Näiden asetusten perusteella kyselyn suoritettiin kymmenen minuuttia.

## <a name="conclusion-oom-errors-and-container-size"></a>Tekemistä: OOMAA virheet ja säilön koko

OOMAA virhe ei välttämättä merkitse säilö on liian pieni. Sen sijaan kannattaa määrittää muistiasetuksia niin, että keon koko kasvaa ja on oltava vähintään 80 % säilö muistikoko.

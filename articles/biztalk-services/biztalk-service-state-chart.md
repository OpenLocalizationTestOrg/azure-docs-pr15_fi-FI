<properties 
    pageTitle="Tehtävien sallittuja tilat tai tilat BizTalk Services-palveluissa | Microsoft Azure" 
    description="Eri MAb tilan sallitut toiminnot/toiminnot: lopettaminen, Käynnistä, uudelleen, keskeyttää, jatkaa, poista, skaalata, Päivitä määritys ja varmuuskopioiminen määrittäminen" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>



# <a name="biztalk-services-service-state-chart"></a>BizTalk Services: Palvelun tila-kaavio
BizTalk-palvelun nykyisen tilan mukaan on toiminnoista, joita voi tai ei voi suorittaa BizTalk-palvelusta.

Esimerkiksi valmistella uuden BizTalk palvelun Azure perinteinen-portaalissa. Kun asennus on suoritettu onnistuneesti, BizTalk-palvelu on aktiivisena. Aktiivinen-tilassa voit lopettaa BizTalk-palvelun. Jos Stop on tarkistettu, BizTalk-palvelun siirtyy pysäytetty-tilaan. Lopeta epäonnistuu, jos BizTalk-palvelun siirtyy StopFailed tila. StopFailed-tilassa voit käynnistää BizTalk-palvelun. Jos yrität toiminnon, jota ei sallita, kuten ansioluettelo BizTalk-palvelua seuraava virhesanoma tulee näyttöön:

**Toiminto ei ole sallittu**

Valmistelu BizTalk palvelu, siirry [BizTalk Services: valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Seuraavissa taulukoissa luetellaan toiminnot tai toiminnot, jotka voidaan suorittaa, kun BizTalk-palvelu on tietyn tilassa. ✔ tarkoittaa, että toiminto on sallittu siinä tilassa. Tyhjän tarkoittaa toimintoa ei voi suorittaa siinä tilassa.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Aloittaa, lopettaa, uudelleen, keskeyttää, Resume, ja poistaa toiminnot
<table border="1">
<tr>
        <th colspan="15">Toiminnon tai toiminto</th>
</tr>

<tr>
        <th rowspan="18">BizTalk palvelun tilan</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Aloittaminen</th>
        <th>Seis</th>
        <th>Käynnistä uudelleen</th>
        <th>Keskeytys</th>
        <th>Ansioluettelo</th>
        <th>Poista</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktiivinen</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Ei käytössä</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Keskeytetty</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Pysäytetty</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Palvelun päivitys epäonnistui</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Mittakaava, päivitys määritys ja varmuuskopion toiminnot
<table border="1">
<tr>
        <th colspan="15">Toiminnon tai toiminto</th>
</tr>

<tr>
        <th rowspan="18">BizTalk palvelun tilan</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Asteikko</th>
        <th>Päivitä määritys</th>
        <th>Varmuuskopiointi</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktiivinen</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Ei käytössä</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Keskeytetty</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Pysäytetty</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Palvelun päivitys epäonnistui</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Katso myös
- [BizTalk Services: Valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Kehittäjä, Basic, Vakio ja Premium versiot kaavio](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Varmuuskopiointi ja palauttaminen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk Services: rajoittaminen](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 

**Virtuaalikoneen levyjen: kohden tilin rajoitukset**

Resurssi|Oletusarvoinen enimmäismäärä
---|---
Levyn kapasiteetti tiliä kohti|35 TT
Yhteensä tilannevedoksen kapasiteetin tiliä kohti|10 TT
Maks-kaistanleveyden per tili (tunkeutumisen + juniin<sup>1</sup>)|< = 50 Gbps

<sup>1</sup> *Tunkeutumisen* viittaa kaikki tiedot (pyyntöjen) on lähetetty tallennustilan-tilille. *Juniin* viittaa kaikki tiedot (vastaukset) vastaanoteta tallennustilan tilin.

**Virtuaalikoneen levyjen: kohden levyn rajoitukset**

Levyn Premium tallennustyyppi | P10 | Ñ20 = | P30
---|---|---|---
Levyn koko | 128 giB | 512 giB | Vähintään 1 024 giB (1 TT)
Maks IOPS levyä kohden | 500 | 2300 | 5000
Suurin nopeus levyä kohden | 100 Mt sekunnissa | 150 Megatavua sekunnissa | 200 Mt sekunnissa
Levyjen tiliä ja tallennustilaa enimmäismäärä | 280 | 70 | 35

**Virtuaalikoneen levyjen: kohden AM rajoitukset**

Resurssi|Oletusarvoinen enimmäismäärä
---|---
Maks IOPS AM kohden|80,000 IOPS kanssa GS5 AM<sup>1</sup>
Suurin nopeus AM kohden|2 000 Mt/s GS5 AM<sup>1</sup>

<sup>1</sup> Lisätietoja [AM koko](../articles/virtual-machines/virtual-machines-linux-sizes.md) n AM muita kokoja koskevat rajoitukset. 

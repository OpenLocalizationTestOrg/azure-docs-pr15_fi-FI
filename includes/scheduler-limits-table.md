Seuraavassa taulukossa kuvataan kunkin pää kiintiön, rajoitukset, oletusarvot ja Azure ajoituksen ylikuormitustilan.

|Resurssi|Raja kuvaus|
|---|---|
|**Työn kokoa**|Työn koko on 16K. Jos HYLLYTETTY tai KORJAUSTIEDOSTON johtaa työ, joka on suurempi kuin nämä raja-arvot, 400 virheelliset pyytää Tilakoodi palautetaan.|
|**Pyyntö URL-koko**|Pyyntö URL-osoitteen enimmäiskoko on 2 048 merkkiä.|
|**Kooste otsikon koko**|Suurin kooste otsikon koko on 4096 merkkiä.|
|**Otsikon määrä**|Suurin otsikon määrä on 50 otsikot.|
|**Perusrakenteen koko**|Suurin perusrakenteen koko on 8192 merkkiä.|
|**Toistuminen tekstialue**|Suurin toistuminen aikaväli on 18 kuukauden.|
|**Aika aloitusaika**|"Aika aloitusaika" enimmäismäärä on 18 kuukauden.|
|**Työhistoria**|Vastauksen teksti tallennettu Työhistoria on 2 048 tavua.|
|**Taajuus**|Oletusarvoinen enimmäismäärä korkojakso kiintiö on ilmainen työn kokoelman 1 tunti ja minuutin normaalin työn kokoelman. Max korkojakso on määritettävä työn kokoelman on pienempi kuin suurin. Työn sivustokokoelman kaikki työt on rajoitettu työn sivustokokoelman arvon. Jos yrität luoda työn sitä, kuinka usein työ-sivustokokoelman kuin suurempi usein pyynnön epäonnistuu 409 ristiriidan tilakoodi.|
|**Projektit**|Oletusarvoinen enimmäismäärä työt kiintiö on 5 työt vapaa työn kokoelman ja normaalin työn kokoelman 50 työt. Töiden enimmäismäärä on määritettävä työ-sivustokokoelman. Työn sivustokokoelman kaikki työt on rajoitettu työn sivustokokoelman arvon. Jos yrität luoda lisää töitä kuin suurin työt-kiintiön pyyntö ei onnistu, 409 ristiriidan tilakoodi.|
|**Työn historia säilytys**|Työhistoria säilyttää enintään 2 kuukautta tai viimeksi 1000 suorituskerran ylöspäin.|
|**Valmiit ja kohtasi virheen työn viivyttäminen**|Valmiit ja kohtasi virheen työt säilyvät 60 päivän ajan.|
|**Aikakatkaisu**|On staattinen (ei voi muuttaa) pyynnön aikakatkaisu 60 sekuntia HTTP-toimintoja. Pidempi käynnissä toimintoja noudata HTTP asynkroninen protokollat; esimerkiksi palauttaa 202 välittömästi, mutta jatkaa työskentelyä taustalla.|

## <a name="scenario"></a>Skenaario

Arvovirtakarttojen luominen NSGs paremmin, tämä asiakirja käyttämällä alla skenaario.

![VNet-skenaario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Tässä skenaariossa luot NSG kunkin aliverkon **TestVNet** näennäinen verkossa seuraavalla tavalla: 

- **NSG edusta**. Edustan NSG käytetään *edusta* -osoitteiden ja sisältää kaksi sääntöjä:  
    - **rdp-säännön**. Tämä sääntö sallii RDP-liikenne paikalliseen *edusta* -aliverkon.
    - **web-säännön**. Tämä sääntö sallii HTTP-liikenne paikalliseen *edusta* -aliverkon.
- **NSG Taustajärjestelmä**. Edellinen loppuun NSG käytetään *Taustajärjestelmä* aliverkon ja sisältää kaksi sääntöjä: 
    - **sql-säännön**. Tämän säännön avulla vain *edusta* aliverkosta SQL-liikenne.
    - **web-säännön**. Tämä sääntö estää kaikki internet sidottu tietoliikenteen *taustaan* aliverkosta.

Sääntöjen yhdistelmä Luo DMZ kaltaisessa tilanne, jossa uudelleen aliverkon vastaanottaa vain saapuvan liikenteen SQL edusta-aliverkosta, ja ei ole Internet-käyttöoikeutta, kun edusta-aliverkon yhteydessä Internetiin, ja vastaanottaa saapuvia pyyntöjen vain.
 

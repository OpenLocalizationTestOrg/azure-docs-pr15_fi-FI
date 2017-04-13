## <a name="scenario"></a>Skenaario

Arvovirtakarttojen luominen UDRs paremmin, tämä asiakirja käyttämällä alla skenaario.

![KUVAN KUVAUS](./media/virtual-network-create-udr-scenario-include/figure1.png)

Tässä skenaariossa luot yhden UDR *eteen Lopeta aliverkon* ja toinen UDR *Edellinen Lopeta aliverkon* seuraavalla tavalla: 

- **UDR FrontEnd**. Edustan UDR käytetään *edusta* -osoitteiden ja sisältävät yhden reitin:  
    - **RouteToBackend**. Tätä vaihtoehtoa, toimi lähettää kaikki liikenne uudelleen aliverkon ja **FW1** virtuaalikoneen.
- **UDR Taustajärjestelmä**. Edellinen loppuun UDR käytetään *Taustajärjestelmä* aliverkon ja sisältävät yhden reitin: 
    - **RouteToFrontend**. Tätä vaihtoehtoa, toimi lähettää kaikki liikenne edusta-aliverkosta **FW1** virtuaalikoneen.

Nämä tiet yhdistelmä varmistat, että kaikki liikenne tarkoitettu yhden aliverkon reititetään **FW1** virtuaalikoneen, jossa on käytössä virtual laitteen. Myös tarvitse ottaa käyttöön kyseisen AM, jotta se voi vastaanottaa muiden VMs tarkoitettu liikenne IP-välityksen.

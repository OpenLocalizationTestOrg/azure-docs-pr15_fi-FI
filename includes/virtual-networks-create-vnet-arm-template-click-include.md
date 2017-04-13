## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Ottaa käyttöön ARM-mallin avulla napsauttamalla käyttöönotto

Voit käyttää ennalta määritettyjä ARM mallien lataamisen Microsoftin github säilöön ja avaa yhteisöön. Mallit voidaan käyttöön suoraan github, ulos tai ladata ja muokata tarpeen mukaan. Malli, joka luo VNet kahden aliverkon ottamaan noudattamalla seuraavia ohjeita.

1. Siirry selaimella, [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Mallien luetteloa alaspäin ja valitse **101-vnet-kaksi-aliverkosta**. Valitse **README.md** -tiedosto, alla kuvatulla tavalla.

    ![READEME.md tiedoston github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Valitse **Ota käyttöön Azure**. Kirjoita tarvittaessa Azure tunnistetietosi. 
4. **Parametrit** -sivu-haluat avulla voit luoda uuden VNet arvot ja valitse sitten **OK**. Alla olevassa kuvassa Microsoftin skenaarion arvot.

    ![ARM-Malliparametrit](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Valitse **resurssiryhmä** ja resurssien ryhmä, johon haluat lisätä VNet, tai valitsemalla Lisää uusi resurssiryhmä VNet **Luo uusi** . Alla olevassa kuvassa resurssin uusi resurssiryhmä kutsutaan **TestRG**ryhmäasetukset.

    ![Resurssiryhmä](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Tarvittaessa muuttaa oman VNet **tilaus** ja **sijainti** -asetuksia.
6. Jos et halua nähdä VNet **Startboard**-ruuduksi, poista käytöstä **Startboard PIN-tunnuksen**.
5. Valitse **Leagl ehdot**, Lue ehdot ja valitse Hyväksy **Osta** . 
6. Valitse **Luo** VNet luomiseen.

    ![Esikatselu-portaalissa lähettävä käyttöönotto-ruutu](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Kun asennus on valmis, valitse **TestVNet** > **kaikkia asetuksia** > **aliverkosta** aliverkon ominaisuuksia, näet alla kuvatulla tavalla.

    ![Luo VNet esikatselu-portaalissa](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)
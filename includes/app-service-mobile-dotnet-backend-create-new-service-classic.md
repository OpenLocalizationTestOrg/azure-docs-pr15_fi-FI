1. Kirjaudu sisään osoitteessa [Azure Portal].

2. Valitse **+ Uusi** > **Web + Mobile** > **Mobile-sovelluksen**, Anna nimi Mobile-sovelluksen Taustajärjestelmä.

3. **Resurssiryhmä**-aiemmin resurssiryhmä tai luoda uuden (joko on sama nimi kuin sovelluksesi.) 
 
    Voit valita toisen sovelluksen palvelusopimus tai luoda uuden. Saat lisätietoja sovelluksen Services suunnitelmien ja eri hinnat uuden suunnitelman luominen taso ja nähdä haluamasi sijaintisi [Azure App palvelun suunnitelmien perusteellisempaa yleiskatsaus](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Sovelluksen palvelusopimus**(-) [Standard taso](https://azure.microsoft.com/pricing/details/app-service/)oletusarvo-palvelupaketti on valittuna. Voit valita myös vaihtaa palvelupakettiani tai [luoda uuden](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Sovelluksen palvelun suunnitelman asetukset määrittävät [sijainti-ominaisuuksia, kustannus- ja Laske resurssit](https://azure.microsoft.com/pricing/details/app-service/) -sovellukseen kytketty. 

    Kun päätät osalta, valitse **Luo**. Tämä luo Mobile-sovelluksen Taustajärjestelmä. 
    
6. Valitse uusi Mobile-sovelluksen taustatietokannan, **asetukset** -sivu, **Pika-aloitus** > asiakas-sovelluksen ympäristön > **Yhdistä tietokannan**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Valitse **Lisää tietoyhteys** -sivu **SQL-tietokantaan** > **Luo uusi tietokanta**, Kirjoita tietokannan **nimi**, valitse hinnoittelu taso ja valitse sitten **palvelimeen**.  Voit käyttää uuden tietokannan. Jos sinulla on jo tietokannan entisellä paikallaan, voit käyttää sen sijaan **Käytä aiemmin luotua tietokantaa**. Käytä tietokannan eri sijainnissa ei kannata kaistanleveyden kustannuksia ja uudempi versio viive vuoksi.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. Yksilöivä palvelimen nimi Kirjoita **palvelimen nimi** -kenttään **Uusi palvelin** -sivu, anna sisäänkirjautuminen- ja salasana, valitse **Salli azure services yhteyden palvelimeen**ja sitten **OK**. Tämä luo uusi tietokanta.

9. Takaisin sisään **Lisää tietoyhteys** -sivu valitsemalla **yhteysmerkkijonon**, Kirjoita tietokannan kirjautuminen ja salasana-arvot ja valitse **OK**. Odota muutama minuutti tietokannan käyttöön onnistuneesti ennen kuin jatkat.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

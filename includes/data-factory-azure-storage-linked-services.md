## <a name="azure-storage-linked-service"></a>Azure-tallennustilan linkitetty palvelu

**Azuren tallennustilaan linkitetty palvelun** voit Azure tallennustilan asiakkaan linkittäminen Azure tietojen factory mukaan **tili avaimen**avulla. Tämä on Yleinen käytön tietojen factory Azure-tallennustilan. Seuraavassa taulukossa on kuvaus JSON elementtien tietyn Azuren tallennustilaan linkitetty-palveluun.

| Ominaisuus | Kuvaus | Pakollinen |
| :-------- | :----------- | :-------- |
| tyyppi | Type-ominaisuus on määritettävä: **AzureStorage** | Kyllä |
| connectionString | Määritä tiedot Azure tallennustila connectionString-ominaisuuden muodostamiseen. | Kyllä |

Näytä/kopio tilin avain vaiheet seuraavasta artikkelista saat Azure-tallennustilan: [näkymän, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Esimerkki:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure-tallennustilan Sas linkitetty palvelu  
Jaettu käyttö allekirjoituksen (SAS) tarjoaa valtuutetun resurssien tallennustilan tilissäsi. Tämä tarkoittaa, että voit myöntää asiakkaan rajoitetut käyttöoikeudet objektien tallennustila-tilisi vuosipoiston annettuna kautena aika ja käyttöoikeudet-määritetyn eikä sinun tarvitse jakaa tilin pikanäppäimet. Suojaussidokset on URI, sen kyselyparametrit kattaa kaikki tarvittavat tiedot todennetut tallennustilan resurssin käyttöoikeutta. Tallennustilan resursseja Suojaussidokset käyttämään asiakkaan tarvitsee vain siirtää Suojaussidokset oikeaa konstruktoria tai menetelmää. SAS yksityiskohtaisia tietoja on artikkelissa [jaettu Access allekirjoitukset: SAS-mallin tietoja](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Azure-tallennustilan SAS linkitetty-palvelun avulla voit linkittää Azure tietojen factory Azure-tallennustilan tilin käyttämällä jaettu Access allekirjoitus (SAS). Tietoja factory avulla rajoitettu, aika-sidottu käyttää muistiin resursseja kaikki/koskevien (blob/säilö). Seuraavassa taulukossa on kuvaus JSON elementtien tietyn Azure-tallennustilan SAS linkitetty-palveluun. 

| Ominaisuus | Kuvaus | Pakollinen |
| :-------- | :----------- | :-------- |
| tyyppi | Type-ominaisuus on määritettävä: **AzureStorageSas**  | Kyllä |
| sasUri | Määritä jaetut Access allekirjoituksen URI Azuren tallennustilaan-resurssit, kuten blob, säilön tai taulukko. Katso lisätietoja alla olevista muistiinpanoista. | Kyllä | 


**Esimerkki:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

**SAS URI**luotaessa ottaen huomioon seuraavasti:  

- Azure Data Factory tukee vain **Palvelun SAS**ei ole tiliä Suojaussidokset. Katso lisätietoja nämä kaksi tiedostomalleja [Tyypit, Jaetut Access allekirjoitukset](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) .
- Tarvittavat luku-/ kirjoitusoikeudet **käyttöoikeudet** on määritettävä objektien miten linkitetyn palvelu (luku, kirjoita luku-/ kirjoitusoikeudet) käytetään tietojen factory perusteella.
- **Voimassaolon ajan** on määritetty oikein. Varmista, että Azuren tallennustilaan objektien käyttöoikeus ei vanhene putkijohto aktiivinen ajan kuluessa.
- URI luodaanko oikean säilö/blob tai taulukon tasolla tarpeen mukaan. SAS URI-tunnusta Azure-blob avulla Data Factory-palvelu voi käyttää kyseisen tietyn blob. SAS URI-tunnusta Azure-blob-säilö avulla käydä läpi kyseisen säilön BLOB Data Factory-palvelun. Jos haluat säätää access enemmän tai vähemmän objekteja myöhemmin tai päivitä SAS URI, muista päivittää linkitetyn palvelun uusi URI.   

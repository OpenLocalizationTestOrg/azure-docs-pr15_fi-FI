<properties
    pageTitle="Muuttaa avaimen säilö vuokraajan tunnus, kun tilauksen siirtää | Microsoft Azure"
    description="Vaihtamisesta avaimen säilö vuokraajan tunnus, kun tilaus on siirretty toiseen vuokraajalle"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Muuttaa avaimen säilö vuokraajan-tunnus, kun tilauksen siirtäminen
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>K: tilauksen on siirretty vuokraajasta A vuokraajan B. Miten muuttaminen vuokraajan tunnus Omat aiemmin avaimen säilö ja määritetty oikein käyttöoikeusluettelot vuokraajan B ansaitun?

Kun luot uuden avaimen säilö tilauksen, se on sidottu automaattisesti kyseisen tilauksen Azure Active Directory-vuokraajan tunnus. Tapahtumat kaikki access-käytäntö on liitetty myös tämän vuokraajan ID-tunnuksellasi. Kun siirrät Azure tilauksen vuokraajan A vuokraajan B, että aiemmin avaimen vaults eivät ole käytettävissä mukaan ansaitun (käyttäjät ja sovellukset) alihallinnan B. Voit korjata tämän ongelman, sinun on:

- Muuta vuokraajan tunnus liittyvät kaikki olemassa olevat avaimen vaults tähän tilaukseen-asiakasympäristöön B.
- Poista kaikki olemassa olevat Accessin käytännön merkinnät.
- Lisää uusi access-käytäntö tietueet, jotka liittyvät vuokraajan B.

Esimerkiksi jos sinulla on avaimen säilö 'myvault' tilauksessa, joka on siirretty vuokraaja A vuokraajan B, Tässä voit muuttaa tämän avaimen säilö vuokraajan tunnuksen ja poistamisesta vanha käytäntöjen.

<pre>
$vaultResourceId = (Hae AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() määrittäminen AzureRmResource - ResourceId $vaultResourceId-ominaisuudet $vault. Ominaisuudet:
</pre>

Koska tätä säilö alihallinnan A ennen siirtoa, **$vault alkuperäisen arvon. Properties.TenantId** vuokraajan a, kun **(Get-AzureRmContext). Tenant.TenantId** on vuokraaja B.

Oman säilö on liitetty oikea vuokraajan-tunnus ja vanha access käytännön merkinnät poistetaan, määrittää uuden Accessin käytännön [Määrittäminen AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx)tapahtumia.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos sinulla on kysyttävää Azure avaimen säilö, tutustu [Azure avaimen säilö keskustelupalstoilla](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

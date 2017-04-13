<properties 
pageTitle="Cloud Services-roolin config XPath huijata taulukon | Microsoft Azure" 
description="XPath asetuksia voit cloud palvelun roolin config voit näyttää asetuksia kuin ympäristömuuttuja." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Näyttää roolin asetukset kuin ympäristömuuttuja XPath kanssa

Cloud palvelun työntekijä tai web-roolin palvelun määritystiedosto voit näyttää runtime määritysten arvot kuin ympäristömuuttujia. Seuraavat XPath-arvot ovat tuettuja (jotka vastaavat arvot API).

XPath-arvot ovat käytettävissä myös [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) kirjaston kautta. 

## <a name="app-running-in-emulator"></a>Sovelluksen emulaattorin käynnissä

Ilmaisee, että sovellus on käynnissä emulaattori.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Koodi  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Käyttöönoton tunnus

Hakee esiintymän käyttöönoton-tunnus.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Koodi  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Roolin tunnus 

Hakee nykyisen esiintymän rooli tunnuksen.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Koodi  | var-id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Päivitä toimialueen

Hakee Päivitä toimialueen esiintymän.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Koodi  | var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Virhe toimialueen

Hakee vika toimialueen esiintymän.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Koodi  | var suodatuspalvelu = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Roolinimi

Hakee esiintymien roolinimi.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Koodi  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Config-asetus

Hakee määritettyyn määritys-asetuksen arvoa.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Koodi  | var-asetus = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Paikallinen tallennus polku

Hakee paikallisen säilön polun esiintymän.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Koodi  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Paikallinen tallennustilan koko

Hakee paikallisesta säilöstä esiintymän kokoa.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Koodi  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Päätepisteen protokolla 

Hakee esiintymän päätepisteen-protokollaa.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Koodi  | var-prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokollan; |

## <a name="endpoint-ip"></a>Päätepisteen IP

Hakee määritettyyn päätepisteen IP-osoite.

| Tyyppi | Esimerkki |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Koodi  | var-osoite = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Päätepisteportti 

Hakee päätepisteportti esiintymän.

| Tyyppi  | Esimerkki |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Koodi  | var-portin = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Esimerkki

Esimerkki Työntekijä-roolin, joka luo käynnistys tehtävän nimeltä ympäristömuuttuja `TestIsEmulated` asettaminen [ @emulated xpath-arvon](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) -tiedosto.

Luo [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paketti.

Ota käyttöön-roolin [etätyöpöydän kautta](cloud-services-role-enable-remote-desktop.md) .

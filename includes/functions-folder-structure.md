
Koodi kaikki toiminnot määritetty toiminto-sovelluksessa sijaitsevat pääkansion, joka sisältää isännän kokoonpano ja vähintään yhden alikansiot, jokainen sisältää erillisen-funktio, kuten seuraavassa esimerkissä koodi

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

*Host.json* -tiedosto sisältää joitakin runtime konfigurointi ja on avattujen funktion sovelluksen pääkansioon. Lisätietoja käytettävissä olevista asetuksista on artikkelissa [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) WebJobs.Script säilöön wikisivun.

Kukin funktio on kansio, joka sisältää vähintään yhden kooditiedostoja, function.json-määritys ja muut riippuvuudet.
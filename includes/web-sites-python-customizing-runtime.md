Azure määräytyy sen virtual ympäristössä, jossa on seuraava prioriteetti käytettävät Python version:

1. määritetty pääkansion runtime.txt versio
1. määritetty Python määrittämällä web app-kokoonpanossa versio ( **asetukset** > web-sovelluksen Azure-portaalissa**Sovelluksen asetukset** -sivu)
1. Python 2.7 on oletusarvo, jos mikään edellä mainituista ei määritetty

Sisällön kelvollisia arvoja 

    \runtime.txt

ovat seuraavat:

- Python 2.7
- Python 3.4

Jos micro versio (kolmannen numeron) on määritetty, se ohitetaan.

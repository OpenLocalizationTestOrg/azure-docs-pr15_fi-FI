<properties 
    pageTitle="Media-palveluiden PlayReady käyttöoikeuden mallin yleiskatsaus" 
    description="Tässä artikkelissa on yleiskatsaus PlayReady käyttöoikeuden malli, joka käyttää PlayReady käyttöoikeuksien määrittämiseen." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="media-services-playready-license-template-overview"></a>Media-palveluiden PlayReady käyttöoikeuden mallin yleiskatsaus

Azure Media Services on nyt palvelu välittää Microsoft PlayReady käyttöoikeudet. Kun peruskäyttäjän toisto-ohjelman (esimerkiksi Silverlight) yrittää PlayReady suojatun sisällön toistaminen, pyyntö lähetetään käyttöoikeuden toimitus-palvelun hankkia käyttöoikeus. Jos käyttöoikeus-palvelun hyväksyy pyynnön, joka lähetetään asiakkaan ja sitä voidaan käyttää salaus ja toistaa määritetyn sisällön käyttöoikeuden ongelmien vianmääritys.

Media-palveluiden sisältää myös API, joiden avulla voit määrittää PlayReady käyttöoikeudet. Käyttöoikeuksien sisältävät oikeudet ja rajoitukset, jota haluat käyttää PlayReady DRM runtime voidaan valvoa, kun käyttäjä yrittää toisto suojatun sisällön.
Alla on joitakin esimerkkejä PlayReady käyttörajoituksia, jotka voit määrittää:

- DateTime-arvo, jonka käyttöoikeutta on voimassa.
- DateTime-arvon, kun käyttöoikeus päättyy. 
- Pysyvän säilön tallentaa asiakkaan käyttöoikeuden. Jatkuva käyttöoikeuksia käytetään yleensä sallimaan offline-tilassa toisto sisällön.
- Pienin suojaustaso, pelaajan on oltava toistaa sisältöä. 
- Audio\video sisällön tulosteen ohjausobjektit tulosteen suojaustasoa. 
- Lisätietoja on kohdassa tulosteen ohjausobjektit (3.5) [PlayReady yhteensopivuuden säännöt](https://www.microsoft.com/playready/licensing/compliance/) asiakirjassa.

>[AZURE.NOTE]Voit tällä hetkellä vain määrittää PlayRight PlayReady käyttöoikeuden (nämä oikeudet tarvitaan). PlayRight ansiosta asiakkaan toistamisen sisältöä. Määrittäminen toisto tiettyjä rajoituksia PlayRight avulla. Lisätietoja on artikkelissa [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).

Määrittäminen PlayReady käyttöoikeuksien Media-palveluiden avulla, sinun on määritettävä Media Services PlayReady käyttöoikeus-malli. Malli on määritetty XML.

Seuraavassa esimerkissä on helpoin (ja yleisimmät) malli, joka määrittää basic streaming käyttöoikeuden. Tällä käyttöoikeudella asiakkaiden voisi toisto oman PlayReady suojatun sisällön.
    
    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

XML mukainen PlayReady käyttöoikeuden mallin XML-rakenteen PlayReady käyttöoikeuden mallissa XML-rakenne-osassa määritettyjä.

Media-palveluiden määrittää myös .NET-luokat, jotka voidaan muuntaa sarjaksi ja poistaa, ja XML. Tärkeimmät kuvaus-kohdassa [Media Services .NET luokat](media-services-playready-license-template-overview.md#classes). käyttämät voit määrittää käyttöoikeuden malleja.

Lopusta loppuun-esimerkki, joka käyttää .NET luokkien määrittäminen PlayReady käyttöoikeus-mallin, katso [käyttämällä PlayReady dynaaminen salaus ja käyttöoikeus toimituksen palvelu](https://msdn.microsoft.com/library/azure/dn783467.aspx).

##<a id="classes"></a>Media-palveluiden .NET-luokat, joiden avulla voit määrittää käyttöoikeuden mallit

Seuraavassa esitellään tärkeimmät .NET käytetään määrittäminen Media Services PlayReady käyttöoikeuden malleja. Näiden luokkien Yhdistä [PlayReady käyttöoikeuden mallin XML-rakenne](media-services-playready-license-template-overview.md#schema)määritellään käyttämiseen.

Muuntaa sarjaksi ja poistaa, ja Media-palveluiden käyttöoikeus mallista XML käytetään [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) -luokka.

###<a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate

[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - tähän luokkaan edustaa vastaus lähetetään takaisin peruskäyttäjän mallia. Se sisältää kentän mukautettuja tietoja merkkijonon välillä käyttöoikeuspalvelimen ja sovelluksen (voi olla hyötyä mukautetun sovelluksen logiikan) sekä luettelo vähintään yksi käyttöoikeus-malleista.

Tämä on mallin hierarkian "ylimmän tason"-luokassa. Mikä tarkoittaa, että vastaus malli sisältää käyttöoikeuden malliluettelon sisältävät käyttöoikeussopimuksen-mallit (suoraan tai epäsuorasti) kaikki luokat, jotka muodostavat mallitiedot avulla voi muuntaa sarjaksi.


###<a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate

[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - luokan edustaa käyttöoikeuden mallin luominen PlayReady käyttöoikeudet käyttäjille palautetaan. Se sisältää sisällön avaimen käyttöoikeus ja oikeuksien tai rajoituksia, PlayReady DRM runtime toimialuerekisteröintipalvelun, kun sisällön avaimen avulla.


###<a id="PlayReadyPlayRight"></a>PlayReadyPlayRight

[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - tähän luokkaan edustaa PlayReady käyttöoikeuden PlayRight. Toisto määritetty käyttöoikeus ja PlayRight itse (ja toisto tietyn käytännön) nolla tai rajoitukset sisältöä voi myöntää käyttäjälle. Paljon PlayRight käytäntö on tehdään tulostus-rajoituksia, jotka ohjata tulostaa sisältöä voidaan toistaa päälle ja rajoitukset, jotka on sijoitettava paikka, annetun tulosteen käytettäessä. Esimerkiksi jos DigitalVideoOnlyContentRestriction on käytössä, sitten DRM runtime sallii vain videon näytetään digitaalisen tulostus (analogisen videon tulostus ei voi siirtää sisällön) kautta.

>[AZURE.IMPORTANT]Seuraavanlaisiin rajoituksia voi olla erittäin tehokas, mutta se voi vaikuttaa myös kuluttaja-toiminto. Jos tulos suojaukset on määritetty liikaa, sisällön voi olla unplayable Joissakin palveluissa. Katso lisätietoja artikkelista [PlayReady yhteensopivuuden säännöt](https://www.microsoft.com/playready/licensing/compliance/) asiakirjan.

Katso, mitä suojaus tasoilla Silverlight tukee esimerkiksi: [Silverlight tuki tulosteen suojaukset](http://go.microsoft.com/fwlink/?LinkId=617318).

##<a id="schema"></a>PlayReady käyttöoikeuden mallin XML-rakenne
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

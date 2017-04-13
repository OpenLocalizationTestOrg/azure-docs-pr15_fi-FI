####<a name="configuring-the-ios-project-in-xamarin-studio"></a>IOS-projektin määrittäminen Xamarin Studiossa

1. Avaa **Info.plist**Xamarin.Studio, ja Päivitä **Pikaoppaista tunnus** Pikaoppaista-tunnus, jonka loit aiemmin käyttämällä uusi sovellus.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Vieritä **Taustan tilat** ja valitse **Tausta-tila käyttöön** - ja **Etä ilmoitukset** -ruutuun. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Kaksoisnapsauta projektin ratkaisu-toiminnolla voit avata **Projektin asetukset**.

4.  Valitse **Muodosta** **iOS Pikaoppaista allekirjoitus** , ja valitse vastaavan **käyttäjätiedot** ja olet määrittänyt vain projektin **Provisioning profiilin** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Näin varmistat, että projektin käyttää uutta profiilia koodin allekirjoittamiseen. Virallinen Xamarin laitteen valmistelu asiakirjat-kohdassa [Xamarin laitteen käyttöönotto].

####<a name="configuring-the-ios-project-in-visual-studio"></a>IOS-projektin määrittäminen Visual Studiossa

1. Visual Studion projektin hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**.

2. Ominaisuudet-sivujen **sovellus iOS** -välilehti ja Päivitä **tunnus** aiemmin luomasi tunnus.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Valitse **iOS Pikaoppaista allekirjoitus** -välilehden vastaavan **käyttäjätiedot** ja olet määrittänyt vain projektin **Provisioning profiilin** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Näin varmistat, että projektin käyttää uutta profiilia koodin allekirjoittamiseen. Virallinen Xamarin laitteen valmistelu asiakirjat-kohdassa [Xamarin laitteen käyttöönotto].

4. Kaksoisnapsauta Info.plist, voit avata sen ja ottaa **RemoteNotifications** tausta tilat-kohdassa. 



[Xamarin laitteen valmistelu]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
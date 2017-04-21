<properties
   pageTitle="Sivun otsikon, joka näyttää selaimessa-välilehti ja etsinnän tulokset"
   description="Artikkelin kuvaus, joka näytetään purkamisen sivuja ja useimmat hakutulokset"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Korotuksia-malli (otsikko 1 tai H1 artikkeli): otsikko on artikkelissa yläreunassa

Kopioi korotuksia tätä mallia, kopioi on artikkelissa oman paikallisen repo tai napsauta GitHub-Käyttöliittymän raaka-painike ja kopioida korotuksia.

  ![Vaihtoehtoisen tekstin; ei jättää tyhjäksi. Kuvaile kuva.][8]

Johdanto kappaleen: sekä dolor amet adipiscing elit. Phasellus interdum nulla risus lacinia porta nisl imperdiet sed. Mauris dolor mauris tempus sed lacinia nec euismod ei felis. Nunc semper porta ultrices. Maecenas neque nulla condimentum akateeminen ipsum istut amet, dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Otsikko 2 (H2)

Aenean istut amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus rhoncus-faucibus sed urna faucibus.  volutpat mi tunnus purus ultrices iaculis nec ei neque. [Linkkiteksti linkin azure.microsoft.com ulkopuolella](http://weblogs.asp.net/scottgu). Nullam dictum dolor aliquam pharetra-palvelussa. Vivamus ac hendrerit mauris [linkki esimerkkiteksti linkki artikkeliin service-kansiossa](../articles/expressroute/expressroute-bandwidth-upgrade.md).

10 kertaa enemmän liikennettä noutaminen [Google]  [ gog] kuin [Yahoo] - [ yah] - tai [MSN] [msn].

> [AZURE.NOTE] Sisennetty viitteen teksti.  Sanan "Huomautus" lisätään julkaisun aikana. Ut EU: n pretium lacus. Nullam purus est, iaculis sed est taso, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi EU: n condimentum turpis nisi purus.

1. Aenean istut amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus faucius- **Rhoncus** -faucibus sed urna. Suspendisse volutpat mi tunnus purus ultrices iaculis nec ei neque.

    ![Vaihtoehtoisen tekstin; ei jättää tyhjäksi. Kerääminen auton kilpailu punainen.][5]

3. Nullam dictum dolor aliquam pharetra-palvelussa. Vivamus ac hendrerit mauris. Sed dolor dui condimentum et varius a vehicula osoitteessa nisl.

    ![Vaihtoehtoisen tekstin; Älä jätä tyhjä][6]


Suspendisse volutpat mi tunnus purus ultrices iaculis nec ei neque. Nullam dictum dolor aliquam pharetra-palvelussa. Vivamus ac hendrerit mauris. Otrus informatus: [linkin toisen azure.microsoft.com dokumentaatio aiheen 1](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Otsikko (H2)

Ut EU: n pretium lacus. Nullam purus est, iaculis sed est taso, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi EU: n condimentum turpis nisi purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum ennakko ipsum primis-faucibus orci luctus et ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam ei <i>nisl molestie</i> pharetra eget est. [Toisen azure.microsoft.com dokumentaatio aiheen linkki 2](web-sites-custom-domain-name.md)


Quisque commodo eros taso lectus euismod auctor eget istut amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Otsikko 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + SEM

2. Valitse massa EU: n tellus tempus hendrerit Nullam.

    ![Vaihtoehtoisen tekstin; ei jättää tyhjäksi. Esimerkki kanavan 9 videon.][7]

3. Quisque felis enim fermentum ut aliquam nec pellentesque pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Vestibul ennakko ipsum primis-faucibus orci luctus et ultrices posuere cubilia Curae; Nullam ultricies ipsum akateeminen volutpat hendrerit purus diam pretium eros, akateeminen tincidunt nulla sekä sed turpis: [linkin 3 toisen azure.microsoft.com dokumentaatio aiheen](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    

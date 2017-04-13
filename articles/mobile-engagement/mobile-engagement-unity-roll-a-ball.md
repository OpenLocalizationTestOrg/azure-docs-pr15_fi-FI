<properties
    pageTitle="Yhtenäisyyden kuvat pallo-opetusohjelma"
    description="Perinteinen yhtenäisyyden luominen aikaisempi pallo-Ottelu on vanhat tarvittavat kaikki Mobile välitys yhtenäisyyden hakeminen"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Luo yhtenäisyyden aikaisempi tali Ottelu

Tässä opetusohjelmassa käydään läpi päävaiheet toimintoja on muutettu [yhtenäisyyden kuvat pallo opetusohjelma](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Tämä esimerkki Ottelu koostuu käytettävä player-objekti, joka on sovelluksen käyttäjä voi hallita ja Ottelu tavoitteena on ' kerättävät ' collectible objektien riski törmätä collectible objektit Playerin objektia. Tämä olettaa yhtenäisyyden editorissa ympäristön basic tuntemusta. Jos kohtaat ongelmia, valitse pitäisi viitata koko opetusohjelman. 

### <a name="setting-up-the-game"></a>Ottelu määrittäminen
Seuraavia ohjeita ovat [yhtenäisyyden opetusohjelma](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Avaa **Yhtenäisyyden-editori** ja valitse **Uusi**. 
    
    ![][51] 
    
2. **Projektin nimen** & **sijainti**- **3D** ja valitse **Luo projekti**.
    
    ![][52]

3. Tallenna juuri luonut uuden projektin osana nimen **MiniGame** kanssa sisällä uusi oletusarvo-näkymän ** \_näkymien** **kalusto** -kansion alikansio:
    
    ![][53]

4. Luo- **3D-objekti -> tasossa** toistaminen kentäksi ja anna **maahan** tasossa tätä objektia

    ![][1]

5. Palauta **maasta** objektin muunto-osan, niin, että se on alkuperän. 

    ![][3]

6. Poista **Näytä ruudukko** **Gizmos** valikosta **maasta** objektin.

    ![][4]

7. Päivitä **maasta** objektin **Skaalaus** -osan [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Lisää projektiin uusi- **3D-objekti -> pallo** ja nimetä **Playerin**pallo tätä objektia. 

    ![][6]

9. Valitse **Player** -objekti ja valitse **Palauta Transform** samalla tasolla objekti. 

10. Päivitä **muunto -> sijainti -> Y-koordinaatin** osan lukuna 0,5 Player-y.  

    ![][7]

11. Luo uusi kansio nimeltä **materiaalien** projektin missä materiaalin väri Playerin luodaan. 

12. Luo kutsutaan **taustan** tähän kansioon uusia **materiaali** . 

    ![][8]

13. Päivittää materiaalista värin päivittämällä sen **Albedo** -ominaisuus. Voit valita [0,32,64] RGB-arvot. 

    ![][9]

14. Vedä tämä materiaali näkymän näkymän värin lisääminen **maasta** objekti. 

    ![][10]

17. Lopuksi Päivitä **muunto -> kierto Y ->** 60 selkeyttä suunta Light-objektissa. 

    ![][12]

### <a name="moving-the-player"></a>Toisto-ohjelman siirtäminen
Seuraavia ohjeita ovat [yhtenäisyyden opetusohjelma](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. **RigidBody** osan lisääminen **Player** -objekti. 

    ![][13]

2. Luo uusi kansio nimeltä **komentosarjojen** projektissa. 

3. Valitse **Lisää osa -> uusi komentosarja Script C# ->**. Anna sille nimi **PlayerController**ja valitse **Luo ja lisää**. Tämä luo ja liitä komentosarja Playerin objekti.  

    ![][14]

5. Siirrä tämä komentosarja-projektin **komentosarjat** -kansioon. 

6. Avata komentosarjan muokattavaksi tuttuja komentosarjaeditori, Päivitä komentosarjoja seuraava koodi ja tallenna se. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Huomaa, että edellä komentosarja **nopeus** -ominaisuutta. 10 nopeus-ominaisuuden päivittäminen yhtenäisyyden-editorissa.  

    ![][15]

9. Osumien **toistaa** yhtenäisyyden-editorin. Nyt pitäisi onnistua ohjaaminen näppäimistöllä tali ja olisi Kierrä ja siirtyminen. 

### <a name="moving-the-camera"></a>Kameran siirtäminen
Seuraavia ohjeita ovat [yhtenäisyyden-opetusohjelma](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) ja sitominen **Main kameran** **Player** -objekti. 

1. Päivitä **Transform.Position** on X = 0, Y = 10.5, Z =-10.  
2. Päivitä **Transform.Rotation** on X = 45, Y = 0, Z = 0.  

    ![][16]

2. Lisää uusi komentosarja kutsutaan **CameraController** **MainCamera** , ja siirrä se komentosarjat-kansiossa. 

    ![][17]

3. Avata komentosarjan muokattavaksi ja lisää seuraava koodi:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Yhtenäisyyden ympäristössä - Vedä Playerin muuttujan pelaajan tila Main kameran objektin niin, että kahden liittyvät toisiinsa. 

    ![][18]

6. Nyt Jos osumien toista yhtenäisyyden-editorin ja Playerin pallo-objektin kiertäminen sitten näet seuraavan siirto-kamera.  

### <a name="setting-up-the-play-area"></a>Toista-alueen määrittäminen
Seuraavia ohjeita ovat [yhtenäisyyden opetusohjelma](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Luodaan ympäröivän maasta niin, että Playerin pallo objektia ei pudota käytöstä sen liikuttamista play-alueelle. 

1. Valitse **luominen -> Luo tyhjä Ottelu objekti ->** ja anna sille nimi **seinien**

    ![][19]

2. Valitse seinät - objektin luominen uusia- **3D-objekti -> kuution** ja anna sille nimi "Länsi seinän". 

    ![][20]

3. Päivitä **muunto -> sijainti** ja **Muunna -> mittakaava** Länsi seinän objektin. 

    ![][21]

4. Kaksinkertainen Länsi seinän luominen **Itä seinän** päivitetyt muunnoksen sijainti ja asteikko. 

    ![][22]

5. Kaksinkertainen Itä seinän luomalla **Pohjois seinän** päivitetyt muunnoksen sijainti ja asteikko. 

    ![][23]

6. Pohjois-seinän kaksoiskappaleet ja luo **Etelä seinän** päivitetyt muunnoksen sijainti ja asteikko. 

    ![][24]

### <a name="creating-collectible-objects"></a>Collectible objektien luominen
Seuraavia ohjeita ovat [yhtenäisyyden opetusohjelma](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Jotkin houkuttelevien näköisen objekteja, jotka muodostavat collectible objekteja, jotka Playerin tali-objekti on kerää mukaan riski törmätä heidän kanssaan joukko luodaan. 

1. Luo uusi **3D-kuutio-objekti** ja anna sille nimi nouto. 

2. Säädä- **muunnos -> kierron** & noudon objektin-**muunnos -> asteikko** . 

    ![][25]

3. Luo ja liitä **Uuden C# komentosarjan** kutsutaan **kierron** noudon objekti. Varmista, että Siirrä komentosarja-komentosarjat-kansiossa. 

    ![][26]

4. Avaa tämän komentosarjan muokattavaksi ja päivität sen ovat seuraavat: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Nyt osumien toistotilasta yhtenäisyyden-editorin ja noudon objektin Näytä kiertäminen sen akselilla.

6. Luo uusi kansio nimeltä **Prefabs** 

    ![][27]

7. Vedä **nouto** objekti ja aseta se Prefabs-kansiossa.

    ![][28]

8. Luo kutsutaan **Pickups**uusi **Tyhjä Ottelu objekti** . Palauta sijaintinsa origin ja vedä sitten noudon objektin Ottelu objektiin.  

    ![][29]

9. **Nouto** objektin monistaminen ja kerro **maasta** objektin **Player** -objektin ympärille päivittämällä **Transform.Position's X- ja Z** -arvot asianmukaisesti. 

    ![][30]

10. Luoda **uutta aineistoa** kutsutaan **nouto** ja update punainen määrittää värin päivittämällä **Albedo ominaisuuden** samalla on ollut päivityksessä maasta objekti. 

    ![][31]

11. Käytä kaikissa 4 noudon objekteissa materiaalista.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Keräätkö noudon objektit
Seuraavia ohjeita ovat [yhtenäisyyden opetusohjelma](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Päivitämme Playerin, jotta se pystyy 'kerää' noudon objektien mukaan riski törmätä heidän kanssaan. 

1. Avaa **PlayerController** komentosarjan muokattavaksi Player-objekti liitetään ja päivittää sen seuraavasti:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Luo uusi **tunniste** **Valitse ylös** (se on vastattava komentosarja ominaisuudet)  

    ![][33]
    
    ![][34]

3. Lisää tämän **tunnisteen** Prefab nouto objekti. 

    ![][35]

4. Ota käyttöön Prefab objektin **IsTrigger** -valintaruutu.

    ![][36]

5. Lisää kiinteä leipätekstin nouto Prefab objekti. Suorituskyvyn optimointi päivitämme staattinen collider, joka on käytetty dynaaminen collider. 

    ![][37]
  
6. Valitse lopuksi prefab objektin **IsKinematic** -ominaisuutta. 

    ![][38]

7. Osumien **toistaminen** yhtenäisyyden-editorin ja saa oikeuden toista tämä **aikaisempi pallo** Ottelu siirtämällä näppäimistön näppäimellä suunta syötteen Player-objekti. 

### <a name="updating-the-game-for-mobile-play"></a>For mobile toista Ottelu päivittäminen
Edellä kuvattujen tehdyistä yhtenäisyyden basic opetusohjelman. On nyt Muokkaa Ottelu, minkä ansiosta mobiililaitteella helpossa muodossa. Huomaa, että on käytetty tähän mennessä testikäyttöön Ottelu näppäimistöön. Nyt on muokkaa sitä niin, että olemme hallita Playerin käyttämällä puhelimen liikerata eli Voit käyttää kiihtyvyysmittarin syötteenä. 

Avaa **PlayerController** -komentosarjan muokattavaksi ja päivittää liikerata kiihtyvyysmittarin-avulla voit siirtää Player-objektin **FixedUpdate** -menetelmää. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Tässä opetusohjelmassa tekee yhtenäisyyden Ottelu basic luominen ja voit ottaa valittua voit toistaa Ottelu laitteessa. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    

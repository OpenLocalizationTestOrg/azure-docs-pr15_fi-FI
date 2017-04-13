## <a name="send-messages-to-event-hubs"></a>Tapahtuman porttiin lähettämiseen

Tässä osassa on kirjoittaa C-sovelluksen tapahtumien lähettäminen tapahtumaa-toiminnossa. Käytämme [Apache Qpid projektin](http://qpid.apache.org/)Proton AMQP-kirjasto. Tämä on paperimuotoon käyttäminen palvelun Bus olevien ja aiheet AMQP c kuin näkyy [Tässä](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Lisätietoja on [Qpid Proton](http://qpid.apache.org/proton/index.html)ohjeissa.

1. [Qpid AMQP Messenger sivun](http://qpid.apache.org/components/messenger/index.html) **Asentaminen Qpid Proton** -linkkiä ja noudata ohjeita käyttöympäristön mukaan. Olemme olettaa Linux-ympäristöön; esimerkiksi [Azure Linux AM](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04 kanssa.

2. Kääntää Proton kirjaston, asenna seuraavat paketit:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Lataa [Qpid Proton kirjastossa](http://qpid.apache.org/proton/index.html) kirjasto ja Pura se, kuten:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Luo kansio, käännä ja asenna luominen:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. Työn hakemistoa Luo uusi tiedosto nimeltä **sender.c** seuraavat sisältöä. Muista korvattavat tapahtumaa-toiminnossa nimesi ja nimitilan nimi-arvo (tämä on yleensä `{event hub name}-ns`). URL-koodatun version avain myös korvaa aiemmin luotu **SendRule** varten. Voit myös URL-Osoitteen koodata se [tähän](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Käännä tiedosto, mikäli **gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] Tämä koodi Käytämme lähtevän ikkunan 1 pakottaa viestit, mahdollisimman pian. Yleensä sovelluksen yrittää erä viestejä niin, että siirtonopeuden. Saat lisätietoja tämän ja muiden ympäristöjen ja ympäristöjen, jonka sidontojen toimitetaan Qpid Proton kirjaston käyttämisestä [Qpid AMQP Messenger-sivu](http://qpid.apache.org/components/messenger/index.html) (tällä hetkellä Perl, PHP, Python ja Ruby).

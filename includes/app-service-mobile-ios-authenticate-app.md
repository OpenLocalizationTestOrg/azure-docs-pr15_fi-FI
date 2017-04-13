**Tavoite-C**: 

1. Mac-tietokoneeseen Avaa _QSTodoListViewController.m_ Xcode ja lisää seuraavalla tavalla. Muuttaa _google_ _microsoftaccount käytettävissä_-, _twitter_-, _facebook_- ja _windowsazureactivedirectory_ , jos et käytä Google kuin tunnistetietojen toimittaja. Jos käytät Facebook- [sovelluksen whitelist Facebook toimialueiden tarvitset](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Korvaa `[self refresh]` - `viewDidLoad` - _QSTodoListViewController.m_ kanssa seuraavasti:

            [self loginAndGetData];

3. Paina Käynnistä sovellus ja kirjaudu sitten _Suorita_ . Kun olet kirjautunut sisään, sinun pitäisi Todo luettelon tarkasteleminen ja tee päivitykset.

**Swift**:

1. Mac-tietokoneeseen Avaa _ToDoTableViewController.swift_ Xcode ja lisää seuraavalla tavalla. Muuttaa _google_ _microsoftaccount käytettävissä_-, _twitter_-, _facebook_- ja _windowsazureactivedirectory_ , jos et käytä Google kuin tunnistetietojen toimittaja. Jos käytät Facebook, [sinun on whitelist Facebook-toimialueet-sovelluksen](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Poista rivit `self.refreshControl?.beginRefreshing()` ja `self.onRefresh(self.refreshControl)` lopussa `viewDidLoad()` - _ToDoTableViewController.swift_. Puhelun `loginAndGetData()` niiden paikassa:

            loginAndGetData()

3. Paina Käynnistä sovellus ja kirjaudu sitten _Suorita_ . Kun olet kirjautunut sisään, sinun pitäisi Todo luettelon tarkasteleminen ja tee päivitykset.

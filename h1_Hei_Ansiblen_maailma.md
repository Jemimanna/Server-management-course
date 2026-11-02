# h1 Hei Ansiblen maailma
## x) Tiivistelmät
### Karvinen 2026: SSH public key - Login without password
* Public key (julkinen avain) avulla kirjautumista käytetään, koska se on nopeampaa kuin kirjoittaa salasana aina uudestaan, se voi myös tehdä serveristä turvallisemman.
* OpenSSH lataus:
  * `sudo apt-get update`
  * `sudo apt-get -y install ssh`
    * sudo = komento suoritetaan järjestelmänvalvojan oikeuksilla
    * apt-get = paketinhallintatyökalu
    * update = päivittää pakettiluettelot -> tarkistaa mitkä paketit tarvitsevat päivityksiä
    * -y = vastaa automaattisesti "yes" kaikkiin kysymyksiin
    * install ssh = lataa SSH:n
  * `sudo systemctl enable --now ssh`
    * systemctl = System Control -> hallitaan palveluita
    * enable = SSH käynnistyy boottauksessa
    * --now = SSH käynnistyy heti
  * `ssh localhost`
    * ottaa SSH yhteyden localhostiin
    * w -komennolla näkee ketkä ovat kirjautuneet
    * exit -> SSH yhteys katkaistaan
* Automaattinen kirjautuminen SSH julkisella avaimella:
  * `ssh-keygen`
    * luo avainparin
  * `ssh-copy-id localhost`
    * lisää julkisen avaimen .ssh/authorized_keys kansioon etäkoneelle
  * Nyt kirjautuminen on automaattinen SSH yhteydellä eli ei tarvitse erikseen kirjoittaa salasanaa
### Omat huomiot
* SSH:n käyttö nopeuttaa huomattavasti serverin hallintaa, varsinkin jos on monta eri käyttäjää.
### Karvinen 2026: Hello Ansible
* Ansible on konfiguraatiohallintatyökalu, joka toimii SSH:n kautta -> kirjoitetaan infrastruktuuri koodina -> käyttäjä kuvailee lopputilan ja ansible tekee muutokset, vain jos niitä tarvitaan.
* Ansible lataus:
  * `sudo apt-get update`
  * `sudo apt-get install ansible micro bash-completion tree`
    * micro = tekstieditori
    * bash-completion = tab täydentää kirjoitettavan sanan
    * tree = näyttää puumaisen rakennelman tiedostoista ja kansioista
    * `micro bash-completion tree` ovat "extra" asioita, jotka helpottavat ansiblen käyttöä
* Testaus:
  * Tehtiin kansio ansible tiedostoille ja luotiin hosts.ini listaus käyttäjistä, joita kontrolloidaan -> sinne lisättiin localhost.
  * `ansible all -a 'uptime' -i hosts.ini`
    * komento kertoo kauanko kaikki "hosts" koneet ovat olleet päällä
* Site.yml laitettiin listoja eri rooleista, joita koneille annettiin.
* Roolit piti luoda erikseen ja niille tasks/main.yml
  * main.yml -> sisältää koodin, joka suoritetaan automaattisesti
### Omat huomiot
* Ansiblen käytto on aika tarkkaa ja esim. roolien merkkaus pitää tehdä tarkasti, jotta se toimii.

## a) Debian 13 lataus
* https://github.com/Jemimanna/Server-management-course/blob/main/h0_oikeasti_netiss%C3%A4.md

## b) SSH-demoni virtuaalikoneelle
Tämän tehtävän suoritukseen on käytetty ohjeena Karvisen SSH public key - Login without password (2026) julkaisua.

Ajoin ensin komennon `sudo apt-get update`, joka päivittää pakettiluettelot ja tarkistaa mitkä paketit tarvitsevat päivityksiä.

Tämän jälkeen ajoin komennon `sudo apt-get -y install ssh`, joka latasi SSH:n koneelle ja -y vastasi automaattisesti "yes" kaikkiin kysymyksiin.

Sitten ajoin komennon `sudo systemctl enable --now ssh`, joka käynnistää SSH:n aina boottauksen yhteydessä (enable) ja komento --now käynnisti sen myös heti.

Tämän jälkeen testasin yhteydenottoa SSH:lla. Komennolla `ssh jemima@localhost`, otin yhteyden localhost koneeseen.

<img width="472" height="119" alt="image" src="https://github.com/user-attachments/assets/fdf2f462-02f0-4684-b2dc-f7deb778bc86" />

Komennolla `w` näin, että olin kirjautunut kaksi kertaa.

<img width="470" height="59" alt="image" src="https://github.com/user-attachments/assets/b942b35a-2f68-44d2-99bc-0dbbf995daac" />

`exit` komennolla SSH-yhteys voitiin katkaista.

<img width="167" height="46" alt="image" src="https://github.com/user-attachments/assets/bc43de79-7552-4a88-8986-07d4499714f2" />

Näin pääsin siis ottamaan SSH yhteyden localhost koneeseen "jemima" käyttäjänä.

## c) Pubkey - Automatisoidaan SSH kirjautuminen julkisella avaimella
Tämän tehtävän suoritukseen on käytetty ohjeena Karvisen SSH public key - Login without password (2026) julkaisua.

Ensiksi loin uuden avainparin komennolla `ssh-keygen` ja kopioin siitä luodun julkisen avaimen tiedot komennolla `ssh-copy-id jemima@localhost`. Tämä kopioi julkisen avaimen tiedot .ssh/authorized_keys kansioon "jemima" käyttäjälle. Tämä tehtiin sen takia, että avaimen kopiointi mahdollistaa automaattisen SSH kirjautumisen.

Koska julkinen avain on kopioitu käyttäjän SSH kansioon, se osaa tunnistaa sitä vastaavalla yksityisellä avaimella tehdyt kirjautumiset ja täten kirjautuminen on "automaattinen" ilman tarvetta kirjoittaa salasanaa.

Nyt, kun kirjauduin, salasanaa ei kysytty, vaan pääsin suoraan ottamaan SSH-yhteyden.

<img width="484" height="110" alt="image" src="https://github.com/user-attachments/assets/33a249eb-3688-4875-8659-900ed794fd03" />

## d) Hei Maailma! Ansiblella
Tämän tehtävän suoritukseen on käytetty ohjeena Karvisen Hello Ansible (2026) julkaisua.
### Ansible lataus
Ensiksi oli Ansiblen lataus. Tein sen ajamalla ensin `sudo apt-get update` komennon, joka päivittää pakettiluettelot ja tarkistaa mitkä paketit tarvitsevat päivityksiä.

<img width="375" height="92" alt="image" src="https://github.com/user-attachments/assets/63ab19cd-1a79-4e0b-854f-9af154396860" />

Tämän jälkeen latasin Ansiblen komennolla `sudo apt-get install ansible micro bash-completion tree`. 
    * micro = tekstieditori
    * bash-completion = tab täydentää kirjoitettavan sanan
    * tree = näyttää puumaisen rakennelman tiedostoista ja kansioista
    * `micro bash-completion tree` ovat "extra" asioita, jotka helpottavat ansiblen käyttöä.
Latauksessa kesti hetki.

<img width="464" height="414" alt="image" src="https://github.com/user-attachments/assets/d963d7ce-9d20-4f5f-bf9f-3c0f1c2e8df6" />

### Testataan SSH Ansiblen avulla
Tein hakemiston kaikille Ansible konfiguraatioille menemällä ensin kotihakemistooni komennolla `cd` ja sen jälkeen ansible/ hakemiston luominen komennolla `cd ansible/`. Nyt kun hakemisto oli luotu menin vielä sinne komennolla `cd ansible/`.

<img width="170" height="45" alt="image" src="https://github.com/user-attachments/assets/d3094dd2-04f2-4afa-b468-1e90d644ff58" />

Seuraavaksi loin tiedoston hosts.ini, johon lisättäisiin lista "hosts" joita voidaan kontrolloida. Tein tämän komennolla `micro hosts.ini`. Micro on tekstieditori ja se avasi hosts.ini tiedoston, jonne kirjoitin localhost. Tämä lisäsi localhostin listaan.

<img width="327" height="71" alt="image" src="https://github.com/user-attachments/assets/c75c055f-5793-4979-8b19-2d06c508016e" />

Testasin vielä, että localhost varmasti tallentui hosts.ini tiedoston sisälle komennolla `cat hosts.ini`. Cat-komento näyttää tiedoston sisällön.

<img width="215" height="47" alt="image" src="https://github.com/user-attachments/assets/f0a52b82-5ace-4eaa-bbb8-d6c4f9319077" />

Seuraavaksi testasin ansiblen toimintaa laittamalla sen suorittamaan uptime komennon kaikille hosteille. Tein tämän komennolla `ansible all -a 'uptime' -i hosts.ini`. `all` viittaa kaikkiin koneisiin, joita löytyy hosts.ini tiedostosta ja `uptime` näyttää kauanko kukin kone on ollut päällä. 

Kuvasta näkyy localhostin uptime, sillä localhostin olin lisännyt hosts.ini kansioon.

<img width="951" height="61" alt="image" src="https://github.com/user-attachments/assets/3cd85ba3-d71b-4111-b4a6-b5778a303f50" />

### Python korjaus
Aiemmassa kuvassa näkyy, kuinka ansible valittaa python versiosta. Tämä korjataan laittamalla se käyttämään python3 hosts.ini tiedoston sisällä. Kuvassa [all:vars] all tarkoittaa kaikkia hosts.ini tiedostossa olevia koneita ja vars "variables" eli eri muuttujat.

<img width="226" height="75" alt="image" src="https://github.com/user-attachments/assets/8c79861c-6aba-4966-8158-60955cc9670c" />

Ajoin komennon `ansible all -a 'uptime' -i hosts.ini` uudestaan ja nyt python varoitus oli kadonnut.

<img width="317" height="44" alt="image" src="https://github.com/user-attachments/assets/54057bdb-73ed-4af2-88eb-f9363eb852ce" />

### Ansible.cfg
Nopeuttaakseni ansiblen käyttöä tein uuden tiedoston ansible.cfg jälleen komennolla `micro ansible.cfg` ja sinne lisäsin hosts.ini tiedoston, jotta ei tarvitsisi lisätä "hosts.ini" jokaiseen komentoon. Tämä nopeuttaa ansiblen käyttöä.

<img width="224" height="55" alt="image" src="https://github.com/user-attachments/assets/8cd52bfd-b7aa-4802-b108-19fc6c8dda6c" />

Testasin sen toimintaa komennolla `ansible all -a "uptime"`, joka eroaa aiemmin laittamastani komennosta, sillä -i -hosts.ini kohtaa ei tarvinnut merkata.

<img width="312" height="50" alt="image" src="https://github.com/user-attachments/assets/c73bb871-51e4-485c-8e9f-7a9eb4932cf1" />

### Site.yml - roolit
Seuraavaksi piti tehdä rooleja 

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
* 
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
    * 

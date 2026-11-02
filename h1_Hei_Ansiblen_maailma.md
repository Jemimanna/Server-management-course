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

### Site.yml
Seuraavaksi piti tehdä rooli "hello". Tein uuden tiedoston komennolla `micro site.yml` ja sinne kirjoitin seuraavat tiedot:

<img width="205" height="65" alt="image" src="https://github.com/user-attachments/assets/cff64b29-924e-487c-a5ae-9fe4cac5e4c0" />

Ajoin komennon `ansible-playbook site.yml`, jolla suoritetaan playbook tiedostoja. Komento löysi tiedoston site.yml, mutta antaa virheilmoituksen, koska roolia "hello" ei ole vielä luotu. 

<img width="467" height="109" alt="image" src="https://github.com/user-attachments/assets/e64a3ca7-a142-4a0d-92af-0c518cafc36f" />

### Hello roolin luonti
Aluksi loin uuden hakemiston komennolla `mkdir -p roles/hello/tasks/`. `mkdir` luo uuden hakemiston, `-p` on sen takia komennossa, että se luo tarvittaessa kaikki ylemmät hakemistot ja `roles/hello/tasks` on hakemiston polku. Tämä luo polun roles -> hello -> tasks.

<img width="269" height="26" alt="image" src="https://github.com/user-attachments/assets/0e94e536-52db-49c8-9e4c-d902c6314a85" />

Seuraavaksi loin vielä main.yml kohdan roles/hello/tasks polkuun, jossa koodi suoritetaan. Tein tämän komennolla `micro roles/hello/tasks/main.yml` ja lisäämällä sinne seuraavat tiedot.
* - copy: = kopioi
* dest: /tmp/hello-ansible = destination eli kohde
* content: "testaus\n" = sisältö

<img width="290" height="65" alt="image" src="https://github.com/user-attachments/assets/3b1c501b-3844-4218-87c4-6903cd9e43ee" />

Ajoin komennon `ansible-playbook site.yml` uudestaan, sillä nyt se kutsuu luomaani hakemistoa roles/hello ja siellä tasks/main.yml suoritetaan automaattisesti. Vastaukseksi sain, että yhtä kohtaa oli muutettu (changed).

<img width="574" height="137" alt="image" src="https://github.com/user-attachments/assets/123e51c4-a43d-4865-a340-4708e4357325" />

Testasin vielä toisella komennolla `ssh localhost 'cat /tmp/hello-ansible'` tuleeko tiedostoon laittamani teksti vielä esiin. Se antoi vastaukseksi "testaus" eli komento toimi oikein.

<img width="319" height="32" alt="image" src="https://github.com/user-attachments/assets/95b6c6c4-516e-4f46-9e8a-d5fb907f897d" />

### Ansible - näytä mitä teet
Tahdoin vielä tehdä `ansible-playbook site.yml` tulostamasta paremman niin, että se näyttää, mitä se tekee. Lisäsin komennolla `micro ansible.cfg` tiedostoon kohdan display_args_to_stdout = true, joka printtaa sen, mitä ansible tekee. 

<img width="218" height="64" alt="image" src="https://github.com/user-attachments/assets/a571ad28-311d-41aa-b96d-7c137231f7d5" />

Nyt kun ajoin komennon `ansible-playbook site.yml` se näytti "hello" kohdassa, mitä hello tekee/sisältää. Vertasin tätä vielä `cat roles/hello/tasks/main.yml` komennon tulosteeseen.

<img width="452" height="152" alt="image" src="https://github.com/user-attachments/assets/e582c3ba-6515-4223-933b-cda3d695e4b1" />

<img width="291" height="53" alt="image" src="https://github.com/user-attachments/assets/8c1935d6-fa98-4a11-b963-73b1aa566660" />

### Lopputulos
Ajoin komennon `tree -F`, joka näyttää puuhakemiston tiedostoista ja hakemistoista. Tämä hahmottaa, miten kaikki linkittyy toisiinsa.

<img width="170" height="122" alt="image" src="https://github.com/user-attachments/assets/19446bc0-be0f-4f3e-af67-fb4874199443" />

Ajoin komennon `head -1000 ansible.cfg hosts.ini site.yml roles/hello/tasks/main.yml`, joka näytti kaiken, mitä olin laittanut kuhunkin hakemistoon/tiedostoon.

<img width="473" height="215" alt="image" src="https://github.com/user-attachments/assets/75a98140-4937-4c9c-b8a0-78efcabf2702" />

Viimeiseksi vielä ajoin jälleen komennon `ansible-playbook site.yml`, joka näytti, että kaikki toimi oikein.

<img width="572" height="149" alt="image" src="https://github.com/user-attachments/assets/e2c8686c-4697-4301-b7be-2bbb62079b49" />


## Lähteet
*


# H2 Voileipä
## x) Tiivistelmät
### Karvinen 2026: Sudo without password
* `sudo adduser antero` lisää käyttäjän antero.
* `sudo groupadd sudoless` luo ryhmän nimeltä sudoless.
* `sudo adduser antero sudoless` lisää antero-käyttäjän sudoless-ryhmään.
* `sudo -i` avaa root käyttäjän. Toisen ikkunan pitäminen auki root oikeuksilla, kun muokkaa toisella sudoa, voi potentiaalisesti korjata sudon, jos sen rikkoo.
* `sudo visudo /etc/sudoers.d/sudoless` visudo ajaa editorin ja pääsee muokkaamaan /sudoers.d/sudoless -> `%sudoless ALL = (ALL) NOPASSWD: ALL` ei tarvitse käyttää salasanaa.
#### Omat huomiot
* Ilmeisesti jos lisää nyt uusia käyttäjiä sudoless ryhmään sama sääntö pätee heihin. 
### Munroe 2006: xkcd 149: Sandwich
* Oma huomio: sudo antaa pääkäyttäjän oikeudet.
### Karvinen 2026: Passwordless Sudo with Ansible
* `cat roles/antero/tasks/main.yml` lisätään tänne rooleihin eri atribuutteja.
* `become: true` -> lisätään site.yml -> ansible yrittää sudo oikeuksia.
* `ansible-playbook site.yml --ask-become-password` kysyy sudo salasanaa.
#### Omat huomiot
* Aika outoa, että pitää erikseen kysyä Ansiblea kysymään salasanaa.
### Ansible-doc
* `copy` kopioi tiedoston tai hakemiston johonkin sijaintiin.
  * `content` asettaa tiedoston sisällöksi suoraan määritetyn arvon.
  * `dest` remote absoluuttinen polku, johon tiedosto/hakemisto kopioidaan.
  * `src` paikallinen polku kopioitavalle tiedostolle.
  * `owner` käyttäjän nimi, joka omistaa tiedoston/hakemiston.
  * `group` ryhmän nimi, jolle tiedosto kuuluu.
  * `mode` määrittää käyttöoikeudet tiedostolle/hakemistolle.
  * esim.
  
    ```yaml
    - name: Copy file with owner and permissions
      ansible.builtin.copy:
        src: /srv/myfiles/foo.conf
        dest: /etc/foo.conf
        owner: foo
        group: foo
        mode: '0644'
    ```
* `apt` hallinnoi apt paketteja (Debian/Ubuntu).
  * `name` lista pakettien nimistä.
  * `state` ilmaisee paketin halutun tilan.
  * `update_cache` päivittää tiedot saatavilla olevista paketti päivityksistä.
  * esim.

    ```yaml
    - name: Update all packages to their latest version
      ansible.builtin.apt:
        name: "*"
        state: latest
    ```

* `file` asettaa attribuutteja tiedostoille, hakemistoille tai symbolisille linkeille tai niitä voi poistaa.
  * `path` polku tiedostoon, jota hallitaan.
  * `recurse` asettaa määritetyt tiedoston ominaisuudet hakemiston sisällä oleville tiedostoille rekursiivisesti.
  * `src` polku tiedostoon, johon symbolinen linkki osoittaa.
  * `state` jos `absent` tarkoittaa, että hakemisto poistetaan ja tiedostot ja symboliset linkit unlinkataan.
  * `owner` käyttäjän nimi, joka omistaa tiedostojärjestelmän kohteen.
  * `group` ryhmän nimi, joka omistaa tiedostojärjestelmän kohteen.
  * `mode` määrittää käyttöoikeudet, jotka tiedostojärjestelmän kohteella ovat.
  * esim.

    ```yaml
    - name: Create a symbolic link
      ansible.builtin.file:
        src: /file/to/link/to
        dest: /path/to/symlink
        owner: foo
        group: foo
        state: link
    ```
* `user` hallinnoi käyttäjiä ja niiden attribuutteja.
  * `name` nimi käyttäjälle, joka luodaan, poistetaan tai jota muokataan.
  * `create_home` luo kotihakemiston käyttäjälle, kun käyttäjä luodaan tai jos kotihakemistoa ei ole olemassa.
  * `comment` asettaa käyttäjälle kuvauksen.
  * `groups` lista lisäryhmistä, joiden jäsen käyttäjä on myös.
  * `shell` asettaa käyttäjän komentotulkin.
  * `state` pitääkö tilin olla olemassa vai ei.
  * `system` onko käyttäjä järjestelmän käyttöön tarkoitettu vai peruskäyttäjä.
  * esim.
    
    ```yaml
    - name: Create a user 'johnd' with a home directory
      ansible.builtin.user:
        name: johnd
        create_home: yes
      ```
* `authorized_key` lisää tai poistaa SSH valtuutettuja avaimia.
  * `user` etäpalvelimen käyttäjänimi, jonka authorized_keys tiedostoa muokataan.
  * `key` SSH julkinen avain.
  * esim.

    ```yaml
    - name: Set authorized key taken from a file
      ansible.posix.authorized_key:
        user: charlie
        state: present
        key: "{{ lookup('file', '/home/charlie/.ssh/id_rsa.pub') }}"
    ```
### Omat huomiot
* Ansible-doc auttaa huomattavasti löytämään tarvittavat attribuutit omaan käyttöön. Esimerkit havainnollistavat hyvin, miten attribuutit kuuluu asetella.

## a) Sudoless
Tämän harjoituksen suorittamiseen käytin apuna Karvisen 2026 Sudo without password artikkelia ja Karvisen 2026 SSH public key - Login without password artikkelia.

Loin ensiksi uuden käyttäjän ansible komennolla `sudo adduser ansible`.

<img width="262" height="130" alt="image" src="https://github.com/user-attachments/assets/b3f09825-0af5-4d2e-bdf7-8c3cd7e5d3d0" />

Seuraavaksi loin uuden ryhmän sudoless komennolla `sudo groupadd sudoless`.

<img width="204" height="22" alt="image" src="https://github.com/user-attachments/assets/71eaee52-75db-4b7f-9d0b-8766a5c6daa5" />

Sitten lisäsin ansible käyttäjän ryhmään sudoless komenolla `sudo adduser ansible sudoless`.

<img width="239" height="20" alt="image" src="https://github.com/user-attachments/assets/a884a8a9-8ea5-4d8f-8c14-559a5e407464" />

Seuraavaksi loin komenolla `sudo visudo /etc/sudoers.d/sudoless` tiedoston, johon laitoin säännöt kaikille, jotka kuuluvat sudoless ryhmään. Visudo ajaa tekstieditorin ja /etc/sudoers.d/sudoless on polku, johon luodaan tekstitiedosto.

<img width="266" height="19" alt="image" src="https://github.com/user-attachments/assets/7a83bc9c-3881-4c2a-96d5-19f7a97abc94" />

Sinne lisäsin `%sudoless ALL = (ALL) NOPASSWD: ALL`, jossa:
* `%sudoless` tarkoittaa ryhmää.
* `ALL` tarkoittaa kaikkia tietokoneita.
* `(ALL)` tarkoittaa, että käyttäjä saa suorittaa komentoja, minkä vaan käyttäjän oikeuksilla myös root.
* `NOPASSWD` tarkoittaa, ettei kysytä salasanaa.
* `ALL` tarkoittaa, että kaikki komennot sallitaan.

<img width="176" height="24" alt="image" src="https://github.com/user-attachments/assets/5ff86fee-e3b9-4d72-8264-35ee73ac3e40" />

Seuraavaksi testasin, ottaa ssh yhteyden komennolla `ssh ansible@localhost` ja siellä ajoin komennon `sudo -k`, joka unohtaa, jos olisin antanut sudo komentoja ansible käyttäjällä.

<img width="470" height="142" alt="image" src="https://github.com/user-attachments/assets/d3f71592-0daa-48dd-9fc8-03097ea54857" />

Nyt kun olin ansible käyttäjällä testasin, ettei ansible käyttäjältä kysytä salasanaa sudo komennon yhteydessä. Ajoin komennon `sudo echo "Minulta ei kysytty salasanaa!"`. Salasanaa ei kysytty, joten sudo without password onnistui.

<img width="310" height="32" alt="image" src="https://github.com/user-attachments/assets/61cdb4cc-44d0-4c0c-bb24-c5f13245a15c" />

SSH automaattinen kirjatumisen tein kopioimalla jo olemassa olevani julkisen avaimen tiedot ansible käyttäjälle. Tämän tein suorittamalla komennon `ssh-copy-id ansible@localhost`.

<img width="467" height="151" alt="image" src="https://github.com/user-attachments/assets/ea48b94d-2b63-45d2-8232-c654c6ce6322" />

Testasin, että SSH avain toimii ottamalla jälleen yhteyden ansible käyttäjään komennolla `ssh ansible@localhost`. Salasanaa ei kysytty, joten automaattinen kirjautuminen SSH:lla onnistui.

<img width="467" height="118" alt="image" src="https://github.com/user-attachments/assets/e816c56d-d1db-4a87-a051-8c61b1c1a627" />

## b) Ansiblella tehty SSH:lla kirjautuva tunnus.
Tämän harjoituksen suorittamiseen käytin apuna Karvisen 2026 Passwordless Sudo with Ansible artikkelia. 

Alkuun loin kokonaan uuden roolin, jolle sitten asettaisin asetukset. Tein tämän menemällä ansible hakemistooni `cd ansible` ja siellä komennolla `mkdir -p roles/jemsku/tasks`, jossa mkdir luo uuden hakemistopolun ja -p kaikki puuttuvat yläkansiot jos niitä tarvitaan. 

<img width="270" height="117" alt="image" src="https://github.com/user-attachments/assets/f3e35568-889a-41e1-b6da-3d78fd579a7d" />

Minun piti vielä lisätä main.yml, jonka tein komennolla `micro roles/jemsku/tasks/main.yml`, jossa micro on tekstieditori, jolla main.yml luotiin ja jolla sinne voi kirjoittaa.

<img width="302" height="126" alt="image" src="https://github.com/user-attachments/assets/f4df4527-9bb1-40d0-bf93-c721d011de13" />

Minun täytyi kopioida julkisen SSH avaimen tiedot main.yml, joten hain ne komennolla `cat ~/.ssh/id_ed25519.pub` ja kopioin ne. Seuraavaksi menin muokkaamaan main.yml tiedostoa jälleen micro komennolla. Lisäsin sinne seuraavat attribuutit:

<img width="550" height="157" alt="image" src="https://github.com/user-attachments/assets/0b256485-faca-4c06-a026-ab54cc29cef2" />

Tarkistin vielä `cat` komennon avulla, että ne tallentuivat sinne.

<img width="535" height="174" alt="image" src="https://github.com/user-attachments/assets/cf8e79fd-ec86-477b-addf-9545c4aac8ea" />


Seuraavaksi menin site.yml tiedostoon komennolla `micro site.yml` ja lisäsin sinne uuden roolin `jemsku` ja `become: true`, joka kysyy sudo oikeuksia.

<img width="89" height="56" alt="image" src="https://github.com/user-attachments/assets/b50d306b-38e4-4674-b9dc-faad462f7532" />

Tämän jälkeen ajoin komennon `ansible-playbook site.yml` ja se valitti, ettei ole sudo salasanaa.

<img width="469" height="139" alt="image" src="https://github.com/user-attachments/assets/623aec51-aa6d-4cf6-a037-fb44069bd152" />

Sitten korjatakseni tämän, ajoin komennon `ansible-playbook site.yml -K`, jossa -K kysyy sudo salasanaa. Site.yml ajettiin ja näkyi, että neljä muutosta on tapahtunut.

<img width="743" height="283" alt="image" src="https://github.com/user-attachments/assets/44fc7e27-088c-4fa6-9e20-16ed0b37dd06" />

Ajoin komennon vielä uudelleen tarkistaakseni, ettei kaikki tarvittavat muutokset oli nyt tehty. Sain vastaukseksi ok=6 ja kaikki muut nollaa, eli se oli oikein.

<img width="739" height="281" alt="image" src="https://github.com/user-attachments/assets/741413dc-86e5-4a50-b313-f10bcad43b04" />

Sitten viimein testasin SSH kirjautumista komennolla `ssh jemsku@localhost` ja minulta ei kysytty salasanaa ja pääsin suoraan käyttäjälle. Tarkistin vielä, että se oli oikeasti jemsku käyttäjä komennoilla `whoami`, joka kertoo kuka käyttäjä on kyseessä ja komennolla `pwd`, joka näyttää käyttäjän kotihakemiston.

<img width="480" height="145" alt="image" src="https://github.com/user-attachments/assets/a846be57-4f44-4566-a1ee-2c360546396f" />

## c) Kahden paketin asennus
Tämän tehtävän suoritukseen käytin tietoja, joita on `ansible-doc apt` dokumentaatiossa.

Päätin testata ladata tree ja curl paketit ansiblella. Menin muokkaamaan site.yml komennolla `micro site.yml`. Katsoin `ansible-doc apt` dokumentaatiosta esimerkeistä mallia ja lisäsin site.yml seuraavat tiedot:

<img width="176" height="128" alt="image" src="https://github.com/user-attachments/assets/d4deb0f7-830f-4f6b-8e84-dddc5892db34" />

Koska minulla oli ladattu jo tree ja curl, piti testata toimiiko lataus ensiksi poistamalla ne. Tämän tein komennolla `sudo apt remove tree curl`. Testasin vielä, että komennot eivät vastanneet.

<img width="365" height="215" alt="image" src="https://github.com/user-attachments/assets/d091020a-ba68-4ff9-abb7-34cdad9d26cc" />

Sitten ajoin komennon `ansible-playbook site.yml -K`, jossa näkyi changed=1.

<img width="737" height="308" alt="image" src="https://github.com/user-attachments/assets/118bb61b-6d98-41b1-8a81-3d112212d484" />

Tämän jälkeen kokeilin, että tree ja curl oli latautunut uudelleen testaamalla vastaavatko ne komentoon. Nyt olin siis saanut ladattua kaksi eri pakettia ansiblella.

<img width="461" height="353" alt="image" src="https://github.com/user-attachments/assets/43fb7d68-1228-4d63-8b13-07d88293a72d" />

## d) File
Tässä harjoituksessa käytin apuna `ansible-doc file` dokumentaatiota.

Aluksi loin uuden tiedoston komennolla `micro /home/jemima/tiedostoteksti.txt` ja kirjoitin sinne tekstiä. Seuraavaksi menin jälleen muokkaamaan site.yml tiedostoa. Lisäsin sieltä malli esimerkin `ansible-doc file` mukaan seuraavat tiedot:

<img width="244" height="187" alt="image" src="https://github.com/user-attachments/assets/18879185-eeec-412a-9771-3369974e2855" />

Ajoin komennon `ansible-playbook site.yml -K`, joka näytti changed=1 ja ei virheilmoituksia. Ajoin komennon vielä uudestaan ja sitten sain ok=8 ja muut 0.

<img width="737" height="340" alt="image" src="https://github.com/user-attachments/assets/f426249f-a6d6-4f0a-9174-ce0bc9e920ba" />

Oikeudet ovat tiedostolle, että omistajalla on 6 eli read ja write oikeudet, ryhmällä 4 eli read oikeudet ja kaikilla muilla ei ole mitään oikeuksia. Eli siis `-rw-r-----`, jossa ensimmäinen viiva - tarkoittaa että tiedosto ei ole hakemisto (d), rw- on omistajan osio (read, write, -), r-- ryhmän osio (read, -, -) ja --- kaikki muut eli ei mitään oikeuksia.

<img width="374" height="32" alt="image" src="https://github.com/user-attachments/assets/8d3e94f4-ee09-4b33-abc6-f1d0d027df35" />


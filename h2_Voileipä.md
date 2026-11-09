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
Tämän harjoituksen suorittamiseen käytin apuna Karvisen 2026 Sudo without password artikkelia.

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

Seuraavaksi testasin 






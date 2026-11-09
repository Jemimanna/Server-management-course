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





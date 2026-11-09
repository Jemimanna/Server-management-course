# H2 Voileipä
## x) Tiivistelmät
### Karvinen 2026: Sudo without password
* `sudo adduser antero` lisää käyttäjän antero
* `sudo groupadd sudoless` luo ryhmän nimeltä sudoless
* `sudo adduser antero sudoless` lisää antero-käyttäjän sudoless-ryhmään
* `sudo -i` avaa root käyttäjän. Toisen ikkunan pitäminen auki root oikeuksilla, kun muokkaa toisella sudoa, voi potentiaalisesti korjata sudon, jos sen rikkoo.
* `sudo visudo /etc/sudoers.d/sudoless` visudo ajaa editorin ja pääsee muokkaamaan /sudoers.d/sudoless -> `%sudoless ALL = (ALL) NOPASSWD: ALL` ei tarvitse käyttää salasanaa.
### Omat huomiot
* Ilmeisesti jos lisää nyt uusia käyttäjiä sudoless ryhmään sama sääntö pätee heihin. 
### Munroe 2006: xkcd 149: Sandwich
* Oma huomio: sudo antaa pääkäyttäjän oikeudet.
### Karvinen 2026: Passwordless Sudo with Ansible
*

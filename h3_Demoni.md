# H3 Demoni
## x) Tiivistelmät
### Karvinen 2026: Apache installed with Ansible - quick notes
* `sudo apt-get install apache2` lataa Apache 2.
* `sudoedit /etc/apache2/...` pääsee muokkaamaan Apache 2 tiedostoja.
* `sudo systemctl restart apache2` starttaa Apache2 kun halutut muokkaukset on tehty.
* `tasks/main.yml` sisältää tiedot ansiblen koodista.
* `handlers/main.yml` uudelleenkäynnistys suoritetut palvelut.
* `files/example.com.conf` hallinnoitu tiedosto, jossa tehdyt muutokset ylikirjoitetaan.
### Ansible Community Documentation: Handlers: running operations on change
* Handlerit ovat tehtäviä, jotka suoritetaan, kun tehdystä muutoksesta on ilmoitettu niille.
* `notify` käytetään ilmoittamaan nimetyille handlereille tehtävässä tapahtuneesta muutoksesta.
* `handlers` osiossa määritellään handlerien nimet ja tehtävät.
* Handleri suorittaa uudelleenkäynnistyksen vain kerran, vaikka samasta muutoksesta olisi ilmoitettu sille monta kertaa.
### Ansible-doc service
* Hallitsee palveluita etäkoneilla.
* `enabled` määritetään pitääkö palvelun käynnistyä bootin yhteydessä.
* `name` palvelun nimi.
* `state` määrittää palvelun tilan esim. `started`, `stopped`, `reloaded`, `restarted`.
* EXAMPLES: Käynnistää httpd palvelun, mikäli sitä ei ole jo käynnistetty (state: started).
```yaml
- name: Start service httpd, if not started
  ansible.builtin.service:
    name: httpd
    state: started
```
## y) 


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
## y) Bor - Open-source policy management for Linux desktops
* Bor on avoimen lähdekoodin järjestelmä tarkoitettu Linux-työasemien keskitettyyn hallintaan.
* Bor koostuu kevyestä Go agentista ja keskitetystä palvelimesta.
* Käytännöt välitetään suoratoistona clienteille reaaliajassa.
* Versio 0.8.0 lisäsi kolme uutta käytäntötyyppiä: Thunderbird, Microsoft Edge for Business ja Firewalld zones.
* Versio toi myös mm. vahvistetun tietoturva järjestelmän, mahdollisuuden jakaa ylläpitotehtäviä tarkemmin ja Web UI overhaul uudistuksen, joka muutti mm. navigaatiota ja lisäsi URL reitityksen.






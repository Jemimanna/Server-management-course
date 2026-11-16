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

## a) Apassi
Tämän tehtävän suoritukseen käytin apuna Karvisen 2026 Apache installed with Ansible - quick notes artikkelia.

Ensiksi päivitin pakettiluettelon komennolla `sudo apt-get update`. Tämän jälkeen asensin Apache2 komennolla `sudo apt-get install apache2`. Asennuksessa kesti hetki.

<img width="470" height="305" alt="image" src="https://github.com/user-attachments/assets/acc3ee30-d414-438d-9d46-921a7bbb7dd7" />

Tämän jälkeen menin komennolla `cd ansible` muokkaamaan ansiblen tiedostoja. Loin komennolla `mkdir -p /roles/apache2/files` uuden roolin apache2 ja sille alikansion files. Samalla tavalla loin vielä kaksi muuta hakemistoa apache2 roolille: `mkdir -p roles/apache2/handlers/` ja `mkdir -p roles/apache2/tasks`.

<img width="278" height="10" alt="image" src="https://github.com/user-attachments/assets/198bcea5-e7d3-4291-a2ca-6e5a9ee92533" />

<img width="292" height="20" alt="image" src="https://github.com/user-attachments/assets/9efb0e89-1705-4fd2-8819-907b44155859" />

Nyt puuni näytti seuraavan laiselta:

<img width="177" height="185" alt="image" src="https://github.com/user-attachments/assets/ca6ecefa-7073-45dd-8e29-e2a08f35c9f1" />

Käytin micro tekstieditoria luomaan main.yml tiedoston tasks hakemistoon, johon tulisi tehtävät, jotka haluan, että apache2 suorittaa. Tämän tein komennolla `micro roles/apache2/tasks/main.yml` ja pääsin muokkamaan main.yml tiedostoa. Sinne kirjoitin seuraavat tiedot:

<img width="307" height="197" alt="image" src="https://github.com/user-attachments/assets/ccde5584-76b0-4bf5-ad99-1a7c9d89aa7b" />

Katsoin vielä että ne tallentuivat komennolla `cat roles/apache2/tasks/main.yml`

<img width="303" height="212" alt="image" src="https://github.com/user-attachments/assets/e212e054-8aaf-4b01-928f-97473e518b65" />

Seuraavaksi menin muokkaamaan handlers hakemistoa ja lisäsin sinnekin main.yml tiedoston komennolla `micro roles/apache2/handlers/main.yml`. Sinne lisäsin seuraavat tiedot: 

<img width="133" height="45" alt="image" src="https://github.com/user-attachments/assets/59d4ae0d-97a8-4690-b782-6447701f952f" />

Jälleen tarkistin, että tiedot tallentuivat komennolla `cat roles/apache2/handlers/main.yml`.

<img width="323" height="71" alt="image" src="https://github.com/user-attachments/assets/be79f49b-d6a3-4245-8583-f7a4f9c232ee" />

Viimeisenä vielä loin files hakemistolle example.com.conf tiedoston komennolla `micro roles/apache2/files/example.com.conf`. Sinne lisäsin seuraavat tiedot:

<img width="245" height="80" alt="image" src="https://github.com/user-attachments/assets/cbf13898-2509-445f-8bee-5d4a9e020db0" />

Jälleen tarkistin, että tiedostot tallentuivat komennolla `cat roles/apache2/files/example.com.conf`

<img width="349" height="112" alt="image" src="https://github.com/user-attachments/assets/e2853696-1c87-4aae-b220-c79b26663df0" />






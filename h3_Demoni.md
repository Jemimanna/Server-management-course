# H3 Demoni
## x) Tiivistelmät
### Karvinen 2026: Apache installed with Ansible - quick notes
* `sudo apt-get install apache2` lataa Apache 2.
* `sudoedit /etc/apache2/...` pääsee muokkaamaan Apache 2 tiedostoja.
* `sudo systemctl restart apache2` starttaa Apache2 kun halutut muokkaukset on tehty.
* `tasks/main.yml` sisältää tiedot ansiblen koodista.
* `handlers/main.yml` uudelleenkäynnistys suoritetut palvelut.
* `files/example.com.conf` sisältää mm. polun html tiedostoon.
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

## a) Apache 2 asennus käsin
Tämän tehtävän suoritukseen käytin apuna Karvisen 2026 Apache installed with Ansible - quick notes artikkelia, Karvisen 2008 Install Apache Web Server on Ubuntu ja ChatGPT -tekoälyä, joka käytti lähteinään Apache HTTP Server Documentation - mod_userdir, Apache HTTP Server Documentation - Authorization ja Ubuntu Server Documentation - Apache2.

Ensiksi asensin Apache2 komennolla `sudo apt-get install apache2`. Asennuksessa kesti hetki.

<img width="470" height="305" alt="image" src="https://github.com/user-attachments/assets/acc3ee30-d414-438d-9d46-921a7bbb7dd7" />

Tämän jälkeen menin selaimessa osoitteeseen http//:localhost ja näin onnistuneesti Apachen sivut.

<img width="422" height="269" alt="image" src="https://github.com/user-attachments/assets/97a24599-f1cb-4591-9b82-a12faaa08aac" />

Seuraavaksi otin käyttäjien kotisivut käyttöön komennolla `sudo a2enmod userdir`. Jotta se menisi päälle ajoin komennon `sudo systemctl restart apache2`.

<img width="192" height="12" alt="image" src="https://github.com/user-attachments/assets/82a6f007-38bc-4859-a843-5f471636d633" />

<img width="243" height="11" alt="image" src="https://github.com/user-attachments/assets/2774bbbd-1072-4823-9de6-f47b1a5c2631" />

Tämän jälkeen testasin luoda public_html sivuston normaalina käyttäjänä. Menin kotihakemistooni ensin komennolla `cd` ja sitten loin public_hmtl hakemiston komennolla `mkdir public_html`. Tarkistin nimeni komennolla `whoami`, jotta voisin sitten asettaa sen selaimeen.

<img width="179" height="55" alt="image" src="https://github.com/user-attachments/assets/290e7d63-e225-427f-a58f-1a08af0bde85" />

Selaimeen kirjoitin http://localhost/~jemima/ ja aukesi seuraavanlainen sivu:

<img width="206" height="128" alt="image" src="https://github.com/user-attachments/assets/84a60795-fbbd-4042-bb9c-890f6fa3cae6" />

Tämä tarkoitti, että oikeudet oli asetettu väärin käyttäjille. Tehtävän loppusuorituksessa pyysin apua ChatGPT -tekoälyltä, sillä en ollenkaan tiennyt miten edetä tehtävässä. Se käytti lähteinä vastauksilleen Apache HTTP Server Documentation - mod_userdir, Apache HTTP Server Documentation - Authorization ja Ubuntu Server Documentation - Apache2.

Ajoin komennot `ls -ld ~` ja `ls -ld ~/public_html`, jotta näyttivät, mitkä oikeudet kotihakemistossa sekä public_html olivat.

<img width="352" height="54" alt="image" src="https://github.com/user-attachments/assets/40fae096-d9a2-40c4-9954-e71b08b95f56" />

Ajoin komennon `chmod 711 ~`, joka muutti kotihakemiston oikeudet niin että omistajalla on luku, kirjoitus ja suoritusoikeudet ja ryhmällä ja muilla on suoritusoikeus. Ajoin sitten komennon `chmod 755 ~/public_html`, joka muutti public_html omistajan oikeudet luku, kirjoitus ja suoritusoikeudet ja ryhmälle ja muille luku sekä suoritusoikeudet.

<img width="352" height="68" alt="image" src="https://github.com/user-attachments/assets/a427e702-7e88-4d30-8745-8d7812a33e29" />

Nyt käyttäjäsivusto näkyi oikeanlaisena:

<img width="245" height="161" alt="image" src="https://github.com/user-attachments/assets/a2cc815e-1967-49fe-a504-725d179ebeb0" />

Seuraavaksi piti tehdä niin, että sivusto näkyisi http://localhost osoitteessa. Aluksi loin index.html `micro ~/index.html` tiedoston jonne laitoin html koodia, joka näkyisi sitten nettisivun etusivuilla.

<img width="173" height="99" alt="image" src="https://github.com/user-attachments/assets/69e5e275-36d1-48f2-afe9-ae2851bb30cc" />

Nyt kun olin luonut koodin, jota pystyn muokata ilman sudoa, piti mennä muokkaamaan apache2 asetuksia. Ajoin komennon `sudo micro /etc/apache2/sites-available/000-default.conf`. Siellä vaihdoin DocumentRoot kohtaan /home/jemima/public_html polun ja lisäsin sen alapuolelle:

```yaml
<Directory /home/jemima/public_html>
    Require all granted
</Directory>
```
Tässä require all granted antoi kaikille pääsyn kansion sisältöön verkkosivuston kautta.

<img width="413" height="329" alt="image" src="https://github.com/user-attachments/assets/52de0337-245b-4823-9c96-7498636443e0" />

Sitten ajoin komennon `sudo apache2ctl configtest`, joka tarkisti oliko asetuksissa mitään kirjoitusvirheitä. Syntax OK viesti tarkoittaa, että kaikki oli kunnossa. Tämän jälkeen käynnistin apache2 uudelleen komennolla `sudo systemctl restart apache2`. Tässä vaiheessa sivustolla ei vielä näkynyt index.html tiedostoani, sillä olin vahingossa luonut sen kotihakemistooni enkä public_html hakemistoon. Joten siirsin sen sinne komennolla `mv ~/index_html ~/public_html/index_html`. Komento tarkoittaa move eli siirrä.

<img width="305" height="48" alt="image" src="https://github.com/user-attachments/assets/f687a33e-3185-4ca8-b85b-ed0d655816db" />

Nyt kun menin http://localhost sivustolle index.html kirjoittamani tiedot tulivat sivulla viimein esille.

<img width="454" height="384" alt="image" src="https://github.com/user-attachments/assets/ce09e20e-1c0c-41a6-b006-28c29a2df93e" />

Testasin vielä, että normaali käyttäjä pystyy muokkaamaan index.html ilman sudo oikeuksia. Tein testin ensiksi katsomalla, että olen normaali käyttäjä komennolla whoami.

<img width="122" height="35" alt="image" src="https://github.com/user-attachments/assets/fc0d349f-a96c-4804-b6ad-cc468892f9f3" />

Tämän jälkeen ajoin komennon `micro ~/public_html/index.html` ja vaihdoin tekstiä. Sitten tallensin ja menin katsomaan tuliko muutos nettisivuille. Muutos tuli näkyviin, joten tehtävä onnistui.

<img width="221" height="146" alt="image" src="https://github.com/user-attachments/assets/c5b5ec18-097c-4534-8dfb-26743f11d886" />

## b) Nginx asennus käsin
Tämän tehtävän suoritukseen käytin apuna aiemmassa tehtävässä oppimiani asioita,  Karvisen 2008 Install Apache Web Server on Ubuntu, Karvisen H3 Demoni vinkit kohtaa ja ChatGPT -tekoälyä, joka käytti lähteinään Nginx Beginner's Guide ja Ubuntu Server How to configure Nginx.

Ennen kuin pystyin ladata Nginx piti sammuttaa Apache 2. Tein tämän komennolla `sudo systemctl stop apache2` ja sen jälkeen tarkistin sammuiko se komennolla `systemctl is-active apache2`, joka antoi vastaukseksi inactive eli se oli poissa päältä.

<img width="236" height="59" alt="image" src="https://github.com/user-attachments/assets/38ab64f3-963a-4039-8d13-3873da243ec7" />

Seuraavaksi päivitin pakettiluettelon komennolla `sudo apt-get update` ja latasin nginx komennolla `sudo apt-get install nginx`.

<img width="470" height="400" alt="image" src="https://github.com/user-attachments/assets/2ce1916f-8c25-4032-a795-9d283094c4f8" />

Nyt kun se oli ladattu niin käynnistin sen komennolla `sudo systemctl start nginx` ja tarkistin, että se oli active komennolla `systemctl status nginx`.

<img width="468" height="179" alt="image" src="https://github.com/user-attachments/assets/f9790b6d-7387-4cdf-939a-2e114d745bc0" />

Seuraavaksi menin muokkaamaan nginx asetuksia komennolla `sudo micro /etc/nginx/sites-available/default`. Käytin seuraavaa kohtaa mallina ja myös aiemmin tekemiäni asetuksia mitä apacheen laitoin.

<img width="404" height="129" alt="image" src="https://github.com/user-attachments/assets/85423cb8-e66e-49d1-ac4d-0122de2575ef" />

Muokkasin root polun `/home/jemima/public_html;` ja lisäsin sen alapuolelle `index index.html;`.

<img width="206" height="34" alt="image" src="https://github.com/user-attachments/assets/6f317a9f-7e19-48fc-a811-95b4f63fea89" />

Tämän jälkeen tarkistin, että nginx asetukset olivat oikein komennolla `sudo nginx -t`, joka toimii suht samalla tavalla kuin apachen `sudo apache2ctl configtest`, jota käytin aiemmassa tehtävässä. Sain vastaukseksi, että test is successful eli kaikki oli ok.

<img width="334" height="40" alt="image" src="https://github.com/user-attachments/assets/42fb315a-bb52-4651-8d37-53be23a6f40b" />

Sitten käynnistin nginx uudestaan komennolla `sudo systemctl restart nginx`, jotta asetukset tulisivat voimaan. Menin tarkistamaan, että aiemmin tekemäni index.html sivuston näkyisi http://localhost sivulla nginx välityksellä. En tajunnut ottaa kuvaa, mutta ennen kuin nginx asetukset oli asetettu näkyi pelkkä Apachen oletussivusto eikä index.html enään. Nyt kuitenkin sivusto tuli jälleen näkyviin.

<img width="286" height="101" alt="image" src="https://github.com/user-attachments/assets/dc3d447e-7980-4c0b-955b-346e3f3fe3cd" />

Jälleen piti vielä testata, että normi käyttäjä voi muokata sivustoa, joten tarkistin käyttäjäni komennolla `whoami`.

<img width="139" height="35" alt="image" src="https://github.com/user-attachments/assets/c91a10f7-602f-4ee8-983f-ebf5eb4d6cb5" />

Sitten menin muokkaamaan index.html koodia komennolla `micro ~/public_html/index.html`. Lisäsin sinne tekstiä ja menin katsomaan muuttuiko http://localhost sivuston tiedot. Tekstit oli ilmestynyt eli muokkaus onnistui ilman sudoa normi käyttäjänä nginxin avulla.

<img width="302" height="131" alt="image" src="https://github.com/user-attachments/assets/c00d23e1-a159-41a3-9d12-d94c7a741d5b" />

## c) Automaattinen Nginx asennus Ansiblella
Tämän tehtävän suoritukseen käytin apuna Karvisen 2026 Apache Installed with Ansible - quick notes artikkelia ja ChatGPT -tekoälyä virheen selvityksessä, joka käytti lähteinään Nginx Beginner's Guide ja Ubuntu Server How to configure Nginx.

Aluksi menin ansible hakemistoon komennolla `cd ansible` ja vielä `cd roles`. Sitten loin kolme uutta roolia komennoilla `mkdir -p nginx/files`, `mkdir -p nginx/handlers` ja `mkdir -p nginx/tasks`. Tämän jälkeen puu näytti tältä:

<img width="284" height="247" alt="image" src="https://github.com/user-attachments/assets/d0ad0ef0-8cb0-4e6e-919b-f2d7da11e359" />

Seuraavaksi loin tasks hakemistoon main.yml tiedoston, johon lisäsin koodia. Tein tämän komennolla `micro roles/nginx/tasks/main.yml`.
```yaml
- apt:
   name: nginx
   state: present

- copy:
    dest: "/etc/nginx/sites-available/example.com.conf"
    src: "example.com.conf"
    owner: "root"
    group: "root"
    mode: "0644"
  notify: restart nginx

- file:
    src: /etc/nginx/sites-available/example.com.conf
    dest: /etc/nginx/sites-enabled/example.com.conf
    owner: root
    group: root
    state: link
  notify: restart nginx
```
Tässä koodissa annetaan paketille nginx nimi, copy kohta kopioi asetustiedostot oikeaan kansioon, file luo linkin sites-available ja sites-enabled välillä ja notify ilmoittaa seuraavaksi luomalleni handlerille, että pitää restartata mikäli muutoksia tehdään.

<img width="295" height="203" alt="image" src="https://github.com/user-attachments/assets/c9b8dcbf-cbf3-40e2-921f-befacff1c4b0" />

<img width="297" height="209" alt="image" src="https://github.com/user-attachments/assets/63f33769-af6b-4a5f-9700-b9b5c6f57cde" />

Seuraavaksi loin handlers hakemistolle main.yml tiedoston komennolla `micro roles/handlers/main.yml`. Sinne lisäsin seuraavan koodin:
```yaml
- name: restart nginx
  systemd:
     name: nginx
     state: restarted
```
Tässä koodissa annetaan handlerille nimeksi restart nginx ja määritetään, että se restarttaa nginx palvelun uudelleen pyydettäessä.

<img width="148" height="58" alt="image" src="https://github.com/user-attachments/assets/5dd432f7-33e3-41b7-9385-8284c9161b62" />

<img width="302" height="63" alt="image" src="https://github.com/user-attachments/assets/99f4fbf9-b30e-48ad-ae71-2b84692bd8b6" />

Sitten loin files hakemistolle aiemmin mainitsemani example.com.conf tiedoston komennolla `micro roles/nginx/files/example.com.conf`. Sitä varten minun piti etsiä oikeat asetukset. Ajoin komennon `cat /etc/nginx/sites-available/default` ja löysin sieltä esimerkit:

<img width="198" height="57" alt="image" src="https://github.com/user-attachments/assets/1fbbb579-f10f-4297-ba5b-1b994aee20a9" />

<img width="410" height="174" alt="image" src="https://github.com/user-attachments/assets/0a73fd72-a549-4910-ba67-9840f38858b1" />

Sitten lisäsin seuraavaan koodin example.com.conf tiedostoon:
```yaml
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /home/jemima/public_html;
        index index.html;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
Tässä koodissa asetin 80 portin oletuspalvelimen portiksi, root kohdassa on polku, josta nettisivut löytyvät ja location etsii asettamiani tiedostoja.

<img width="328" height="127" alt="image" src="https://github.com/user-attachments/assets/13a48ac5-dc8d-42ef-8f75-f47481914983" />

Seuraavaksi menin site.yml tiedostoon lisäämään rooleihin nginx roolin, jotta ansible-playbook toimisi. 

<img width="123" height="68" alt="image" src="https://github.com/user-attachments/assets/14084adf-3413-4224-b8e5-b0d30a1a653c" />

Sitten ajoin komennon `ansible-playbook site.yml -K` ja sain seuraavan virheilmoituksen:

<img width="943" height="142" alt="image" src="https://github.com/user-attachments/assets/97b296b2-3526-45dd-bf95-9aa7aded85ce" />

Menin muokkaamaan nimettyä polkua `- name` kohtaa. Koodi näytti oikealta, mutta varmuuden vuoksi kumitin pois viivan ja laitoin sen uudelleen. Tämän jälkeen tallensin ja ajoin `ansible-playbook site.yml -K` uudelleen ja tulos oli kaikki ok.

<img width="814" height="407" alt="image" src="https://github.com/user-attachments/assets/d06482e8-e5a6-4a9d-8110-f57450f27b63" />

Sivusto kuitenkin ei näyttänyt index.html enään, joten jokin oli pielessä. Tässä kohdassa pyysin ChatGPT -tekoälyltä apua ja se käytti lähteinään Nginx Beginner's Guide ja Ubuntu Server How to configure Nginx. Ajoin komennon `sudo systemctl status nginx` ja se näytti failed eli jokin oli pielessä.

<img width="469" height="229" alt="image" src="https://github.com/user-attachments/assets/1d1dc37c-2199-4e9e-952c-6f72ff314593" />

Tarkistin tilanteen komennolla `sudo nginx -t`. Ilmeisesti ongelma oli siinä, että oli duplikaatio default serveristä, joka piti nyt poistaa.

<img width="470" height="71" alt="image" src="https://github.com/user-attachments/assets/b1d3cb09-6fc0-408b-9124-39d7eb6a68da" />

Poistin sen komennolla `sudo rm /etc/nginx/sites-enabled/default` ja sitten ajoin uudelleen `sudo nginx -t`, joka sanoi, että test is successful. Sitten vielä restarttasin nginxin komennolla `sudo systemctl restart nginx`.

<img width="338" height="143" alt="image" src="https://github.com/user-attachments/assets/7b9ed321-6467-4d73-aa67-40a5d8d5ed2f" />

Nyt index.html sivusto tuli jälleen näkyviin. Testasin myös vielä ihan lopuksi ajaa uudelleen `ansible-playbook site.yml -K` ja se näytti kaikki ok.

<img width="266" height="98" alt="image" src="https://github.com/user-attachments/assets/7b52d3b8-1274-454c-b850-f199ddd253b7" />

<img width="782" height="408" alt="image" src="https://github.com/user-attachments/assets/c2cb11cc-7790-4eab-8658-7a0ffd44c831" />

## Lähteet
* Apache. Access Control. Luettavissa: https://httpd.apache.org/docs/2.4/howto/access.html. Luettu: 16.11.2026.
* Apache. Apache Module mod_userdir. Luettavissa: https://httpd.apache.org/docs/2.4/mod/mod_userdir.html. Luettu: 16.11.2026.
* Ansible Community Documentation. Handlers: running operations on change. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html. Luettu: 16.11.2026.
* Ansible-doc service sivut. Luettu: 16.11.2026.
* eniac111. Show HN: Bor - Open-source policy management for Linux desktops. Hacker News. Luettavissa: https://news.ycombinator.com/item?id=49142569. Luettu: 16.11.2026.
* Karvinen, T. 2026. Apache installed with Ansible - quick notes. Luettavissa: https://terokarvinen.com/apache-ansible/. Luettu: 16.11.2026.
* Karvinen, T. 2008. Install Apache Web Server on Ubuntu. Luettavissa: https://terokarvinen.com/2008/05/02/install-apache-web-server-on-ubuntu-4/index.html. Luettu: 16.11.2026.
* Karvinen, T. 2026. Palvelinten hallinta: h3 Demoni: vinkit. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#laksyt. Luettu: 16.11.2026.
* Nginx. Beginner's Guide. Luettavissa: https://nginx.org/en/docs/beginners_guide.html. Luettu: 16.11.2026.
* Petrov, B. 2.8.2026. Bor v0.8.0 released. Luettavissa: https://getbor.dev/blog/2026-08-02-bor-v080-release/. Luettu: 16.11.2026.
* Ubuntu. How to configure nginx. Luettavissa: https://ubuntu.com/server/docs/how-to/web-services/configure-nginx. Luettu: 16.11.2026. 
* Ubuntu. How to install Apache2. Luettavissa: https://ubuntu.com/server/docs/how-to/web-services/install-apache2. Luettu: 16.11.2026.

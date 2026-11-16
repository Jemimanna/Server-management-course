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
Tämän tehtävän suoritukseen käytin apuna Karvisen 2026 Apache installed with Ansible - quick notes artikkelia, Karvisen 2008 Install Apache Web Server on Ubuntu ja ChatGPT -tekoälyä.

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

Tämä tarkoitti, että oikeudet oli asetettu väärin käyttäjille. Tehtävän loppusuorituksessa pyysin apua ChatGPT -tekoälyltä, sillä en ollenkaan tiennyt miten edetä tehtävässä. Se käytti lähteinä vastauksilleen Apache HTTP Server Documentation - mod_userdir, Apache HTTP Server Documentation - Authorization ja Ubuntu Server Documentation - Apache2 (2026).

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

<img width="413" height="329" alt="image" src="https://github.com/user-attachments/assets/52de0337-245b-4823-9c96-7498636443e0" />

Sitten ajoin komennon `sudo apache2ctl configtest`, joka tarkisti oliko asetuksissa mitään kirjoitusvirheitä. Syntax OK viesti tarkoittaa, että kaikki oli kunnossa. Tämän jälkeen käynnistin apache2 uudelleen komennolla `sudo systemctl restart apache2`. Tässä vaiheessa sivustolla ei vielä näkynyt index.html tiedostoani, sillä olin vahingossa luonut sen kotihakemistooni enkä public_html hakemistoon. Joten siirsin sen sinne komennolla `mv ~/index_html ~/public_html/index_html`

<img width="305" height="48" alt="image" src="https://github.com/user-attachments/assets/f687a33e-3185-4ca8-b85b-ed0d655816db" />

Nyt kun menin http://localhost sivustolle index.html kirjoittamani tiedot tulivat sivulla viimein esille.

<img width="454" height="384" alt="image" src="https://github.com/user-attachments/assets/ce09e20e-1c0c-41a6-b006-28c29a2df93e" />

Testasin vielä, että normaali käyttäjä pystyy muokkaamaan index.html ilman sudo oikeuksia. Tein testin ensiksi katsomalla, että olen normaali käyttäjä komennolla whoami.

<img width="122" height="35" alt="image" src="https://github.com/user-attachments/assets/fc0d349f-a96c-4804-b6ad-cc468892f9f3" />

Tämän jälkeen ajoin komennon `micro ~/public_html/index.html` ja vaihdoin tekstiä. Sitten tallensin ja menin katsomaan tuliko muutos nettisivuille. Muutos tuli näkyviin, joten tehtävä onnistui.

<img width="221" height="146" alt="image" src="https://github.com/user-attachments/assets/c5b5ec18-097c-4534-8dfb-26743f11d886" />

## b) Moottorix
Ennen kuin pystyin ladata Nginx piti sammuttaa Apache 2. Tein tämän komennolla `sudo systemctl stop apache2` ja sen jälkeen tarkistin sammuiko se komennolla `systemctl is-active apache2`, joka antoi vastaukseksi inactive eli se oli poissa päältä.

<img width="236" height="59" alt="image" src="https://github.com/user-attachments/assets/38ab64f3-963a-4039-8d13-3873da243ec7" />

Seuraavaksi päivitin pakettiluettelon komennolla `sudo apt-get update` ja latasin nginx komenolla `sudo apt-get install nginx`.

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




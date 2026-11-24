# H4 Pizza Fantasia
## Tiivistelmät
### Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks: 4.12.1 Size and Complexity of Some DSLs
* Salt DSL-kieli on todella laaja ja sisältää paljon dokumentaatiota.
* Jinja2 malleja käytetään YAML-koodin tuottamiseen.
* Puppetissa ohjausrakenteet eroavat huomattavasti muista tunnetuista ohjelmointikielistä.
### Omat huomiot
* DSL-kielet ovat hyvin laajoja ja usein monimutkaisia. Ne usein sisältävät monta kerrosta, joten on vaikeaa kuvitella, että joku osaisi ulkoa yhtäkään niistä.
### Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks: 4.12.2 Use of DSL Functions in Case Configuration
* Erilaisia funktioita käytetään vähän konfiguraatioissa.
* Yleisimmin käytettyjä funktioita olivat `exec`, `file`, `package`, `group` ja `service`. 
* `exec` funktion käytössä käyttäjän pitää varmistaa, että toiminnon suoritus monta kertaa tuottaa saman lopputilan kun sen suoritus kerran eli että toiminto on idempotentti.
* Pieni määrä funktioita kattaa suuren osan tarpeista eri konfiguraatioille.
### Omat huomiot
* Useimmiten käytetään vain yleisimmin käytettyjä funktioita kongifuraatioissa. Niitä voidaan käyttää montakin kertaa samassa tehtävässä.
* Tämä myös helpottaa tekemistä, sillä ei ole miljoonaa eri funktiota, joiden käyttö tarvitsisi osata.
* Mihin kaikkiin muihin tarkoituksiin on tehty eri funktioita ja ovatko ne oikeasti hyödyllisiä, jos niitä käytetään todella harvoin?
### Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks: 4.12.3.1 Dependencies Between Main Functions
* Järjestelmän pitää olla idempotentti eli muutoksia tehdään vain, jos järjestelmä ei ole jo halutussa tilassa.
* Idempotenttisuus voidaan toteuttaa ehtorakenteiden avulla.
* `exec` idempotenttisuuden toteuttaminen riippuu siitä ohjelmasta, joka sen pitäisi suorittaa.
* `exec` ja `file` toimintojen avulla voidaan toteuttaa muiden funktioiden idempotenttisuus esim. `service`, `package` ja `group`.
### Omat huomiot
* Yksinkertaiset ns. ydin toiminnot helpottavat muiden funktioiden idempotenttisuuden toteuttamista.
* Tämä vähentää työmäärää idempotenttisuuden toteuttamisessa.

## a) Caddy demonin asennus
Aluksi päivitin pakettiluettelon komennolla `sudo apt-get update`, jonka jälkeen latasin Caddyn komennolla `sudo apt-get install caddy`.

<img width="476" height="399" alt="image" src="https://github.com/user-attachments/assets/4aee6b55-3544-4fe8-8866-a495ecffa413" />

Seuraavaksi laitoin pois päältä apache2 ja nginx komennoilla `sudo systemctl stop apache2` ja `sudo systemctl stop nginx`. Tämä pysäytti ne. 

<img width="232" height="93" alt="image" src="https://github.com/user-attachments/assets/8ceaf16a-5ec4-4b67-a01f-34aabd5247c2" />

Tämän jälkeen ajoin vielä `sudo systemctl disable apache2` ja `sudo systemctl disable nginx`, jotka estivät niiden automaattisen käynnistymisen kun kone käynnistyy.

<img width="470" height="109" alt="image" src="https://github.com/user-attachments/assets/f0ea850b-68af-449d-bf84-fb19fa9ffdff" />

Tarkistin olivatko ne päällä komennoilla `sudo systemctl status apache2` ja `sudo systemctl status nginx`. Tulokseksi sain inactive ja failed eli ne olivat poissa päältä. En ollut varma, mitä nginxin vastaus failed tarkoitti. Ne pitivät laittaa pois päältä, jotta Caddy pystyisi käyttämään porttia 80.

<img width="469" height="204" alt="image" src="https://github.com/user-attachments/assets/f25319bf-9907-412c-966e-238032ba75f5" />

<img width="475" height="221" alt="image" src="https://github.com/user-attachments/assets/0d729869-73fa-4e32-bfc4-e8aa669cbfe2" />

Seuraavaksi laitoin caddyn päälle komennolla `sudo systemctl enable --now caddy` ja tarkistin, että se meni päälle komennolla `sudo systemctl status caddy`. Sain vastauksen active eli se oli päällä.

<img width="466" height="296" alt="image" src="https://github.com/user-attachments/assets/a8aa0b21-b69c-49d8-b62c-d214355bf9ae" />

Seuraavaksi menin tarkistamaan, että caddy oikeasti toimi menemällä selaimeen. Siellä kirjoitin osoiteriville http://localhost ja sain caddyn sivut esiin. Tämä osoitti, että asennus onnistui.

<img width="480" height="423" alt="image" src="https://github.com/user-attachments/assets/5a5e90a0-e3bf-4fd7-86f8-da19b259ad4a" />

## b) Caddy demonin automatisaatio ansiblella
Seuraavaksi caddyn asentaminen piti automatisoida ansiblen avulla. Aluksi menin ansible hakemistooni komennolla `cd ansible`. Sitten loin caddylle oman hakemiston ansiblen sisälle, johon voisin asettaa automaattisen latauksen. Tein hakemiston komennolla `mkdir -p roles/caddy/tasks`. Tässä -p luo myös puuttuvat ylähakemistot, jos niitä on ja tasks olisi se paikka, johon luon itse koodin.

<img width="266" height="238" alt="image" src="https://github.com/user-attachments/assets/dcc7e959-4ae8-4516-b7c7-3a75603a3691" />

Etsin `ansible-doc apt` sisällöstä esimerkkejä ja käytin myös apuna H3 tehtävääni.

<img width="291" height="56" alt="image" src="https://github.com/user-attachments/assets/9b8a5ef3-103b-44b1-8969-730d42f6a9c7" />

Seuraavaksi loin caddylle main.yml tiedoston, johon itse koodi tulisi. Tein tämän komennolla `micro roles/caddy/tasks/main.yml`.

Asetin main.yml seuraavat tiedot:
```yaml
- name: Caddy lataus
  apt:
    name: caddy
    state: present

- name: Caddy käynnistys
  systemd:
     name: caddy
     state: started
     enabled: yes
```

<img width="150" height="121" alt="image" src="https://github.com/user-attachments/assets/355d5cc4-bf34-4df1-a0a2-34f6ce54fe78" />

Tarkistin vielä komennolla `cat roles/caddy/tasks/main.yml`, että koodi oli tallentunut.

<img width="296" height="134" alt="image" src="https://github.com/user-attachments/assets/6caaad99-df87-4304-bc85-fe1cf1e2cf79" />

Seuraavaksi minun piti lisätä caddy rooli site.yml kansioon. Ajoin komennon `micro site.yml` ja lisäsin roles kohtaan caddy roolin.

<img width="107" height="71" alt="image" src="https://github.com/user-attachments/assets/f9d76e3e-59b6-4840-870a-4103b0383dd8" />

Seuraavaksi testatakseni, että koodi käynnistää caddyn, pistin sen ensiksi pois päältä komennolla `sudo systemctl stop caddy` ja tarkistin, että se oikeasti meni pois komennolla `systemctl is-active caddy`. Vastaus oli inactive eli nyt se oli poissa päältä.

<img width="268" height="109" alt="image" src="https://github.com/user-attachments/assets/2161c535-620f-4ba9-ac6a-acb2c44fbe6f" />

Tämän jälkeen ajoin playbookin komennolla ``








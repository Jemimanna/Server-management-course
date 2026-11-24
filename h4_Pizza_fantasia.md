# H4 Pizza Fantasia
## x) Tiivistelmät
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
Tämän tehtävän suoritukseen käytin aiemmin tekemääni tehtävää h3_Demoni ja ansible-doc sivuja.

Aluksi päivitin pakettiluettelon komennolla `sudo apt-get update`, jonka jälkeen latasin Caddyn komennolla `sudo apt-get install caddy`.

<img width="476" height="399" alt="image" src="https://github.com/user-attachments/assets/4aee6b55-3544-4fe8-8866-a495ecffa413" />

Seuraavaksi laitoin pois päältä apache2 ja nginx komennoilla `sudo systemctl stop apache2` ja `sudo systemctl stop nginx`. Tämä pysäytti ne. 

<img width="232" height="93" alt="image" src="https://github.com/user-attachments/assets/8ceaf16a-5ec4-4b67-a01f-34aabd5247c2" />

Tämän jälkeen ajoin vielä `sudo systemctl disable apache2` ja `sudo systemctl disable nginx`, jotka estivät niiden automaattisen käynnistymisen kun kone käynnistyy.

<img width="470" height="109" alt="image" src="https://github.com/user-attachments/assets/f0ea850b-68af-449d-bf84-fb19fa9ffdff" />

Tarkistin olivatko ne päällä komennoilla `sudo systemctl status apache2` ja `sudo systemctl status nginx`. Tulokseksi sain inactive ja failed eli ne olivat poissa päältä. En ollut varma, mitä nginxin vastaus failed tarkoitti. Ne pitivät laittaa pois päältä, jotta Caddy olisi se, jota käytetään.

<img width="469" height="204" alt="image" src="https://github.com/user-attachments/assets/f25319bf-9907-412c-966e-238032ba75f5" />

<img width="475" height="221" alt="image" src="https://github.com/user-attachments/assets/0d729869-73fa-4e32-bfc4-e8aa669cbfe2" />

Seuraavaksi laitoin caddyn päälle komennolla `sudo systemctl enable --now caddy` ja tarkistin, että se meni päälle komennolla `sudo systemctl status caddy`. Sain vastauksen active eli se oli päällä.

<img width="466" height="296" alt="image" src="https://github.com/user-attachments/assets/a8aa0b21-b69c-49d8-b62c-d214355bf9ae" />

Seuraavaksi menin tarkistamaan, että caddy oikeasti toimi menemällä selaimeen. Siellä kirjoitin osoiteriville http://localhost ja sain caddyn sivut esiin. Tämä osoitti, että asennus onnistui.

<img width="480" height="423" alt="image" src="https://github.com/user-attachments/assets/5a5e90a0-e3bf-4fd7-86f8-da19b259ad4a" />

## b) Caddy demonin automatisaatio ansiblella
Tämän tehtävän suoritukseen käytin aiemmin tekemääni tehtävää h3_Demoni ja ansible-doc sivuja.

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
Koodissa enabled:yes käynnistää caddyn automaattisesti kun kone käynnistyy.

<img width="150" height="121" alt="image" src="https://github.com/user-attachments/assets/355d5cc4-bf34-4df1-a0a2-34f6ce54fe78" />

Tarkistin vielä komennolla `cat roles/caddy/tasks/main.yml`, että koodi oli tallentunut.

<img width="296" height="134" alt="image" src="https://github.com/user-attachments/assets/6caaad99-df87-4304-bc85-fe1cf1e2cf79" />

Seuraavaksi minun piti lisätä caddy rooli site.yml kansioon. Ajoin komennon `micro site.yml` ja lisäsin roles kohtaan caddy roolin.

<img width="107" height="71" alt="image" src="https://github.com/user-attachments/assets/f9d76e3e-59b6-4840-870a-4103b0383dd8" />

Seuraavaksi testatakseni, että koodi käynnistää caddyn, pistin sen ensiksi pois päältä komennolla `sudo systemctl stop caddy` ja tarkistin, että se oikeasti meni pois komennolla `systemctl is-active caddy`. Vastaus oli inactive eli nyt se oli poissa päältä.

<img width="268" height="109" alt="image" src="https://github.com/user-attachments/assets/2161c535-620f-4ba9-ac6a-acb2c44fbe6f" />

Tämän jälkeen ajoin playbookin komennolla `ansible-playbook site.yml -K`. En saanut virheilmoitusta ja tuli ilmoitus changed=2.

<img width="305" height="35" alt="image" src="https://github.com/user-attachments/assets/c13e9224-9e7a-448a-945b-7887a82604a1" />

<img width="476" height="206" alt="image" src="https://github.com/user-attachments/assets/53770b7d-d4c5-4f83-8868-0d5a21394ab0" />

Nyt kun ajoin `systemctl is-active caddy` sain vastaukseksi active eli automaattinen käynnistys ansiblella onnistui.

<img width="267" height="43" alt="image" src="https://github.com/user-attachments/assets/1756ccf5-8b8f-422c-acff-0dce94134ccb" />

Ajoin vielä kerran `ansible-playbook site.yml -K` tarkistaakseni, että nyt muutoksia ei tehtäisi, sillä caddy oli jo päällä. Vastaus oli ok=14 ja muut nollaa eli se toimi oikein.

<img width="476" height="205" alt="image" src="https://github.com/user-attachments/assets/874005db-4095-4c30-836e-80d45cfa6abe" />

## c) Asetustiedostojen muutos
Tämän tehtävän tekemiseen käytin apuna h3_Demoni tehtävääni, ansible-doc sivuja ja ChatGPT -tekoälyä, joka käytti lähteenään Caddy Documentation: The Caddyfile, respond, Caddyfile Quick-start ja Keep Caddy Running.

File hakemiston luomiseen ja sen sisällöissä käytin apuna ChatGPT:n antamia lähteitä. Loin aluksi caddylle file hakemiston komennolla `mkdir -p roles/caddy/files` ja sinne `micro roles/caddy/files/asetus`, johon kirjoittaisin uudet tiedot. Kirjoitin sinne:
```yaml
:80 {
    respond "Uusi asetus toimii"
}
```
Respond attribuutti palauttaa laitetun tekstin webbipalvelimelle. 80: on taas portti, jota caddy käyttää.

<img width="194" height="35" alt="image" src="https://github.com/user-attachments/assets/05870311-a1e4-4a08-a73f-fd17c85c48c3" />

Tämän jälkeen uusi asetus piti lisätä suoritettavaksi main.yml sisälle. Menin siis sinne komennolla `micro roles/caddy/tasks/main.yml`. Lisäsin sinne:
```yaml
- name: Caddy kopio
  copy:
    src: asetus
    dest: /etc/caddy/Caddyfile
```
Tässä koodissa copy vie uuden asetuksen caddylle src -> dest. Eli tekemäni `asetus` siirtyy `/etc/caddy/Caddyfile` sijaintiin. Muutin myös caddy käynnistys kohdan state kohdan restarted eli, että se käynnistää caddyn uudelleen.

<img width="192" height="155" alt="image" src="https://github.com/user-attachments/assets/dd54e870-29c1-4c60-b4bf-dfa45925bfc5" />

Sitten ajoin playbookin komennolla `ansible-playbook site.yml -K` ja sain vastaukseksi changed=2.

<img width="469" height="232" alt="image" src="https://github.com/user-attachments/assets/4f21a73c-be74-4bbb-bced-0d516688135f" />

Sitten ajoin komennon `cat /etc/caddy/Caddyfile`, johon nyt oli tullut asetus tiedostoon laittamani teksti. Tämä osoitti, että tiedoston tiedot oli kopioitu oikeaan osoitteeseen.

<img width="257" height="53" alt="image" src="https://github.com/user-attachments/assets/45d36db1-f105-45ea-a04d-2f2c1038523a" />

Seuraavaksi restarttasin caddyn komennolla `sudo systemctl restart caddy` ja testasin tulisiko laittamani teksti nyt näkyviin curlin avulla. Tein tämän komennolla `curl http://localhost` ja sain laittamani respond tekstin vastauksena.

<img width="254" height="26" alt="image" src="https://github.com/user-attachments/assets/2f170cd3-b5a4-42c8-be41-56d902afc947" />

Myös nyt kun menin selaimessa sivustolle, näkyviin tuli laittamani teksti.

<img width="182" height="73" alt="image" src="https://github.com/user-attachments/assets/de5994b8-625d-4d7a-9326-d3f29f3c748c" />

## d) Asetusten rikkominen ja korjaus ansiblella
Päätin poistaa aiemmassa tehtävässä käyttämäni asetustiedoston Caddyfile. Aluksi tarkistin, että se näkyy komennolla `ls /etc/caddy`.

<img width="214" height="50" alt="image" src="https://github.com/user-attachments/assets/341bee6c-10c2-4a9c-88a6-fd2e80fceda0" />

Seuraavaksi poistin sen komennolla `sudo rm /etc/caddy/Caddyfile`.

<img width="273" height="77" alt="image" src="https://github.com/user-attachments/assets/dc027d89-d0a6-464d-b5b5-9e0c201559de" />

Tässä vaiheessa huomasin, että main.yml tiedostossani täytyi vaihtaa kopioinnin ja käynnistyksen paikat, jotta kopiointi suoritettaisiin ennen käynnistämistä. Muutin siis niiden paikat.

<img width="175" height="151" alt="image" src="https://github.com/user-attachments/assets/eab1278e-e82e-44d9-a892-07e663c48162" />

Seuraavaksi ajoin `ansible-playbook site.yml -K` nähdäkseni kopioiko se Caddyfilen uudelleen ja korjaa virheen. Kuvassa näkyy, että kopiointi suoritetaan ensin ja siinä tulee changed.

<img width="449" height="206" alt="image" src="https://github.com/user-attachments/assets/3d87b16d-56d1-403f-ae38-b4a0b5d93587" />

Nyt kun ajoin `ls /etc/caddy` Caddyfile oli jälleen siellä.

<img width="210" height="47" alt="image" src="https://github.com/user-attachments/assets/27e99e5a-fec7-4549-a4b7-4380362d5900" />

Myös curl ja nettisivu toimivat.

<img width="239" height="26" alt="image" src="https://github.com/user-attachments/assets/7b9cf81b-662f-4e06-8973-694ef0abfe80" />

<img width="170" height="67" alt="image" src="https://github.com/user-attachments/assets/f11283b6-0a2a-4100-9f6a-ce00e63f907c" />

## e) Idempotenttisuus
Viime tehtävän kuvassa näkyy changed=2, joka tarkoittaa, että kahta asiaa muutettiin playbookin suorituksessa. Todistaakseni idempotenttisuuden, että muutoksia ei tehdä, mikäli järjestelmä on jo halutussa tilassa, ajoin playbookin uudestaan. Sain changed=1 kohdassa käynnistys, sillä olin asettanut restarted komennon tapahtumaan jokaisella kerralla.

<img width="400" height="238" alt="image" src="https://github.com/user-attachments/assets/0f3b60f6-b614-4a57-9ccd-3bee62c74e43" />

Korjatakseni tämän menin muokkaamaan main.yml ja vaihdoin restarted kohdan takaisin started.

<img width="134" height="61" alt="image" src="https://github.com/user-attachments/assets/af3db6c1-f868-4d7b-a0d6-cb1e1c288bbf" />

Sitten ajoin playbookin ja sain vastaukseksi ok=15 ja muut nollaa. Ajoin sen vielä kerran varmuuden vuoksi ja vastaus oli edelleen ok=15 muut nollaa. Idempotenttisuus siis toteutui.

<img width="788" height="403" alt="image" src="https://github.com/user-attachments/assets/775c91b3-ed53-4a2e-a204-f0d689126b48" />

## f) Pari hankittu: Sofia Rinne

## Lähteet
* Ansible-doc sivut. Luettu: 24.11.2026.
* Caddy Documentation. Caddyfile Quick-Start. Luettavissa: https://caddyserver.com/docs/quick-starts/caddyfile. Luettu: 24.11.2026.
* Caddy Documentation. Keep Caddy Running. Luettavissa: https://caddyserver.com/docs/running. Luettu: 24.11.2026.
* Caddy Documentation. Respond. Luettavissa: https://caddyserver.com/docs/caddyfile/directives/respond. Luettu: 24.11.2026.
* Caddy Documentation. The Caddyfile. Luettavissa: https://caddyserver.com/docs/caddyfile. Luettu: 24.11.2026.
* Karvinen, T. 2023. Configuration Management of Distributed Systems over Unreliable and Hostile Networks. Westminster research. Luettavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks. Luettu: 24.11.2026.
* Rontti, J. 2026. H3 Demoni. Github. Luettavissa: https://github.com/Jemimanna/Server-management-course/blob/main/h3_Demoni.md. Luettu: 24.11.2026.







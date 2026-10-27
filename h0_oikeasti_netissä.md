# h0 Oikeasti netissä tehtävät
## Tiivistelmä: Niemi 2025: H4 Tehtävät
### Vuokraa pilvipalvelin
* Niemen H4 tehtävissä (2025) kerrotaan, kuinka vuokrataan virtuaalipalvelin UpCloud. Aluksi valittiin sijainti perustuen omaan sijaintiin, sillä täten serveri toimii nopeammin.
* Seuraavaksi valittiin suunnitelma, kuinka paljon prosessoreita ja muistia tahdotaan omaan serveriin. Mitä suuremman määrän valitsi, sitä korkeampi hinta oli.
* Operaatio systeemiksi Niemi valitsi Debian GNU/Linux 13 (Trixie) kurssia varten.
* SSH avaimet Niemi loi Linux terminaalissa komennolla ssh-keygen ja kopioi sieltä julkisen avaimen tiedot kirjautumiskohtaan UpCloudiin.
### Virtuaalipalvelimen alkutoimet
* Seuraavaksi otettiin etäyhteys palvelimeen komennolla ssh root@ip_osoite. IP-osoite oli se, joka juuri luotiin UpCloudissa.
* Käyttäjät lisättiin komennolla adduser nimi sudo. Sudo komennon lopussa teki käyttäjästä automaattisesti sudo-käyttäjän.
* Käyttäjän kotihakemistoon luotiin mkdir komennolla uusi hakemisto SSH avaimia varten ja sinne lisättiin julkinen avain (.pub).
* Palomuurin asennus tehtiin niin, että ensin päivitettiin komennolla apt-get update ja sitten asennettiin palomuuri apt-get install ufw. Portin 22 liikenne sallittiin komennolla ufw allow 22/tcp ja kun nämä asetukset oli tehty, palomuuri käynnistettiin ja tarkistettiin komennoilla ufw enable ja ufw status.
* Root tili lukittiin komennolla sudo usermod --lock root ja PermitRootLogin vaihdettiin "no". Palvelin käynnistettiin uudelleen komennolla sudo service ssh restart.
### Web-palvelin
* Apache2 ladattiin ensin päivittämällä järjestelmä komennolla sudo apt update ja sitten sudo apt-get install apache2.
* Kaikkien käyttäjien pääsy selaimeen sallittiin komennolla sudo ufw allow 80/tcp ja sudo ufw enable.
* sudo apt-get update, sudo get-apt dist-upgrade, sudo apt install unattended-upgrades ja sudo apt-get install bash completion olivat lopussa käytettyjä komentoja, mutta niitä ei selitetty auki. Lopuksi tehtiin kokonainen reboottaus komennolla systemctl reboot.
### Omat huomiot
* sudo pitää olla todella monen komennon edessä, jotta se suoritetaan järjestelmänvalvojan oikeuksilla.
* apt-get komentoa käytettiin päivittämiseen ja asioiden lataamiseen.
## Tiivistelmä: Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi
* Partasen tehtävässä Nimekäs (2025) ensimmäisessä kohdassa selitetään, miten hankkia julkinen domain nimi virtuaalikoneelle.
* Nettisivuja, josta nimiä voi hankkia on monta. ässä etsittiin halvin vaihtoehto ja siihen valittiin domain privacy päälle.
* Tehtiin käyttäjä palveluun ja maksettiin domain nimestä.
* Advanced DNS kohdasta sai asetettua nettisivun osoittamaan virtuaalipalvelimelle poistamalla CNAME tietueen ja lisäämällä A Record tietueen, joka viittaa virtuaalipalvelimelle.
* www kohta viittaa itse nettisivuun.
* @ viittaa domain osoitteeseen.
### Omat huomiot
* Nettisivuja, joista nimiä voi hankkia on monia erilaisia.
* Mikäli omalla sivuston nimellä ei ole väliä, kannattaa vaihdella nimeä, kunnes löytää mahdollisimman halvan.

## Omat tehtävät
### a) Debian 13 asennus
Käytin tehtävässä apuna Karvisen ohjetta Install Debian on Virtualbox (2021). Sieltä painoin linkkiä ja aloitin tehtävän lataamalla debian-live-13.7.0-amd64-xfce.iso tiedoston koneelleni. 


## Lähteet
* Karvinen, T. 2021. Install Debian on Virtualbox. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu: 27.10.2026.
* Niemi. 2025. H4 Tehtävät. Luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md. Luettu: 26.10.2026.
* Partanen, A. 2025. Harjoitus 5: Nimekäs. Luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html. Luettu: 26.10.2026.

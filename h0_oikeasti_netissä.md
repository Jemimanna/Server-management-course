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
Käytin tehtävässä apuna Karvisen ohjetta Install Debian on Virtualbox (2021). Sieltä painoin linkkiä ja aloitin tehtävän lataamalla debian-live-13.7.0-amd64-xfce.iso tiedoston koneelleni. Minulla oli VirtualBox jo ladattuna, joten valitsin sieltä expert install, jotta pystyin laittamaan oikeat konfiguraatiot virtuaalikoneelleni. Painon VirtualBoxissa New ja sieltä sain luotua uuden virtuaalikoneen. Aluksi asetin koneelle nimen sekä ISO Image kohtaan debian-live-13 ISO tiedoston. Käyttöjärjestelmäksi valitsin Linuxin ja OS versioon Debian(64-bit). Proceed with Unattended Installion täppää en klikannut, jotta voin itse konfiguroida haluamani asetukset.
<img width="471" height="182" alt="image" src="https://github.com/user-attachments/assets/5478b290-ea45-406c-9d53-bc4cc76a2e3e" />

Base memory kohtaan asetin 4090 MB, CPU:n määrän kahteen ja täppä use EFI kohtaan.
<img width="466" height="114" alt="image" src="https://github.com/user-attachments/assets/beba72ee-42a7-4a06-95af-21601de75aa8" />

Asetin Virtual Hard Disk kooksi 60 GB ja painoin Finish.
<img width="482" height="296" alt="image" src="https://github.com/user-attachments/assets/0fe10713-70d9-4c37-bcd0-22fc3747a9ac" />

Nyt kun olin saanut asetukset laitettua, käynnistin virtuaalikoneen Start with GUI painikkeella. Sain seuraavan kuvan ja valitsin ylimmän vaihtoehdon Enterillä eli Live system (amd64)
<img width="399" height="361" alt="image" src="https://github.com/user-attachments/assets/d50203ab-1e52-449a-88b2-fab7e2a9b0fa" />

Nyt olin päässyt koneeseen, testasin toimintaa painamalla Applications ja sieltä Web Browser.
<img width="640" height="461" alt="image" src="https://github.com/user-attachments/assets/b50da333-83dc-4c2d-9d4b-d9860eb909d4" />

Testasin toimintaa hakemalla "Tero Karvinen" ja sieltä löysin sivuston.
<img width="640" height="467" alt="image" src="https://github.com/user-attachments/assets/daa62a4d-e87f-4489-b651-fa7bff7a390c" />

Seuraavaksi oli asennus, klikkasin Install Debian kuvaketta.
<img width="52" height="55" alt="image" src="https://github.com/user-attachments/assets/fc73b90b-38b4-43b7-8d48-d3002188255e" />

Siitä aukesi Calamares installer ja asetin siellä seuraavat asetukset:
* Language: American English
* Location: Region: Europe, Zone: Helsinki
  <img width="410" height="223" alt="image" src="https://github.com/user-attachments/assets/763f4864-e7c0-465a-a67f-397200bc4443" />
* Keyboard: Finnish (testasin toimintaa vasemmassa alanurkassa)
  <img width="416" height="272" alt="image" src="https://github.com/user-attachments/assets/220099f6-422f-4b7b-9397-3b7a4ab2231b" />
* Partitions: Erase disk: valitaan, Encrypt system: ei valita
* Users: (Asetin salasanan kuvan jälkeen)
  <img width="430" height="298" alt="image" src="https://github.com/user-attachments/assets/8418c149-3dd0-4090-a556-6d852666868a" />

Asetukset olivat valmiit ja klikkasin install.
<img width="431" height="327" alt="image" src="https://github.com/user-attachments/assets/50342fe3-b555-4915-aeda-f11820543dcc" />

Lataus alkoi.
<img width="422" height="302" alt="image" src="https://github.com/user-attachments/assets/566efc4d-96ba-46bc-add4-546dadb6c48b" />

Pääsin kirjautumaan aiemmin asettamillani käyttäjätunnuksilla.
<img width="636" height="462" alt="image" src="https://github.com/user-attachments/assets/e459a12b-e3b3-459e-b312-d43aefd7bb6a" />

Näin sain Debianin asennettua.
<img width="632" height="456" alt="image" src="https://github.com/user-attachments/assets/053f527f-b274-43e6-96fb-1ec491050d90" />

Päivitin listaukset komennolla sudo apt-get update ja sudon takia piti antaa myös salasanani.
<img width="531" height="247" alt="image" src="https://github.com/user-attachments/assets/fd3b74b8-5cf1-4525-81cc-d631161e373b" />

Seuraavaksi komento sudo apt-get -y dist-upgrade, joka päivitti kaiken.
<img width="403" height="367" alt="image" src="https://github.com/user-attachments/assets/4868f903-3b1b-402a-8164-b87f3ab83520" />

Palomuurin asennus komennolla sudo apt-get -y install ufw ja laitoin sen päälle komennolla sudo ufw enable.
<img width="250" height="33" alt="image" src="https://github.com/user-attachments/assets/d14fc12c-fd1e-49f8-a7db-af338438cd71" />

Tämän jälkeen reboottasin systeemin painamalla Applications, Log out ja Restart.
<img width="638" height="404" alt="image" src="https://github.com/user-attachments/assets/31b60edf-fbed-4f97-b788-9132b8cb05bc" />

Tein vielä VirtualBox Guest Additions asennuksen, jotta käyttäminen helpottuu ja ruutu skaalautuu paremmin. Aluksi klikkasin Devices ja sieltä Insert Guest Additions CD Image.
<img width="635" height="462" alt="image" src="https://github.com/user-attachments/assets/b85161b1-9b1b-4e70-b776-312aa138dce9" />

Sain seuraavanlaisen virheilmoituksen ja en tiennyt miten se korjataan, joten pyysin apua ChatGPT -tekoälyltä. Se käytti pohjana tiedoilleen Shivam Malanin artikkelia Fix VirtualBox "Unable to insert the virtual optical disk" (VERR_PDM_MEDIA_LOCKED) (2026).

<img width="206" height="331" alt="image" src="https://github.com/user-attachments/assets/90758e24-0941-441d-a476-140692b89bb8" />

Sain ohjeeksi ensin sulkea koko virtuaalikoneen ja sitten mennä VirtualBoxissa asetuksiin ja sieltä Storage->etsiä CD/DVD asema ja klikata levy kuvaketta->remove disk from virtual drive. Sitten käynnistin Debianin uudelleen.

Näin sain korjattua ongelman ja siirryin eteenpäin. Klikkasin uudelleen Insert Guest Additions CD Image ja nyt se näkyi kun avasin File managerin.
<img width="326" height="262" alt="image" src="https://github.com/user-attachments/assets/4da01c07-c6b3-43e2-a65e-ba512f8847a5" />

Sitten menin Applications ja terminaaliin, siellä laitoin komennot:
<img width="433" height="218" alt="image" src="https://github.com/user-attachments/assets/308d2f18-b921-48f2-a05a-62876e7b1eab" />

Tämän jälkeen suoritin jälleen rebootin Applications->Log out->Restart. Kirjauduin sisään ja nyt skaalautuvuus toimi oikein. Nyt 
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/94882537-7023-4454-a146-f9180bfeab8a" />



## Lähteet
* Karvinen, T. 2021. Install Debian on Virtualbox. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu: 27.10.2026.
* Niemi. 2025. H4 Tehtävät. Luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md. Luettu: 26.10.2026.
* Partanen, A. 2025. Harjoitus 5: Nimekäs. Luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html. Luettu: 26.10.2026.

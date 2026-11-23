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

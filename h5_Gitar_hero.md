# H5 Gitar hero
## Tiivistelmät
### Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git?
* Git tallentaa sen hetkisen tilan `commit` komennolla. Se toimii niin että se ottaa snapshotin siitä, miltä tiedostot näyttävät sillä hetkellä ja tallentaa sen.
* Jos tiedostoa ei muokattu, Git ei tallenna sitä uudelleen, vaan käyttää aiemmin tallennettua samaa tiedostoa.
* Git on vähän niin kuin mini tiedostojärjestelmä.
* Gittiä voi käyttää ilman internetiä tai VPN:ää.
* Tallennetut tiedot tarkistetaan tarkistussumman avulla -> Git huomaa mahdolliset muutokset.
* Gitillä on kolme päätilaa: Modified eli tiedostoa on muokattu, Staged eli tiedosto on lisätty menemään seuraavaan committiin ja Committed eli että muutokset on tallennettu.
* Working tree kohdassa tiedostoja muokataan, staging area kohdassa lisätään ne muutokset, mitä halutaan seuraavaan committiin ja Git directoryyn tallennetaan eri versiot.
### git add --all && git commit; git pull && git push
* Foster (n.d.) kertoo artikkelissaan Git add, commit, and push ja artikkelissa How to add all tracked files in Git, että `git add -all` kohdassa `git add` ottaa muokatut tiedostot ja lisää ne staging areaan ja `--all` lisää kaikki tehdyt muutokset staging areaan, myös uudet, muokatut ja poistetut tiedostot.
* Fosterin (n.d.) artikkelin Git add, commit, and push mukaan `git commit` komento muuntaa staging areassa olevan sisällön snapshotiksi, joka tallentuu Git-arkiston historiaan.
* Fosterin (n.d.) artikkelissa How to use git pull kerrotaan, että `git pull` komento päivittää nykyisen paikallisen haaran etähaaraassa olevilla uusimmilla muutoksilla.
* Fosterin (n.d.) artikkelin Git add, commit, and push mukaan `git push` komento lähettää paikalliset commitit etä repositoryyn esim. GitHubiin.

## a) Online
Menin Githubiin ja tein sinne uuden repositoryn.

<img width="1193" height="150" alt="image" src="https://github.com/user-attachments/assets/a008fd43-d56c-4ad8-92bc-af133944ac55" />

Asetin nimeksi sunshine-repository ja kuvaavan tekstin. Jätin näkyvyyden Public. Sitten laitoin Add README päälle ja lisenssiin GNU General Public License 3.

<img width="766" height="763" alt="image" src="https://github.com/user-attachments/assets/c0eed7da-ccb7-4050-a70a-2a46be26abcf" />

<img width="914" height="477" alt="image" src="https://github.com/user-attachments/assets/bd47c93b-8083-4844-a289-6cd6a3175d8b" />

## b) Dolly
Tämän tehtävän suoritukseen käytin tunnilla (26.11.2026) oppimiani asioita ja h5 Gitar Hero vinkkejä kohtaa.

Aluksi menin hakemaan olemassa olevani SSH public key. Tein tämän komennolla ensin menemällä ssh kansioon `cd .ssh` ja siellä `ls`, jotta näin avaimeni nimen. Sitten `cat id_ed25519.pub`, jotta näen avaimen sisällön. Kopioin avaimen ja siirryin Githubiin.

<img width="482" height="62" alt="image" src="https://github.com/user-attachments/assets/6aa0aa20-02bd-4abb-ad4b-00f4edfa99ec" />

Githubista menin omasta profiilikuvasta settings ja sieltä kohtaan SSH and GPG keys. Painoin New SSH key ja lisäsin juuri kopioimani julkisen avaimen tiedot siihen.

<img width="939" height="265" alt="image" src="https://github.com/user-attachments/assets/c5dbde4f-f02e-4e23-8387-f1f5438ec3ea" />

Sitten Githubista menin uuteen sunshine-repositoryyni. Sieltä kohdasta code painoin SSH ja kopioin esitetyn linkin.

<img width="932" height="487" alt="image" src="https://github.com/user-attachments/assets/553526e7-baf6-493e-9c76-6b92495c5f59" />

Sitten suoritin komennon `git clone git@github.com:Jemimanna/sunshine-repository.git`, joka kloonaisi tiedot.

<img width="388" height="83" alt="image" src="https://github.com/user-attachments/assets/2d310299-eace-465b-a20d-36ae0794143f" />

Seuraavaksi komennolla `ls` näin, että sunshine-repository oli ilmestynyt ja menin sinne komennolla `cd sunshine-repository`.

<img width="488" height="56" alt="image" src="https://github.com/user-attachments/assets/f77c9e81-1540-468f-bb72-467b908070cf" />

Menin muokkaamaan Githubiin luomaani README.md tiedostoa komennolla `micro README.md`. Lisäsin sinne tekstiä lisää ja tallensin.

<img width="500" height="256" alt="image" src="https://github.com/user-attachments/assets/611d4989-28eb-4416-8cc3-9de0d2abd108" />

Sitten ajoin komennon `git add .` ja `git commit`. Commit messageen laitoin Updating README.md ja tallensin.

<img width="502" height="233" alt="image" src="https://github.com/user-attachments/assets/0119e3f2-3e9b-4272-a683-2a482a5766cd" />

Seuraavaksi ajoin komennon `git push`, jonka tarkoitus oli siirtää juuri tekemäni muutokset Githubiin.

<img width="306" height="136" alt="image" src="https://github.com/user-attachments/assets/e1d47710-b483-465d-a146-e17c8ddbcb89" />

Tämän jälkeen menin repositoryyni Githubissa ja katsoin tulivatko muutokset.

<img width="475" height="245" alt="image" src="https://github.com/user-attachments/assets/541a9a6c-66fc-446d-9656-1daed2c23204" />

Tekstin lisäys tuli esiin eli push onnistui!

## c) Doh!
Menin jälleen muokkaamaan README.md tiedostoa komennolla `micro README.md` ja lisäsin sinne jotain outoa tekstiä.

<img width="503" height="242" alt="image" src="https://github.com/user-attachments/assets/8789b75a-b704-463b-9089-f168d0c547a0" />

Käytin `git status` komentoa katsoakseni, että muutos on otettu huomioon.

<img width="359" height="109" alt="image" src="https://github.com/user-attachments/assets/77b3a08c-278d-44a1-9c2c-1d6606bcd844" />

Sitten poistin muutoksen komennolla `git reset --hard`.

<img width="278" height="32" alt="image" src="https://github.com/user-attachments/assets/b598410a-306d-48ba-b582-f8d7c68c30be" />

Nyt kun ajoin uudelleen `git status` se sanoi, että kaikki on up to date.

<img width="245" height="59" alt="image" src="https://github.com/user-attachments/assets/7339a1b8-af8e-47fc-b8ce-384946de4cf7" />

Katsoin vielä komennolla `cat README.md`, että muutokset oli poistettu.

<img width="299" height="47" alt="image" src="https://github.com/user-attachments/assets/1591d6f5-e8ee-4c2e-8a91-9ce2058bc3c7" />

## d) Tukki



## Lähteet
* Foster, G. n.d. Git add, commit, and push. Graphite. Luettavissa: https://graphite.com/guides/git-add-commit-push. Luettu: 28.11.2026.
* Foster, G. n.d. How to add all tracked files in Git. Graphite. Luettavissa: https://graphite.com/guides/add-all-tracked-files. Luettu: 28.11.2026.
* Foster, G. n.d. How to use git pull. Graphite. Luettavissa: https://graphite.com/guides/how-to-use-git-pull. Luettu: 28.11.2026.
* Chacon, S. & Straub, B. 2014. Getting Started - What is Git?. Pro Git. Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu: 28.11.2026.

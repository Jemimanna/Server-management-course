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
Ajoin komennon `git log --patch`, joka näytti kaikki commitit, mitä olin tehnyt. Siellä näkyi myös Githubissa oleva GNU General Public Licence Version 3 kokonaan. Se oli committina ja siinä ei näkynyt, että sitä olisi muokattu mitenkään.

<img width="409" height="248" alt="image" src="https://github.com/user-attachments/assets/7346c56c-be15-4eb9-b63d-d940fc76cd59" />

Alussa näkyi viimeisin tekemäni commit ja siihen laittamani selittävä viesti. Siinä myös näkyy päivämäärä, jolloin loin sen. Punaisella tekstillä (-) näkyy, miltä teksti näytti aiemmin ja vihreällä (+) sen alapuolella näkyy, miten tekstiä muutettiin.

<img width="443" height="140" alt="image" src="https://github.com/user-attachments/assets/8721a36d-f675-4ee7-a062-f350cb3808ba" />

Olin lisännyt koulussa nimeni sekä sähköpostini gitin latauksen yhteydessä komennoilla `git config --global user.email "jemmaro100@gmail.com"` sekä `git config --global user.name "Jemima Rontti"`. Nimeni ja sähköpostini näkyivät halutulla tavalla.

<img width="347" height="52" alt="image" src="https://github.com/user-attachments/assets/701a13a9-42c4-4f5b-a913-9c277954097f" />

## e) Gitanbile
Menin aluksi ansible kansiooni `cd ansible` ja siellä ajoin komennot `git init`, joka alusti ansible kansion versionhallintaan. Sitten lisäsin kaikki tiedostot sinne `git add .` komennolla.

<img width="409" height="136" alt="image" src="https://github.com/user-attachments/assets/615c17fc-2667-472b-9b8a-6f62cf0c4528" />

Tämän jälkeen ajoin `git commit`, joka tallensi tiedot.

<img width="350" height="208" alt="image" src="https://github.com/user-attachments/assets/7c537b11-e783-44c0-ad52-715aeb44e77a" />

<img width="273" height="134" alt="image" src="https://github.com/user-attachments/assets/a6667859-bce6-4371-98fa-3c83c23e7a79" />

Menin muokkaamaan content kohtaa `cd roles/hello/tasks/main.yml` tiedosto polusta. 

<img width="157" height="57" alt="image" src="https://github.com/user-attachments/assets/ca80c926-e93c-49d3-b801-c700dbfad062" />

<img width="192" height="44" alt="image" src="https://github.com/user-attachments/assets/5eee0905-fbb6-4c04-8a63-a1287ab2dcaa" />

Sitten ajoin `ansible-playbook site.yml -K`, jotta muutokset tulisivat voimaan ja siellä näkyi muutos.

<img width="366" height="121" alt="image" src="https://github.com/user-attachments/assets/4a0f2bbc-3d55-452d-85cb-6f13399c84de" />

<img width="452" height="35" alt="image" src="https://github.com/user-attachments/assets/932a6279-f26a-4731-a71e-6dcb6048c168" />

Sitten vein muutokset gitillä komennolla `git add` ja tallensin komennolla `git commit`, jonne jälleen laitoin kuvaavan lyhyen tekstin.

<img width="362" height="89" alt="image" src="https://github.com/user-attachments/assets/93a5ad46-eea4-4a24-9e64-212564dc1ea4" />

Ajoin vielä `git log --patch`, jotta näkisin tekemäni muutoksen ja siellä se oli. Punaisella näkyy vanha teksti ja vihreällä uusi.

<img width="347" height="115" alt="image" src="https://github.com/user-attachments/assets/59e94e15-9a2a-4879-8fc7-a8c27d425ac3" />


## Lähteet
* Foster, G. n.d. Git add, commit, and push. Graphite. Luettavissa: https://graphite.com/guides/git-add-commit-push. Luettu: 28.11.2026.
* Foster, G. n.d. How to add all tracked files in Git. Graphite. Luettavissa: https://graphite.com/guides/add-all-tracked-files. Luettu: 28.11.2026.
* Foster, G. n.d. How to use git pull. Graphite. Luettavissa: https://graphite.com/guides/how-to-use-git-pull. Luettu: 28.11.2026.
* Chacon, S. & Straub, B. 2014. Getting Started - What is Git?. Pro Git. Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu: 28.11.2026.

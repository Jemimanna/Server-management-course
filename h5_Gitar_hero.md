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





## Lähteet
* Foster, G. n.d. Git add, commit, and push. Graphite. Luettavissa: https://graphite.com/guides/git-add-commit-push. Luettu: 28.11.2026.
* Foster, G. n.d. How to add all tracked files in Git. Graphite. Luettavissa: https://graphite.com/guides/add-all-tracked-files. Luettu: 28.11.2026.
* Foster, G. n.d. How to use git pull. Graphite. Luettavissa: https://graphite.com/guides/how-to-use-git-pull. Luettu: 28.11.2026.
* Chacon, S. & Straub, B. 2014. Getting Started - What is Git?. Pro Git. Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu: 28.11.2026.

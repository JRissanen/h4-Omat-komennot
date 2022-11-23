# h4-Omat-komennot
This is a repository for Configuration Management Systems course exercise 4

__a) Hei komento! Tee järjestelmään uusi "hei maailma" -komento ja asenna se orjille Saltilla. Liitä raporttiisi orjan 'ls -l /usr/local/bin/' tulosteesta ainakin se rivi, jolla näkyy uuden komentotiedostosi oikeudet.__

Ensin tein uuden kansion komennolla: `sudo mkdir /srv/salt/helloworld` ja sinne kansion "Scripts", jonne laitoin scriptit.</br>
Tein scriptin `helloworld.sh` nano-tekstieditorilla, jonka määritin toistamaan lauseen "Hello World!", scriptin sisältö oli `echo "Hello World!"`. </br>
Kopioin scriptin "/usr/local/bin/" kansioon komennolla: `sudo cp helloworld.sh /usr/local/bin/`, jotta se toimisi kaikilla Slave-koneilla.</br>
Tein `init.sls`-tiedoston "helloworld" kansioon.</br>
Nyt `init.sls`-tiedosto määrittää `helloworld.sh`-tiedoston oikeudet valmiiksi oikein kohdassa `- mode: '0755'`. </br>
```
/usr/local/bin/helloworld.sh:
  file.managed:
    - source: salt://helloworld/Scripts/helloworld.sh
    - mode: '0755'
```
Ennen kuin ajoin tilan Saltilla, kävin vaihtamassa "/usr/local/bin/" kansiossa olevan `helloworld.sh`-tiedoston oikeudet.
Se onnistui komennolla: `sudo chmod 644 helloworld.sh`. </br>
Tämän tekemällä pystyin varmistamaan, että tila oikeasti muuttaa oikeudet haluttuun "755" muotoon.

Ajoin tilan kaikille orjille komennolla: `sudo salt '*' state.apply helloworld`, ja se näytti toimivan. </br>
Tarkistin vielä "/usr/local/bin/" hakemistosta, miltä oikeudet näytti ja kaikki näytti hyvältä.

![Screenshot 2022-11-22 145707](https://user-images.githubusercontent.com/116954333/203319706-2cf38102-8e16-4e9a-9c97-f2c8c659046b.png)

__b) whatsup.sh. Tee järjestelmään uusi komento, joka kertoo ajankohtaisia tietoja; asenna se orjille. Vinkkejä: Voit näyttää valintasi mukaan esimerkiksi päivämäärää, säätä, tietoja koneesta, verkon tilanteesta...__

Ensin tein harjoitukselle sopivan kansion: `sudo mkdir /srv/salt/whatsup`. </br>
Tein sinne komennolla: `sudoedit whatsup.sh`-tiedoston, jonka määritin kertomaan päivämäärän: </br>
```
echo 'Today is:'
date +'%d/%m/%y'
```
Testasin scriptin toimivuuden komennolla: `bash whatsup.sh`

Sitten tein samanlaisen `init.sls`-tiedoston: `sudoedit init.sls` kuin a) kohdassa: </br>
```
/usr/local/bin/whatsup.sh:
  file.managed:
    - source: salt://whatsup/whatsup.sh
    - mode: '0755'
```

Kopioin myös `whatsup.sh`-scriptin "/usr/local/bin/" hakemistoon: `sudo cp whatsup.sh /usr/local/bin/`. </br>
Sitten ajoin tilan kaikille Slave-koneille komennolla: `sudo salt '*' state.apply whatsup`. </br>
Kaikki näytti toimivan niinkuin pitikin.

![Screenshot 2022-11-22 155240](https://user-images.githubusercontent.com/116954333/203331358-9e137975-52dd-40bc-ae1c-1474ecbfa241.png)

__c) hello.py. Tee järjestelmään uusi komento Pythonilla ja asenna se orjille. Vinkkejä: Hei maailma riittää, mutta propellihatut saavat toki koodaillakin. Shebang on "#!/usr/bin/python3". Helpoin Python-komento on: print("Hei Tero!")__

Aloitin tekemällä harjoitukselle oman kansion: `sudo mkdir /srv/salt/hellopy`. </br>
Tein sinne `hello.py`-tiedoston: `sudoedit hello.py`, ja sisällöksi koodin, joka tulostaa "Hello!" horisontaalisti sekä vaakasuorasti.

```
#!/usr/bin/python3
print("H e l l o !")
greeting = "H e l l o !"
for i in greeting:
  if i == "H" or i == " ":
    continue
  print(i)
```
Testasin koodin toimivuuden: `python3 hello.py`, ja se näytti toimivan oletuksien mukaan.

![Screenshot 2022-11-22 161913](https://user-images.githubusercontent.com/116954333/203337525-8c2c9050-63f2-4dee-ac87-a3349b9610d4.png)

Sitten tein `init.sls`-tiedoston: `sudoedit init.sls`, niin kuin aiemmissakin kohdissa: 
```
/usr/local/bin/hello.py:
  file.managed:
    - source: salt://hellopy/hello.py
    - mode: '0755'
```

Sitten kopioin samaan tyyliin: `sudo cp hello.py /usr/local/bin/`. </br>
Ajoin tilan kaikille Slave-koneille komennolla: `sudo salt '*' state.apply hellopy`. </br>
Kaikki toimi kuten pitikin.

![Screenshot 2022-11-22 163747](https://user-images.githubusercontent.com/116954333/203341719-d2fdf41b-80da-46eb-951b-ce8e669a2af6.png)

__d) Laiskaa skriptailua. Tee kansio, josta jokainen skripti kopioituu orjille.__

Aloitin jälleen tekemällä uuden kansion harjoitusta varten: `sudo mkdir /srv/salt/recurse`. </br>
Tein "recurse" kansion sisään uuden kansion scriptejä varten: `sudo mkdir scripts`. </br>
Kopioin kaikki aiemmissa harjoituksissa luomani scriptit tähän uuteen "scripts" kansioon: </br>
`sudo cp <tiedoston nimi> /srv/salt/recurse/scripts/`.

![Screenshot 2022-11-22 165925](https://user-images.githubusercontent.com/116954333/203606293-f79cb51b-2ee1-4c32-a268-cf0c43fcc13d.png)

Vaihdoin aiemmin luomieni scriptien oikeudet kansiossa "/usr/local/bin/" muotoon "644", jotta näkisin ajaessani uuden tilan, että vaihtuuko se kaikilla muotoon "755", niin kuin olisi tarkoitus. </br>
Oikeuksien vaihto: `sudo chmod 644 hello.py helloworld.sh whatsup.sh`.

Tein `init.sls`-tiedoston:
```
/usr/local/bin/:
  file.recurse:
    - source: salt://recurse/scripts/
    - mode: '0755'
```

Yritin ajaa tilaa: `sudo salt '*' state.apply recurse`, mutta sain seuraavan virheilmoituksen:

![Screenshot 2022-11-22 171438](https://user-images.githubusercontent.com/116954333/203609519-8b1885c0-b3e8-4602-84cd-84334f00dfe5.png)

Virheilmoituksesta tajusin, että `init.sls`-tiedostoa täytyy muuttaa. </br>
Poistin kohdan `- mode: '0755'` ja lisäsin tiedostoon kohdat `- dir_mode: '0755'` ja `- file_mode: '0755'`, kuten virheilmoituksessa neuvottiin tekemään. </br>
Nyt `init.sls`-tiedosto näyttää seuraavalta: </br>
```
/usr/local/bin/:
  file.recurse:
    - source: salt://recurse/scripts/
    - dir_mode: '0755'
    - file_mode: '0755'
```

Ajoin tilan uudestaan muutosten jälkeen: `sudo salt '*' state.apply recurse`. </br>
Kuinka ollakaan, kaikki toimi.

![Screenshot 2022-11-23 195538](https://user-images.githubusercontent.com/116954333/203616343-b58a6dc7-bf55-4db0-8da6-9c476600325e.png)

__e) Intel. Etsi kolme loppuprojektia joltain vanhalta kurssitoteutukselta. Kuvaile projektit tiiviisti, viittaa ja linkitä alkuperäiseeen raporttin. Tässä alakohdassa ei tarvitse vielä kokeilla mitään koneella, vaan voit kuvailla niitä oheismateriaalin perusteella.__

Simo Tossavainen: Simppeli hyötyohjelma moduli. </br>
Linkki: https://simotossavainen.wordpress.com/2021/05/19/h7-oma-moduli/

* Modulin tarkoitus on laittaa firefox muistamaan edellinen istunto, eli vanhat välilehdet.
* Ohjelma asentaa libre officen, treen ja firefoxin.
* Asetusten muokkaukset firefox selaimella näkyivät hyvin kun config tiedostoja tarkkailtiin terminaalissa.
* Tosiaan simppeli ja käytännöllinen moduli.

Roope, Palvelinten Hallinta: Harjoitus 6 raportti. </br>
Linkki: https://roopelinux.wordpress.com/2018/05/11/palvelinten-hallinta-harjoitus-6-raportti/

* Projektin aihe oli WordPressin asentaminen Saltilla.
* WordPress tietokanta luotiin WordPressin oman Mysql-ohjeen pohjalta.
* Saltilla luotiin omat tilat seuraaville ohjelmille:
 * apache-state
 * mysql-state
 * database-state
 * wordpress-state
* Valmiin modulin loppukäyttäjiä ovat esimerkiksi ihmiset, jotka haluavat nopeasti kokeilla WordPressia.

Ilmo Gröhn, Palvelinten Hallinta H7b – Oma Moduli. </br>
Linkki: https://ilmogrohn.wordpress.com/2021/05/18/palvelinten-hallinta-h7b/

* Projektin tarkoitus oli luoda Saltilla tila, joka asentaa Slave-koneille libre officen ja siihen käyttäjän haluaman fontin.
* Alussa käytettiin perus `cmd.run whoami` kaltaisia komentoja.
* Etsittiin `find` komennon avulla oikea libre officen tiedosto tietokoneelta, jota voitiin muokata.
* Testi tehtiin kahdelle eri koneelle lopuksi ja molemmille onnistui libre officen asennus ja fontin vaihtaminen Salt tilan avulla.

__e) Lukua, ei luottamusta. Kokeile yhtä kohdassa d-Intel löytämääsi modulia koneella. Tämä on infraa koodina, joten luottamusta ei tarvita. Voit lukea koodista, mitä olet ajamassa.__

En saanut valitettavasti kokeiltua yhtäkään moduulia.

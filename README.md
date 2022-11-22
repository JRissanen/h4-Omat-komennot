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
print("Hello!")
greeting = "Hello!"
for i in greeting:
  if i == "H":
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






























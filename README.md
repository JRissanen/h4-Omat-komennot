# h4-Omat-komennot
This is a repository for Configuration Management Systems course exercise 4

__a) Hei komento! Tee järjestelmään uusi "hei maailma" -komento ja asenna se orjille Saltilla. Liitä raporttiisi orjan 'ls -l /usr/local/bin/' tulosteesta ainakin se rivi, jolla näkyy uuden komentotiedostosi oikeudet.__

Ensin tein uuden kansion komennolla: `/srv/salt/helloworld` ja sinne kansion "Scripts", jonne laitoin scriptit.</br>
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


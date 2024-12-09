# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 08/12/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 08/12/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm


## x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)


€ Schneier 2015: Applied Cryptography, 20ed: Chapter 1: Foundations: https://learning.oreilly.com/library/view/applied-cryptography-protocols/9781119096726/08_chap01.html#chap01-sec001

### 1.1 Terminology ("Historical Terms" loppuun)

- Salakirjoitus on viesti joka on muunnettu lukemattomaan muotoon
- Message - Plaintext (cleartext)
- Disguising message - Encryption
- Encrypted message - Ciphertext
- Ciphertext to plaintext - Decryption

- Autehtication, Integrity, Nonrepudiation - Vastaanottajan pitää pystyä varmentamaan lähde sekä se, ettei viestiä ole muuteltu. Ei pitäisi myöskään olla mahdollista kieltää viestin lähettämistä myöhemmin.

- Cryptographic algorithm (cipher) - matemaattinen funktio encryptaamiseen sekä decryptaamiseen

- M = Message, (P = Plaintext) C = Ciphertext D = Decryption E = Encryption k = avain

> E(M) = C -> Encryptattu viesti on siis sama, kuin Decryptattu viesti

> D(C) = M -> Decrypt-funktio C:lle tuottaa viestin

> D(E(M)) = M -> Funktio palauttaa alkuperäisen viestin
Avaimella

> Ek(M) = C -> Lisättiin avain yhtälöön

> Dk(C) = M 

> Dk(Ek(M)) = M

- Käytännössä siis algoritmien turvallisuus liittyy avaimiin. Ei ole väliä, vaikka joku tietää alrgoritmin - jos ei tiedä avainta, ei voi lukea viestiä

Symmetrinen salaus:
- Käyttää usein samaa avainta sekä salauksen, että purkamiseen. Jos tiedät, miten viesti salattiin, tiedät - miten se puretaan.
- "Salaisen avaimen menetelmä", koska avain on pidettävä salassa molemmilla osapuolilla.
- Haasteena avaimen turvallinen välitys vastaanottajalle

Public key - Algorithms
- Salaukseen käytetty avain ei ole sama, kuin purkuavain
- Ei voi helposti/nopeasti laskea salausavaimesta
- Salausavain voidaan tehdä julkiseksi - kuka tahansa voi salata viestin, !vain tietty henkilö vastaavalla purkuavaimella voi purkaa viestin!
- Salausavain = Public key
- Purkuavain = Private key

Hyökkäystyypit:
- Ciphertext-only attack - "Salatekstin hyökkäys" - Vain salattu viesti. Yrittää päätellä alkuperäisen viestin tai avaimen vain salatun tekstin perusteella
- Know-plaintext attack - "Tunnetun selkokielen hyökkkäys" - Salattu sekä vastaava alkuperäinen viesti. Voi löytää salausavaimen tai purkualgoritmin tätä kautta.
- Chosen-plaintext attack - "Valitun selkokielen hyökkäys" - Voi valita sen viestin, jonka salaa. Voi sitä kautta saada vihjeitä salausavaimesta.
- Adaptive-chosen-plaintext attack - "Adaptiivinen valitun selkokielen hyökkäys" - Voi muuttaa viestiä edellisen salauksen perusteella.
- Chosen-key attack - "Valitun avaimen hyökkäys" - Ei voi suoranaisesti valita avainta, mutta tietää kahden avaimen suhteen
- Rubber-hose cryptanalysis - "Pakottaminen" - Voi uhkailla, käyttää väkivaltaa, lahjoa jotain avaimen saamiseksi

Ainoa tunnettu esimerkki täysin turvallisesta salausmentelmästä on kertakäyttöinen *salakirjoitus*
Useimmat käytettävät salausmenetelmät perustuvat Laskennalliseen turvallisuuteen. Sen murtaminen vie niin paljon laskentavoimaa ja aikaa, ettei se ole käytännössä mahdollista. Tietysti - mitä voimakkaammat välineet, sen helpompi murtaa. Eli tulevaisuudessa näidenkin pitää kehittyä.
Brute-force-attack - Kokeillaan kaikkia mahdollisia avaimia, kunnes löydetään oikea. Tehoton pitkien avainten kanssa, koska avainten määrä kasvaa avaimen pituuden kanssa.

  
### 1.4 Simple XOR
0 + 1 = 1 
1 + 0 = 1
1 + 1 = 0
a + a = 0
a + b + b = a

- Viimeinen lause oli hyvin kuvastava: "Saattaa pitää pikkusiskosi lukemasta tiedostojasi, mutta ei pysäytä kryptoanalyytikkoa muutamaa minuuttia kauempaa"
- XOR on heikko, koska käytännössä sen selvittäminen hoituu pelkästään laskemilla. Pelkästään englannin kielen ominaisuudet ja niiden toistuvat kuviot auttaa murtamaan salauksen.

### 1.7 Large Numbers
- Tässä oikeestaan vain laitetaan lukuja perspektiiviin
- Kuten luvussa 1.1 "Security of Algorithms" mainitaan - Jos algorithmin monimutkaisuus on: 2^128, tarvitaan 2^128 operaatiota sen murtamiseen. Vaikka kone tekisi miljoona operaatiota joka sekuntti, se vie 10^19 vuotta ja todennäköisyys on 3 - kertainen U.S loton pääpotin voittamisen.


### Karvinen 2024: Python Basics for Hackers https://terokarvinen.com/python-for-hackers/
Saa käyntiin ```python3```
- Helppo tehdä laskuja, esim 2*2
- Koodia voi suoraan testata painamalla F5 ```micro --plugin install runit```
- Pyyttonilla voi suoraan muuttaa numerot eri muotoihin, esim ```ord("T")```, joka palauttaa arvon 84 ja ```chr(84)``` joka palauttaa 'T'. Hexan voi myös muuntaa suoraan ```hex(84)``` --> '0x54' *0 tarkoittaa hexaa, ei liity käytännössä numeroon millään tavalla*. Binääri ```bin(84)``` -> '0b1010100'. Octal ```oct(84) -> '0o124'
- f-string -> Pistää f ennen stringiä, se tulostaa kaikki näiden sisältä {}

### Vapaaehtoinen: Karvinen 2024: Get Started Micro Editor
### Vapaaehtoinen: Karvinen 2024: Getting Started with Cryptopals using Python
Mutta ei tietenkään niitä "click to expand" alle piilotettuja vinkkejä, niitä kannattaa katsoa vain tarvittaessa. Osa ei tarvitse niitä ollenkaan.


## Ratkaise CryptoPals Set 1 -haasteet. Tehtävät saa ratkaista millä vain ohjelmointikielellä ja käyttää mitä tahansa tekstieditoria tai IDE:ä. Tehtäviä ei kannata ratkaista tekoälyllä, koska se vain kopioi malliratkaisun suoraan koulutusmateriaalistaan.

### a) 1. Convert hex to base64.
Tässä pitää siis saada ```49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d``` muunnettua hexasta base64 - muotoon. Lopputulos pitäisi olla: ```SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t```





Tuli ensimmäisenä mieleen suoraa vaihtaa se yhdellä kauniilla napin painalluksella, koska netissä on näitä converttereitä pullollaan.. Mutta olisi varmaan hyödyllisempää osata tämä itse. (Käytin)[https://www.atatus.com/tools/hex-to-base64]
![image](https://github.com/user-attachments/assets/3fc6408d-30a9-464c-b7f0-a449cc4c9871)


Jouduin käytännössä opettelemaan koko hoidon alusta alkaen, lueskelin Teron materiaaleja ja muita lisää. Päätin kokeilla ```base64.b64decode("49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d")```
![image](https://github.com/user-attachments/assets/2d653e96-abc1-46bb-ba03-21ef4c49705a)

Eli siis muunnos on tehty, mutta käsittääkseni tuo base64 muunsi sen vasta alkuperäiseen binäärimuotoon. Tein vielä crypro1.py - tiedoston ja se näyttää nyt tältä: 

![image](https://github.com/user-attachments/assets/aef197df-8bd1-425e-8979-8553ec00f0eb)


- Hakkasin tässä hetken päätä seinään, mutten halunnut täysin kopioida vastausta mistään. Törmäsin lopulta ```xxd``` ohjelmaan, jota käytetään datan muuntamiseen heksadesimaaliseen ja tekstimuotoon, sekä takaisin.
- Eli asentelin ensin ```sudo apt-get install xxd``` ja sen jälkeen käytin komentoa: ```echo "49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d" |xxd -r -ps |base64```
- ![image](https://github.com/user-attachments/assets/d1661a1f-176b-4c34-88d1-ace38e2ef2bc)
- Ja se palautti luvun
- Nyt varmistan vielä, että ne oikeasti on samat. Koska ihmisen silmä ei välttämättä kaikkea erota, tein seuraavan koodin pyyttonia hyödyntäen, joka vertailee näitä keskenään. Jos saatu luku ja alkuperäinen täsmää, se palauttaa "On sama luku" ja jos ne eivät täsmää, se palauttaa "Tee uudestaan!". Tätä olen ehtinyt opetella tässä samalla: 

![image](https://github.com/user-attachments/assets/76bf24f8-fe83-44a1-8426-55fbb225052e)

- Microeditorista, kun painaa F5, ohjelma ajetaan suoraa ja kuten näkyy - On sama luku

![image](https://github.com/user-attachments/assets/18fb0cf8-3a12-4793-bd25-572ac9562903)

Tässä vielä hetken aikaa ihmettelin, ettei tuo -n - komento tee mitään. Kokeilin sitä base64 ja eri variaatioita, mut sit lopulta otin base64 sieltä perästä pois ja löytyihän sieltä myrkkysieniä:

![image](https://github.com/user-attachments/assets/2af9ce05-043d-4fb8-8bf1-6c98e972d2de)


## b) 2. Fixed XOR.

- Lähdin samalla tavalla liikkeelle tässä tehtävässä, eli syötin ```echo 1c0111001f010100061a024b53535009181c |xxd -r -ps |base64```
- Se palautti ```HAERAB8BAQAGGgJLU1NQCRgc```

![image](https://github.com/user-attachments/assets/981ece68-4431-40b4-882a-05ddda4aafc6)

Olin testannut niin montaa eri variaatiota tuossa aikaisemmassa tehtävässä, että lähdin kokeilemaan suoraa sen ratkaisua. En kuitenkaan yhtään ymmärtäny sitä, miten se XOR - operaatio tehdään koodina, koska ei tosiaan ohjelmoinnista vieläkään oo muuta tietoo, kun Pythonin ihan perusteet.
Rakensin siis tän näkösen litanian: 

- Eli ymmärsin, että tarvii taas tehdä base64 juttuja, jotka skippasin tossa aikaisemmassa tehtävässsä. En kuitenkaan ymmärtäny sitä koodia, mikä tarvitaan tohon XOR - operaatioon, joka muuttaa tavuiksi, joten jouduin konsultoimaan ChatGPT:tä avuksi.
- Lopulta koodi näytti tältä:

![image](https://github.com/user-attachments/assets/9f70ad14-a70b-4af8-87b2-e156b8a5f877)

> bytes(a ^ b for a, b in zip(bytes1, bytes2)): Tämä luo uuden tavujonon, jossa jokaiselle tavulle suoritetaan XOR-operaatio a (bytes1) ja b (bytes2) välillä. - ChatGPT

![image](https://github.com/user-attachments/assets/a347aea1-2d36-453e-b10d-59478f7b2877)

- Tein vielä tarkistukset tuohon ja näyttää, ettei se tulosta jokaista kirjainta. Tulosteesta puuttuu 5

![image](https://github.com/user-attachments/assets/97d69b03-1edf-4b72-a112-41400017bbfa)

- En tätä lähde enempää selvittämään, koska ei siihen taitoja ole ja copy-paste on mielestäni aika turha ratkaisu


## Lähteet
Altering-File-To-Hex, Jadhusan24, Luettavissa:
https://github.com/Jadhusan24/Python-Hex-Dump

xxd Command in Linux, Last Updated : 26 Apr, 2024, Luettavissa:
https://www.geeksforgeeks.org/xxd-command-in-linux/

Python Tips and Tricks: Base64 String Encoding and Decoding, MathByte Academ, Luettavissa:
https://www.youtube.com/watch?v=mxwvvMZaIvU

XXD Command in Linux: A Must-Know for System Administrators, By Pulkit Jain, Oct 7, 2024
https://www.simplilearn.com/xxd-command-in-linux-article



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


Karvinen 2024: Python Basics for Hackers
Vapaaehtoinen: Karvinen 2024: Get Started Micro Editor
Vapaaehtoinen: Karvinen 2024: Getting Started with Cryptopals using Python
Mutta ei tietenkään niitä "click to expand" alle piilotettuja vinkkejä, niitä kannattaa katsoa vain tarvittaessa. Osa ei tarvitse niitä ollenkaan.

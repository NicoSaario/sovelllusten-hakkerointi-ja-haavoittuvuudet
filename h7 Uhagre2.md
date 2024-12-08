# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 08/12/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 08/12/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm


## x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)


€ Schneier 2015: Applied Cryptography, 20ed: Chapter 1: Foundations: https://learning.oreilly.com/library/view/applied-cryptography-protocols/9781119096726/08_chap01.html#chap01-sec001

1.1 Terminology ("Historical Terms" loppuun)

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

- 

1.4 Simple XOR
1.7 Large Numbers
Karvinen 2024: Python Basics for Hackers
Vapaaehtoinen: Karvinen 2024: Get Started Micro Editor
Vapaaehtoinen: Karvinen 2024: Getting Started with Cryptopals using Python
Mutta ei tietenkään niitä "click to expand" alle piilotettuja vinkkejä, niitä kannattaa katsoa vain tarvittaessa. Osa ei tarvitse niitä ollenkaan.

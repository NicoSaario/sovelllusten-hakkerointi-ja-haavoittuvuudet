# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 3/11/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 11/11/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm


# a) Strings. Lataa ezbin-challenges.zip Aja 'passtr'. Selvitä oikea salasana 'strings' avulla. Selvitä myös lippu. (Ensisijaisesti katsomatta sorsia, jos osaat.)
Aika simppeli homma - aloitin lataamalla tuon tiedoston virtuaalikoneelle, ajoin 'passtr' - ```./passtr```

<img width="477" alt="image" src="https://github.com/user-attachments/assets/316209c9-7513-4d06-9ef4-dc59900c92c2">

Sen jälkeen simppeli komento ```strings passtr```, joka näyttää tekstin ymmärrettävällä tavalla tiedostosta. Otetaan esimerkkinä ```cat ./passtr```. Kyllähän tästä pystyy salasanan etsimään, mutta strings tekee koko tiedostosta helpommin luettavan ja sen pystyy putkittamaan käyttämällä esim. |less, jolloin pystyy helpommin selaamaan

<img width="566" alt="image" src="https://github.com/user-attachments/assets/bbe8ecf3-7bea-4967-9573-b463fd2864db">

Sit uudestaan ```./passtr```, salasana sisään ja sehän meni läpi!

<img width="539" alt="image" src="https://github.com/user-attachments/assets/82e42f63-c0a9-45b4-85a7-5d7d2b1b2844">

Verrataan vielä tuo näkyvä lippu tiedoston sisällön kanssa ihan varmuuden vuoksi, vaikka kaiken järjen mukaan pitäis olla samat:

FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1} /vastauksesta

FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1} /tiedostosta

Ja ne on identtisiä, joten lippu varmistettu ja löydetty

# b) Tee passtr.c -ohjelmasta uusi versio, jossa salasana ei näy suoraan sellaisenaan binääristä. Osoita testillä, että salasana ei näy. (Obfuskointi riittää.)
# c) Packd. Aja 'packd' paketista ezbin-challenges.zip. Mikä on salasana? Mikä on lippu? (Tämä tehtävä on hieman haastavampi. Kirjaa ylös kokeilemasi lähestymistavat ja keksimäsi hypoteesit. Toivottavasti pääset itse maaliin, mutta jos et, läpikävely paljastuu tunnilla...)
# d) Vapaaehtoinen bonus: Cryptopals. Crypto Challenge Set 1. Tätä voi tehdä useamman viikon bonuksena. Jos saat ratkaistua kohdat 1 .. "4. Detect single-character XOR", olet jo astunut salakirjoituksen maailmaan.

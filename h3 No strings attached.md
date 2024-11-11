<img width="634" alt="image" src="https://github.com/user-attachments/assets/d2d45773-9f4a-47f0-aa59-aa0c645963ef"># Kotitehtävät
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

Koska olen suht alkeissa vielä ohjelmoinnissa, selvittelin hetken aikaa Obfuskoinnin saloja ja totesin, etten vielä tähän sitä osaa suoraa implementoida. 
Käytin siis ChatGPTä hyödyksi ja aloitin kyselemällä lisää siitä, miten se tapahtuu ja vähän jopa ehkä epäsuorasti viittasin suoraa tehtävään antamatta kuitenkaan koko koodia sille: "Miten tekisin sen, jos salasana pitäisi olla asetettu valmiiksi, esimerkiksi 'hattu123'".

![image](https://github.com/user-attachments/assets/c67d6d92-5c50-48e6-9960-2a61a1607387)

Ymmärsin siis, että tässä käytännössä tehdään vain lähdekoodiin piiloitettu kokonaisuus, jolla ASCII-arvoilla koko salasana näkyy vain ASCII-arvoina. Nuo part1, 2, 3 yhdistetään yhteen vasta kun ohjelma ajetaan ja 104, 97, 116 arvot vastaa kirjaimia 'hat'.
Tämän jälkeen tutkailin lisää koodia ja sitä, mitä pitäisi muuttaa. Käytännössä siis ainoastaan lisätään muutama johta ja muutetaan vähän tuota input - kohtaa ja kaikki pitäisi toimia.

Jatkoin Teron vanhalla salasanalla "sala-hakkeri-321" ja koska en tätä ainakaan ulkoa ole opetellut, käytin Text to ASCII - palvelua https://codebeautify.org/text-to-ascii muuntamaan tuon salasanan ASCII - muotoon.



<img width="552" alt="image" src="https://github.com/user-attachments/assets/a2b63259-b0cb-41b1-8a7e-86ae6badadb5">


![image](https://github.com/user-attachments/assets/4f4576b4-f592-427d-85df-7aeda95f820a)

Pikku päivitys tähän väliin: Hakkasin päätä seinään noin 2h tän homman parissa yhdessä tekoälyjen ChatGPT ja Copilot kanssa. Olis ehkä voinu olla nopeempaa yrittää opetella tätä kunnolla, mutta näytti aluks sen verran heprealta nuo tekoälyn luomat koodit, että ajattelin sen vievän kymmenen kertaa enemmän aikaa jos itse selvittäisin.

Nyt kuitenkin pitkän pitkän sanaväännön jälkeen päästiin lopputulokseen, jossa salasanaa ei näy. Voi olla, että jo ensimmäiset testaukset olisivat toimineet (pelkkä ASCII - metodi), mutta jouduin lopulta kysymään, että tarviiko tehdä jokin päivitys tms. ja vastauksena tuli komento ```gcc -o passtr passtr.c``` sekä tietysti passtr ajaminen ```./passtr```.

<img width="634" alt="image" src="https://github.com/user-attachments/assets/be020119-9712-4472-9d49-685d4c4d38a5">

- Eli Copilot viimeisteli koodin lopulta näin: 

<img width="413" alt="image" src="https://github.com/user-attachments/assets/5b47cb1b-370d-4b93-a5d9-8f71c1cabe1c">

- Jossa XOR - salattiin, eli tallennettiin salasana kokonaislukuina salattuina merkkeinä ja salasana puretaan vain ajon aikana.
- Kyseenalaistan tässä kohtaa ehkä hieman sitä, miksi otin kurssin ennen ohjelmointiin syventymistä.. Jatketaan!

# c) Packd. Aja 'packd' paketista ezbin-challenges.zip. Mikä on salasana? Mikä on lippu? (Tämä tehtävä on hieman haastavampi. Kirjaa ylös kokeilemasi lähestymistavat ja keksimäsi hypoteesit. Toivottavasti pääset itse maaliin, mutta jos et, läpikävely paljastuu tunnilla...)
# d) Vapaaehtoinen bonus: Cryptopals. Crypto Challenge Set 1. Tätä voi tehdä useamman viikon bonuksena. Jos saat ratkaistua kohdat 1 .. "4. Detect single-character XOR", olet jo astunut salakirjoituksen maailmaan.

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

Sen jälkeen simppeli komento ```strings passtr```, joka näyttää tekstin ymmärrettävällä tavalla tiedostosta. Otetaan esimerkkinä ```cat passtr```. Kyllähän tästä pystyy salasanan etsimään, mutta strings tekee koko tiedostosta helpommin luettavan ja sen pystyy putkittamaan käyttämällä esim. |less, jolloin pystyy helpommin selaamaan

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

- Homma lähti näin käyntiin: 

<img width="451" alt="image" src="https://github.com/user-attachments/assets/788031af-398b-4853-934e-74c221f8a061">

Tutkiskelin aikani ```strings packd``` ja kokeilin muutamaa salasanaa joukosta, jolloin huomasin, ettei ne toimi. Törmäsin aikaisemman tehtävän tiimoilta termiin UPX ja siihen, että sillä pystytään peittämään merkkijonoja. Noin 5 - minuutin tarkastelun jälkeen huomasin tämän:

<img width="488" alt="image" src="https://github.com/user-attachments/assets/183b9b92-2c80-4111-a948-ddca4595d3b7">

Ajattelin, että tässä on pakko olla joku juju ja ehkä ratkaisu piilee siinä. Latasin UPX - täältä: https://github.com/upx/upx
Unzippasin ja katselin upx-doc.txt - tiedostoa. Kaikessa yksinkertaisuudessaan se toimii näin:

<img width="321" alt="image" src="https://github.com/user-attachments/assets/2375cb47-8759-4676-ac64-f7074d006ac1">

Samalla aloin selvittää, että miten pystyn avaamaan tuon paketin, jolloin selvisi samasta dokumentista, että "kaikki tiedostoformaatit pystytään avaamaan komennolla ```upx -d yourfile.exe```.

<img width="478" alt="image" src="https://github.com/user-attachments/assets/2af76ac3-33b9-484c-9791-52884171fd37">

- Kokeillaan sitä siis seuraavaksi:
- Kokeilin ja saatoin ehkä jäädä jumiin, koska tuli virheilmoitus 

<img width="424" alt="image" src="https://github.com/user-attachments/assets/bcdb25ae-6229-4f4d-84a2-589c111955ce">

- Rupesin sit noin 15 minuutin selailun jälkeen miettimään ja kokeileen eri ratkaisuja ihan näpyttelemällä kaikkee, mitä tuli mieleen - katoin myös doc taas
- Nyt tajusin tarkistaa latauspaikan ja sen, mitä tuli ajatuksissaan tehtyä - latasin siis uudestaan koko setin https://upx.github.io/ täältä ja 
- Samalla sit jouduin Googlettamaan et miten tar - tiedoston saa purettua :D
- Sieltähän löyty komento ```tar -xf (tähän tiedosto)```
- Sivuhuomiona tähän liittyen, et on kyl viimeaikoina tää Linuxin käyttö tullu tutummaks ja tutummaks, ni on näistä tehtävistä kyl paljon hyötyä siihenkin
- Nyt sitten ihmettelin ja mietin, pohdin ja ihmettelin tätä:
-  ** Maaliin pääsyn jälkeinen huomio: Ihmettelin tuota aikaisemmin sen takia, ettei missään ollut upx execute - tiedostoa. Ei siis käynyt mieleenkään, että voin käyttää pelkkää ./upx ... - komentoa. Kuitenkin heti, kun sain oikean tiedoston ladattua koneelle, se tuli ihan automaatiolla enkä muistanut enää jääneeni koko kohtaan jumiin

<img width="590" alt="image" src="https://github.com/user-attachments/assets/00e479c0-f0a3-4fe1-a751-d47dd582e109">

- Aikani pohdittua (joku aboutkerrallaan 34 minuuttia), ajattelin että nyt loppuu se leikkiminen ja siirsin upx execute - filen suoraan packd - kansioon
- Kappas keppanaa:

 <img width="590" alt="image" src="https://github.com/user-attachments/assets/f596ff8d-3cea-4c47-a4f8-ad5d5945a818">

- Vielä ei kuitenkaan olla maalissa, vaikka juhlia voikin.
- Aluks en kyllä uskonu pääseväni tähän tilanteeseen ja tehtävä näytti jotenkin vähän absurdilta ilman sen suurempaa pohjaa koko asiaan. Kun palasia yhdistelee toisiinsa ja usein kokeilee kaikennäköistä, päästään niillä joskus maaliin. Nyt voi vähän juhlia jo lisää

<img width="614" alt="image" src="https://github.com/user-attachments/assets/8ee51604-b01b-4df5-aa78-47840a7f0ec3">

- Eli sama ```strings packd``` - komento tuon purun jälkeen ja siellähän salasana kiiltää koko komeudessaan
- Samalla se aikaisemmin mainitsemani tehtävän pelastaja "This file is packed with UPX ..." on kadonnut kokonaan!
- Testataan vielä nyt varmuuden vuoksi, että kaikki toimii 

<img width="551" alt="image" src="https://github.com/user-attachments/assets/3c2fc4d3-4f94-4071-b1ca-58e6806a98a3">


### Miten meni?

- Opin aika paljon kaikkea - tiedostot pitävät usein sisällään kaiken näköistä tietoa, jota ei välttämättä huomaa. On kiinnitettävä pieniin yksityiskohtiin enemmän huomiota ja pelkällä yhdellä tiedolla voi saada kaikki loksahtamaan paikalleen. Opin myös sen, että pitäisi vihdosta viimein vaihtaa Linuxille kokonaan ja jatkaa sen parissa lisää työskentelyä, jotta ei pienissäkin asioissa tarvitsisi aina seuloa koko internettiä läpi ja asiat pysyisivät paremmin muistissa. 

- Kyseenalaistan hieman valintaani ottaa kurssi tässä vaiheessa, kun tuo ohjelmistokehitys on vähän heikosti hallussa, mutta kaikki tehtävät tähän liittyen ja luettu materiaali on alkanut laittamaan motivaatiota sen opiskelun suuntaan - ehkä hyvä?

- Pienistäkin voitoista pitää osata nauttia!

# d) Vapaaehtoinen bonus: Cryptopals. Crypto Challenge Set 1. Tätä voi tehdä useamman viikon bonuksena. Jos saat ratkaistua kohdat 1 .. "4. Detect single-character XOR", olet jo astunut salakirjoituksen maailmaan.

Palataan asiaan.



## Lähteet:
What is Code Obfuscation? How to Disguise Your Code to Make it More Secure, freecodecamp (2020), Luettavissa:
https://www.freecodecamp.org/news/make-your-code-secure-with-obfuscation/ (Luettu 11/11/2024)

How to extract tar file on Linux (2021), Korbin Brown, Luettavissa: https://linuxconfig.org/how-to-extract-tar-file-on-linux (Luettu 11/11/2024)


How to Use the strings Command on Linux,  Dave McKay, 2019, Luettavissa: https://www.howtogeek.com/427805/how-to-use-the-strings-command-on-linux/ (Luettu 11/11/2024)


Kotitehtävät: Sovellusten hakkerointi ja haavoittuvuudet ICI012AS3A-3001, Tero Karvinen, https://terokarvinen.com/application-hacking/#laksyt (Luettu 11/11/2024)

ChatGPT ja Copilot - heille erityiskiitos tehtävässä b

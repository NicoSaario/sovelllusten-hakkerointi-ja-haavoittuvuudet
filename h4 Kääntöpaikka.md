# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 17/11/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 17/11/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm


# x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

1) Hammond 2022: Ghidra for Reverse Engineering (PicoCTF 2022 #42 'bbbloat') (Video, noin 20 min) https://www.youtube.com/watch?v=oTD_ki86c9I


- Ghidra on free open source reverse engineering tool - NSAn kehittämä
- Kyseinen video on tarkoitettu opastamaan Ghidran käyttöä ja opettamista hyödynnetään tekemällä lipunryöstö
- Sitä ennen on hyvä testata aina helpot ja nopeat ratkaisut, joilla voi löytyä lisätietoa, esim ```ltrace``` ```strace```
- Video on kaiken kaikkiaan hyvä. Siinä selitetään hieman sitä, miten Ghidra toimii ja miten kannattaa lähestyä tulevaa tehtävää ja pyrkiä ratkaisuun


2) Vapaaehtoinen: € Eagle and Nancy 2020: The Ghidra Book: 2. Reversing And Disassembly Tools (Usein suositeltu kirja Ghidrasta) https://www.oreilly.com/library/view/the-ghidra-book/9781098125684/xhtml/ch02.xhtml#ch02lev29

- Lueskelen ja päivittelen tätä tehtäviä tehdessä


# a) Asenna Ghidra.

Latailin sen oppitunnilta valmiiksi täältä https://github.com/NationalSecurityAgency/ghidra/releases/tag/Ghidra_11.2.1_build

- Lyhyt ja ytimekäs 'Assets' - kohdan alapuolella Zip - tiedosto. Napauta - Lataa - Tutki - Unzip - Tutki lisää

- b) Kohdassa on laajempi kuvaus asentelun jälkeisistä toimista

# b) rever-C. Käänteismallinna packd-binääri C-kielelle Ghidralla. Etsi pääohjelma. Anna muuttujielle kuvaavat nimet. Selitä ohjelman toiminta. Ratkaise tehtävä binääristä, ilman alkuperäistä lähdekoodia. ezbin-challenges.zip

<img width="599" alt="image" src="https://github.com/user-attachments/assets/88f2570b-e61a-4ef3-b307-0feb4f55e1c7">

Kuten videossakin mainittiin, joudutaan lataamaan Java JDK - työkalu manuaalisesti, jotta Ghidra toimii.

Unzippasin ghidra - tiedoston ja yritin lyödä Ghidran päälle ajamalla sitä ```./ghidraRun```- komennolla.

<img width="770" alt="image" src="https://github.com/user-attachments/assets/e2921b49-99ad-46ec-bbf2-febc8e06aac1">

Eli versio on väärä. Tämä myös mainittiin Teron antamissa ohjeissa: "Debian 12-Bookworm (ulkomuistista)
```sudo apt-get install openjdk-17-jdk```
Ota Ghidran versio, joka toimii tällä versiolla Java 17. Muistaakseni Ghidra 11.1.2. Se on vieläpä aika uusi."

Ajattelin kuitenkin testata kepillä jäätä ja saada sen toimimaan tällä 11.2.1 - versiolla:
- Hain täältä: https://www.oracle.com/java/technologies/downloads/#java21 version 21, jota pyydettiin
- Latasin ja koska kyseessä .deb - tiedosto, komennolla ```dpkg -i <tiedosto tähän>``` asentelin sen kuntoon
- Ja sehän toimii!

<img width="884" alt="image" src="https://github.com/user-attachments/assets/5a08bacb-a542-48ec-a878-e6b8db650e0c">

Testasin myös samalla, olisiko tuo löytynyt videon ohjeilla ```apt-cache search jdk```
- Kokeilin jdk21, java21 ja jdk17+ jne, mutta ne eivät toimineet

<img width="199" alt="image" src="https://github.com/user-attachments/assets/d29680d9-e611-4f9b-b37c-74f8e40fc93e">

<img width="390" alt="image" src="https://github.com/user-attachments/assets/bb208a02-2488-450d-9106-bda2af90da12">

Löyty, testattu ja toimi myös noin. Jatkoa ajatellen nopeampi tapa löytää oikeat tiedostot (ja jopa turvallisemmin)


# b) rever-C. Käänteismallinna packd-binääri C-kielelle Ghidralla. Etsi pääohjelma. Anna muuttujielle kuvaavat nimet. Selitä ohjelman toiminta. Ratkaise tehtävä binääristä, ilman alkuperäistä lähdekoodia. ezbin-challenges.zip

- Drag'n'drop - menetelmällä viedään haluttu tiedosto Ghidraan
<img width="781" alt="image" src="https://github.com/user-attachments/assets/01d094a1-2824-4f4d-b665-1017773f333e">

<img width="470" alt="image" src="https://github.com/user-attachments/assets/a19e0f9b-446a-49f6-8206-9dac6080957f">

Sitten vain klikkailin ok - ja 'Analyze now' 

<img width="491" alt="image" src="https://github.com/user-attachments/assets/ca562075-a255-497a-8065-a2d55f1074d4">

Tähän kohtaan voisi lisäillä noita jos haluaisi, muttei se nyt ole tarpeellista.

--- Jatkan tästä myöhemmin
--- Jatkuu

Hetken aikaa ihmettelin, että miksen löydä koko pääfunktiota mistään ja koko koodi näyttää heprealta. Olin kuitenkin aikaisemmissa tehtävissä käyttänyt UPX - työkalua ja unohdin sen kokonaan. 

<img width="386" alt="image" src="https://github.com/user-attachments/assets/eb30d145-e004-4db3-8d01-5e91ffd54b85">

No nyt alkoi näkymään ja myös Ghidra tunnisti heti "main" - funktion tuon rimpsun sisältä. Tuli siis ilmoituslaatikko, jossa Ghidra ilmoitti löytäneensä main - funktion ja kysyi, hypätäänkö siihen samantien. Tottakai painoin kyllä ja pääsin heti perille.

<img width="632" alt="image" src="https://github.com/user-attachments/assets/0a1f1cb1-b57b-4dd3-b1d5-12d69199127f">

- Kuvassa näkyy myös tuo salasana suoraan "piilos-AnAnAs"
- Ainoat asiat, mitä osasin tuosta korjata hieman kieltä opiskelemalla olivat

  <img width="305" alt="image" src="https://github.com/user-attachments/assets/2ccc0a57-999a-417c-91a2-8f13e90c386d">

- Eli iVar1 ```is_password_correct```
- _isoc99_ näytti turhalta, joten otin kokonaan sen pois
- local_28 = ```is_password_correct```
- tuon "main" - Ghidra lisäsi itse, joten ainoastaan ehkä "undefined8" voisi ottaa pois?

Ohjelma käytännössä toimii niin, että se vertaa käyttäjän syöttämän rimpsun salasanaan ja jos se on oikein, lippu tulee näkyviin. Jos syöttö ei täsmää salasanan kansssa, se palauttaaa "Sorry, no bonus."

# c) Jos väärinpäin. Muokkaa passtr-ohjelman binääriä (ilman alkuperäistä lähdekoodia) niin, että se hyväksyy kaikki salasanat paitsi oikean. Osoita testein, että ohjelma toimii. ezbin-challenges.zip

Muistan Teron pitämältä oppitunnilta, että vaihtamalla kahden funktion kohtaa, voi saada tuon halutun lopputuloksen ja toimen liittyi muistaakseni JMP - kohtaan.
Looginen päättely johti siihen, että toisen on pakko olla JMP. Nopealla Googlettamisella löysin https://devcodef1.com/news/1181349/ida-jnz-vs-jmp ja varmistuin siitä, että ne liittyvät toisiinsa. Jump ja Jump if Not Zero.





# Lähteet: 
Hammond 2022: Ghidra for Reverse Engineering (PicoCTF 2022 #42 'bbbloat'), Katsottavissa:
https://www.youtube.com/watch?v=oTD_ki86c9I (katsottu 17/11/2024)

Eagle and Nancy 2020: The Ghidra Book: 2. Reversing And Disassembly Tools, Luettavissa:
https://www.oreilly.com/library/view/the-ghidra-book/9781098125684/xhtml/ch02.xhtml#ch02lev29 (luettu 17/11/2024)

https://askubuntu.com/questions/40779/how-do-i-install-a-deb-file-via-the-command-line

C Tutorial, W3Schools, Luettavissa: https://www.w3schools.com/c/index.php (luettu 18/11/2024)

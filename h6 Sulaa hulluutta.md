# h6 Sulaa hulluutta
Tämä tehtävänanto sisältää pääosin Lari Iso-Anttilan laatimia tehtäviä. https://github.com/lisoant

# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 02/12/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 02/12/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm

## a) Tutki tiedostoa h1.jpg jo opituilla työkaluilla. Mitä saat selville?

Aloitin tutkimaan sitä tarkemmin komennolla ```strings h1.jpg |less```ja kävin koko pitkän litanian läpi:
- Törmäsin tähän:  ![image](https://github.com/user-attachments/assets/e522102b-c29d-46aa-8429-39f4efc5a587)
- Varmaan siis ollut alunperin word - tiedosto ja naamioitu jpg - tiedostoksi. Omaa analyysiäni
- Törmäsin samaan formaattiin useasti ![image](https://github.com/user-attachments/assets/70c75830-db39-4f1f-b159-a42835e75935)
- ![image](https://github.com/user-attachments/assets/5a52a02c-3778-4ad9-90b4-bd267d0626be)
- Myös tämä kiinnitti huomion: ![image](https://github.com/user-attachments/assets/7db2e8f0-e7ab-4703-bfc5-ff3afe6c250b)

Yritin myös Ghidralla saada tiedoston auki, mutta se ei tunnistanut eri kielillä mitään.


## b) Tutki tiedostoa h1.jpg binwalk:lla. Mitä tietoja löydät nyt tiedostosta? Mitä työkalua käyttäisit tiedostojen erottamiseen? (Huomaa, että binwalk versio 2.x ja 3.x toimivat eri tavalla.)

- Jouduin ensin binwalkin asentelemaan ```sudo apt-get install binwalk```
- Version 2.3.4

![image](https://github.com/user-attachments/assets/3f3bb1b0-555e-4f19-aece-58eb0a2bb2e2)

- Binwalkilla löytyi JPEG, TIFF ja Zip - tiedostoja
- TIFF on siis Tag Image File Format - korkealaatuiset kuvat, jotka yleensä on suurempi tiedosto eikä menetä kuvanlaatua https://www.adobe.com/creativecloud/file-types/image/raster/tiff-file.html?msockid=2085557b83266da91351410e82a96cfc
- Selvittelin hetken aikaa siitä ihan mielenkiinnosta, koska tuo formaatti ei itselleni ole niin tuttu. JPEG hukkaa usein tietoa, mutta TIFF ei. Usein korkealaatuisiin/isoihin kuviin käytetään ensin TIFF - formaattia ja tämän jälkeen se lähetetään eteenpäin JPEG - formaatissa, joka pakkaa sen pienempään kokoon. Samaan aikaan on kuitenkin mahdollista (kuten tässä), että sinne sisään on "piilotettu" muutakin tietoa.


![image](https://github.com/user-attachments/assets/64b821c4-26ee-49bc-a55b-61a8dcfab93c)

- Sit piti vähän googletella, että miten nuo Zipit saa auki ja löysin https://www.thegeekdiary.com/binwalk-command-examples-in-linux/ esimerkkejä binwalkin käytöstä.
- Löyisin komennon, joka purkaa kaikki tiedostot kerralla ```binwalk -e <tiedosto tähän>```
- ![image](https://github.com/user-attachments/assets/fd347885-495e-42e4-a76b-21fbb83aa49e)
- Se purki siis tuon koko hela hoidon ja lähdetään tutkimaan sitä lisää:
- ![image](https://github.com/user-attachments/assets/ea432430-e1ba-477b-b1d5-1d4f1d59c68d)

- ![image](https://github.com/user-attachments/assets/ed192638-42b4-49ae-a78e-0b9565f8e9ce)

 - Taitaa kaikki mielenkiintoinen olla sen 'word' kansion sisällä, mutta löytyi sitä havaintoja muualtakin
 - Sieltä löyty document.xml
 - Tutkin sitä ```strings document.xml```
 - Löytyhän sitä luettavaa tekstiä vihdoin. En kuitenkaan saanut missään järkevässä muodossa sitä auki, joten tutkin muita kansioita
 - Yritin unzipata tuota 494FS.zip - tiedostoa, mutta se ilmoitti ettei tiedosto ehkä ole zipfile
- ![Näyttökuva 2024-12-02 205604](https://github.com/user-attachments/assets/5439b7e4-af98-4496-a588-9dd16efe331e)
 - 
 - Kurssi on opettanut ainakin tulkitsemaan asioita niin, ettei kaikki ole aina sitä, miltä näyttää
 - Käytin siis ```file 494FS.zip```- komentoa ja paljastui, että siellähän se word-tiedosto piileskelee
- ![Näyttökuva 2024-12-02 205509](https://github.com/user-attachments/assets/db6720d6-7da8-4078-91bd-9afc4832aa25)
- Oon nyt vähän liian syvällä tässä, mutta haluan saada sen luettavaan muotoon
- Kokeilin avata selaimella, microlla, silti ei tulosta
- Halusin saada sen jollain muulla, kuin Wordillä auki - sillä se luultavasti olisi näkynyt heti
- Nyt selvittelin hieman, miten docx - tiedoston saa markdowniksi
- https://stackoverflow.com/questions/16383237/how-can-doc-docx-files-be-converted-to-markdown-or-structured-text selvisi, että pandocilla saa suoraan ilman sen suurempia kiemuroita
- ```sudo apt-get install -y pandoc```
- ```pandoc -f docx -t markdown 494F5.zip -o 494F5.markdown```
- ```micro 494F5.markdown```
- ![image](https://github.com/user-attachments/assets/10ae4d70-2c4b-410b-a66f-1f7f3825ccdf)
- Sain, kuin sainkin sen auki ja nyt se on vähän helpommin luettavissa
- Kyseessä siis *50 Predictions for the Next 50 years*
- 

### c) FOSS (Free Android OpenSource). Tutustu Android-sovelluksiin Offan (2024) listalta: https://github.com/offa/android-foss Android FOSS. Valitse listalla itsellesi mielenkiintoisin applikaatio ja mene sen GitHubiin. Lataa ohjelman APK itsellesi ja käytä seuraavia työkaluja tutustuaksesi, miten APK:n voi avata.

Työkalut:
1. ZIP
2. [JADX](https://github.com/skylot/jadx)
3. [Bytecode-viewer](https://github.com/Konloch/bytecode-viewer/)

Valitsin [ProtonVPN](https://github.com/ProtonVPN/android-app), koska käytän sitä jatkuvasti arjessa.
 - Latasin APK:n täältä https://f-droid.org/packages/ch.protonvpn.android/ ja valitsin uusimman paketin
- 

1. ZIP
- Latasin tosiaan APK - paketin ja käytin ```unzip```ja paketin nimi
- Tein kansion ```mkdir protonvpn```ja extractasin sen sinne käyttämällä graafista käyttöliittymää, sillä hukkasin hetkeksi koko unzipatun paketin
  ![image](https://github.com/user-attachments/assets/6209a9e7-ee84-45e3-8904-0410d5e44905)
- Nyt pääsee tutkailemaan koko sisältöä


2. JADX 
- Latasin sen täältä: https://github.com/skylot/jadx/releases/tag/v1.5.1
- Unzippasin haluttuun paikkaan ```mkdir jadx``` ```unzip jadx-1.5.1.zip -d /home/nicos/Downloads/jadx```
- Avasin gui-liittymällä ```./jadx-gui```. Tää koko käyttö on ensimmäistä kertaa, joten tehdään ja mietitään samalla

![image](https://github.com/user-attachments/assets/564a8185-0eb6-47fc-88af-a91c26f27912)

-> Open file -> ProtonVPN - unzipattu tiedosto - Latailee (meni noin 60s)
- Avautu tämmöne näkymä

![image](https://github.com/user-attachments/assets/6757391e-8edf-4fbf-89a4-8520ae7d1dea)

- Voi käytännössä tutkia mitä tahansa kansioo ja sen sisältöä
- Esimerkkinä hain Area - codesista Suomen

![image](https://github.com/user-attachments/assets/11702751-8cd8-49e0-8366-03e288061902)

- Kätevä Gui. Helppokäyttöinen, helposti luettava ja kaikki muutaman klikkauksen päässä!


3. Bytecode-viewer
- Sen latasin täältä: https://github.com/konloch/bytecode-viewer/releases 2.12.jar - tiedosto
- Githubin ohjeiden mukaan käytin komentoa: ```java -jar Bytecode-Viewer-2.12.jar```
- Tuli seuraava näkymä:
![image](https://github.com/user-attachments/assets/233db59d-b770-43bb-8a3e-a835c2db91e6)
- Sit voi käyttää "File->Add" ja hakea tiedosto, joka haluttiin
- Hain saman tiedoston jälleen ja takaana alkoi koodi raksuttaa

![image](https://github.com/user-attachments/assets/f31f5882-655e-40b3-814b-b09e68b00701)



## Lähteet:

- Can a jpeg contain tiff image data?, GraphicDesign, Luettavissa: https://graphicdesign.stackexchange.com/questions/109705/can-a-jpeg-contain-tiff-image-data (luettu 02/12/2024)
- What is a TIFF file?, Adobe, Luettavissa: https://www.adobe.com/creativecloud/file-types/image/raster/tiff-file.html?msockid=2085557b83266da91351410e82a96cfc (luettu 02/12/2024)
- binwalk Command Examples in Linux, by admin, Luettavissa: https://www.thegeekdiary.com/binwalk-command-examples-in-linux/ (luettu 02/12/2024)
- How can doc/docx files be converted to markdown or structured text?, Modified 4 months ago, Luettavissa: https://stackoverflow.com/questions/16383237/how-can-doc-docx-files-be-converted-to-markdown-or-structured-text (luettu 02/12/2024)

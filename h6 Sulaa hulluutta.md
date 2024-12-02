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





## Lähteet:

- Can a jpeg contain tiff image data?, GraphicDesign, Luettavissa: https://graphicdesign.stackexchange.com/questions/109705/can-a-jpeg-contain-tiff-image-data (luettu 02/12/2024)
- What is a TIFF file?, Adobe, Luettavissa: https://www.adobe.com/creativecloud/file-types/image/raster/tiff-file.html?msockid=2085557b83266da91351410e82a96cfc (luettu 02/12/2024)

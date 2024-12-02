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

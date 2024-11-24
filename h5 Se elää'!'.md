# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 24/11/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 24/11/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm


## a) Lab1. Tutkiminen mikä on ohjelmassa vialla ja miten se korjataan. lab1.zip

Aloitin tehtävän tekemällä vanhalla tutulla ```strings``` - komennolla. Tällä kertaa sieltä ei kuitenkaan löytynyt muuta mielenkiintoista, kuin nämä good ja bad - messaget: <img width="194" alt="image" src="https://github.com/user-attachments/assets/14bdbecd-2f0e-4ddd-ab58-f774cb1fb911">

- Käännetään koodi siis ajettavaan muotoon jaettujen kalvojen ohjeiden mukaan, eli ```gbd -o main gbd_example1.c -g```:

<img width="329" alt="image" src="https://github.com/user-attachments/assets/fc96923f-abfa-4781-ac85-3d6d5771a60f">

- Nähdään, että sinne ilmestyi tuo "main" -  <img width="174" alt="image" src="https://github.com/user-attachments/assets/52e6cc3d-80c8-47d5-b1da-57f51acc5f41">

- Ajetaan se debuggerilla ```gbd ./main```
- ```run```- komennolla katsotaan, mitä se tekee <img width="233" alt="image" src="https://github.com/user-attachments/assets/eb556956-1bf1-4396-b5a6-9a5f39822f41">

- Jouduin hieman opettelemaan tän koko homman käyttöä, koska jäi ensimmäinen tunti oppitunnista väliin
- Löysin kuitenkin hyvän tutoriaalin aiheeseen liittyen, jossa avataan vähän enemmän sitä, miten eri näkymiä ja muita saa oikeasti selville https://www.youtube.com/watch?v=Dq8l1_-QgAc

- Lisäsin breakpointin main - funktioon ```breakpoint main``` ja hakkasin ```nexti```ä niin kauan, että tapahtui jotain. Ensin tuli tuo "Khoor/#zruog1" ja vähän aikaa naputtelemalla tuli Segmentation fault, joka oli myös aikaisemmissa kuvankaappauksissa. Ongelma oli siis kuvassa näkyvien valkoisten viivojen välillä.

- Hetken ihmettelin, miksei mikään näkymä kerro mitään ja katsoin videon uudelleen, joten käytin ```lay next```, jolloin myös koodi tuli näkyviin
- Tein saman homman, eli naputtelin nextiä hetken aikaa ja tuli seuraava näkymä

<img width="344" alt="image" src="https://github.com/user-attachments/assets/b8882e24-4f57-4e30-ba1b-9ca91f4ad20c">

- Ongelma löytyy siis tuosta kuvassa näkyvästä kohdasta

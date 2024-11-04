# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 3/11/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 3/11/2024 (pvm/kk/v)

Oracle VM VirtualBox ja Debian 12 Bookworm

## x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
OWASP: OWASP Top 10: A01 Broken Access Control https://owasp.org/Top10/A01_2021-Broken_Access_Control/

- Pääsynhallinnan tarkoituksena on rajoittaa käyttäjien toimet vain niille oikeuksille, jotka on myönnetty
- Väärinkäyttö ja haavoittuvuudet voivat johtaa esimerkiksi tietojen paljastumiseen, muuttumiseen, tuhoutumiseen tai johonkin muuhun liiketoiminnalle haitalliseen

Haavoittuvuuksia ovat mm:

1. Pääsy on myönnetty liian monille ja se altistaa tiedot
2. URL-osoitteiden muokkaaminen tai erilaisten työkalujen käyttäminen pääsyn saamiseksi - esim. muokkaamalla HTML-sivua
3. Toisten käyttäjien tileille pääsy omia tunnuksiaan hyödyntäen
4. Puuttuvat pääsynhallinnat API-pyynnöissä (POST, PUT, DELETE)
5. Hyökkääjä voi toimia pääkäyttäjänä tai muuna käyttäjänä ilman valtuutuksia
6. Voi muuttaa JWT-tunnuksia (JSON Web Token) tai evästeitä saavuttaakseen korkeammat oikeudet
7. CORS virheellinen määrittely - sallii esim. "*" käytön
8. Sivujen pakottaminen siihen, että tavallinen käyttäjä voi päästä esimerkiksi adminin kirjautumispaneeliin. Tätä kautta mahdollistaa pääsyn toimintoihin, joihin ei pitäisi olla oikeuksia

Miten niitä ehkäistään?

1. Kaikki muut paitsi julkiset resurssit - kielletään oletuksena
2. Varmistetaan, ettei hyökkääjällä ole mahdollista muokata metadataa tai tarkastuksia
3. Keskittää mekanismit, jolloin pääsynhallinta toteutetaan kerralla ja käytetään uudelleen koko sovelluksessa
4. Valvonta - käyttäjillä pääsy vain omiin tietueihinsa
5. Hakemistoluetteloinnin poisto käytöstä
6. Virheiden kirjaaminen
7. Kun käyttäjä kirjautuu ulos, myös istuntotunnisteet poistuvat - käyttäjää ei enää tunnisteta eikä anneta pääsyä aikaisempiin sessioihin

Esimerkit: 
```https://example.com/app/accountInfo?acct=notmyacct```
Tuossa lopussa modifioitu sivuston URL =notmyacct (voi olla mikä tahansa tunnus). Jos ei ole tarkistettu oikein, hyökkääjä voi käyttää kenen tahansa käyttäjän tiliä

```
 https://example.com/app/getappInfo
 https://example.com/app/admin_getappInfo
```
Helppo testi, jos sovellus ei estä pääsyä esimerkiksi tuolle admin - sivulle, voi hyökkääjä käyttää URL-osoitetta hyväkseen.


Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/

PortSwigger: Access control vulnerabilities and privilege escalation https://portswigger.net/web-security/access-control

Pääsynvalvonta (Access Control) - lyhykäisyydessään rajoitetaan se, kuka tai mikä omaa oikeudet suorittaa toimenpiteitä tai päästä käsiksi resursseihin
- Authentication
* Varmistaa käyttäjän olevan se, kenen pitäisi olla
- Session Management
* Määrittää käyttäjän myöhemmät HTTP-pyynnöt
- Access control
* Määrittää, sallitaanko käyttäjän tehdä ne toimenpiteet, joita hän yrittää tehdä

!HOX HOX
Isoin riski koko operaatiossa on se, että ihmisten on tehtävä suunnittelupäätökset tämän ympärille, joten virhemarginaali on suuri

1. Vertical Access Controls
   - Mekanismi joka rajoittaa pääsyä arkaluontoisiin toimintoihin tietyille käyttäjätyypeille
   - Eri käyttäjillä on pääsy eri toimintoihin - Admin poistaa tai hallinnoi käyttäjiä, tavallinen user ei
   - Broken Access Control - Pääsee kiinni toimintoon, johon ei lupaa! Esim. Ylläpitosivu
     
 2. Horizontal Access Controls
   - Rajoittaa pääsyn resursseihin tietyille käyttäjille
   - Tietyillä käyttäjillä on pääsy vain osaan saman tyyppisistä resursseista - Admin voi tarkastella kaikien käyttäjien tekemiä töitä, user vain ommiaan
   - Broken Access Control - Käsiksi muiden resurrseihin - user1 näkee user2 kotiosoitteen ja maksutavan sekä pankkikortin tiedot tai vaikkapa jonkin suojatun päiväkirjan
 
3. Context-dependent access controls
   - Rajoittaa pääsyä toimintoihin ja resursseihin tilan tai käyttäjän vuorovaikutuksen perusteella
   - Estää käyttäjiä suorittamasta toimintoja väärässä järjestyksessä - Verkkokauppa voi estää käyttäjiä muokkaamasta ostoskoriaan maksun jälkeen

Karvinen 2006: Raportin kirjoittaminen https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

Vapaaehtoinen: PortSwigger 2020: What is SQL injection? - Web Security Academy (noin 10 min video) https://www.youtube.com/watch?v=wX6tszfgYp4
SQL Injektio
- Verkkoturvahaavoittuvuus joka mahdollistaa hyökkääjän näyttää dataa, johon ei normaalisti ole pääsyä
- Muilta käyttäjiltä tai niihin tietoihin, joihin itse sovelluksella on pääsy
- Monissa tapauksissa voidaan muuttaa tai poistaa dataa, jolla muutetaan sovelluksen käytöstä tai toimintatapoja
- Pystytään käyttämään DOS - hyökkäyksessä tai päästään paremmin käsiksi sovelluksen taustalla toimiviin palveluihin vaikkapa palvelimiin ja tietokantoihiin ja tätä kautta laajentamaan hyökkäystä

Esimerkki - nettikauppa, jolla käyttäjälle näytetään tietoja ja tavaroita syötteen avulla
```
SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1 #Piiloitetaan tuotteet. Jos laitetaan 0, se näyttää kaikki
```

Eli pelkästään sivuston URLia käyttämällä, päästään käsiksi tietoihin joita ei pitäisi näkyä: 
* https://insecure-website.com/products?category=Gifts ' --

Muuntautuu seuraavasti:

```
SELECT * FROM products
WHERE category = 'Gifts'--' # -- eli kaikki muut tulee kommenttina ja kaikki tuotteet näkyvät Gifts - kategoriassa
AND released = 1
```

Jos halutaan nähdä kaikki tuotteet kaikista kategorioista, käytetään seuraavaa esimerkkiä:

* https://insecure-website.com/products?category=Gifits'+OR+1=1--

Josta tulee 

```
SELECT * FROM products
WHERE category = 'Gifts'
OR 1=1--' AND released =1 #Koska 1=1 on totta, kysely palauttaa kaikki tuotteet
```

Jos on sovellus, jossa laitetaan sähköposti ja salasana
Query= 
```
SELECT * FROM users WHERE
username = 'wiener' #käyttäjän syöttämä AND
password = 'bluecheese'
```

Jos sähköpostikenttään syötetään ```administrator'--```, kysely on seuraava:

```
SELECT * FROM users WHERE
username = 'admininstrator'--
AND password = ''
```
Se palauttaa käyttäjänimen administrator ja kirjautuu sisään käytännössä ilman salasanaa

- On myös mahdollista hakea tietoa muista paikoista tietokannan sisältä, käyttämällä 'UNION':

```
SELECT name, description
FROM products WHERE
category = 'Gifts' UNION
SELECT username, password
FROM users--
```

Miten niitä haavoittuvuuksia löydetään?
- Voi testata erilaisilla skannereilla, jotka on tehty sitä varten
- Syöttää manuaalisesti esim. ```'``` ```' OR 1=1--```, ```ASCII(97)```, ``` '; waitfor delay ('0:0:20')--```

a) Murtaudu 010-staff-only. Ks. Karvinen 2024: Hack'n Fix https://terokarvinen.com/hack-n-fix/

Tein tossa hyvän tovin, raportoin aikani ja jäin sitten pohtimaan vähän pidemmäksi aikaa tehtävän tekoa ja raportti lensi jonnekin bittiavaruuteen. Nyt tulee siis suppeampi versio tapahtumista:

Eli Hack'n Fix ohjeiden mukaan

```
$ sudo apt-get update
$ sudo apt-get -y install wget unzip micro
```

```
$ wget https://terokarvinen.com/hack-n-fix/teros-challenges.zip
$ unzip teros-challenges.zip
```

```
$ cd challenges/010-staff-only/
$ python3 staff-only.py
from flask import Flask, render_template, request # sudo apt-get install python3-flask
ModuleNotFoundError: No module named 'flask'
```

```
$ sudo apt-get -y install python3-flask python3-flask-sqlalchemy

$ python3 staff-only.py
WARNING: Purposefully VULNERABLE APP!
 * Serving Flask app 'staff-only'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
```
![Näyttökuva 2024-11-04 195307](https://github.com/user-attachments/assets/4a3de8f4-d72c-4a55-aff3-9d60c8805bf2)

![Näyttökuva 2024-11-04 221608](https://github.com/user-attachments/assets/0c328d60-8234-4c28-baef-f32d55afa3f4)

![Näyttökuva 2024-11-04 221816](https://github.com/user-attachments/assets/4b57eea2-b3bf-48e3-a12f-2dcf9a0e449d)

![Näyttökuva 2024-11-04 223155](https://github.com/user-attachments/assets/8e1241da-636d-46d0-8df3-710d8481551d)
![Näyttökuva 2024-11-04 223007](https://github.com/user-attachments/assets/7752eb72-95bc-43f8-97a6-4ec3ce62887a)


Etenin siis tuossa järjestyksessä. Nyt on muutama kokeilu tehty, eli syöttämällä kenttään 'OR 1=1 ja kokeilin muutaman kerran leikkiä tuon ```SELECT password FROM pins WHERE pin='123' kanssa.

- Ekana tuli mieleen, että vilkaisisin tuota "Inspect" ominaisuutta ja sieltä jos saisi ton 'Please enter a number' kohdan rustattua niin, että sais kirjotettua sinne muitakin, kun vaan numeroita. Tehtävänä oli tehdä se ilman, joten kokeillaan ny hetken aikaa muita tapoja.
- Update: Piti vähän surffailla ympäriinsä, mutta onnistuin kaatamaan sivun hetkeksi. Selailin Network - tabia ja hakkasin eri tekstejä 'Reveal my password' kohtaan, joka sen sit lopulta kaatoi.

  <img width="755" alt="image" src="https://github.com/user-attachments/assets/ca4046a1-d87c-405a-ae5c-49ec41cbe80e">

- Kokeilin seuraavia juttuja: Löin käytännössä sormen pohjaan ja painon 0 jatkuvalla syötöllä, ei vaikutusta. Halusin testata tällä, pystyykö sillä aiheuttamaan jonkun virheen
- Mahdollisimman suuri luku - nyt 9 pohjassa
- Ei vaikutusta
- Kokeilin negatiivisia lukuja - ei vaikutusta
- Erikoismerkit, SQL - syöte ei toimi
- Nyt aika mennä tutkimaan tarkemmin FireFoxin inspect - toiminnolla
<img width="929" alt="image" src="https://github.com/user-attachments/assets/375c2551-1aa6-49e2-9b47-6c61bbbc2c64">
- Eli ajatuksena tuli heti ensimmäisenä löytää jostain kohta, jolla saisi nuo "rajoitukset" pois, eli pystyisi laittamaan jotain muuta kuin pelkkiä numeroita
- Jos otan tuon "number" - kohdan pois, käsittääkseni rajoitukset lähtevät pois joten kokeillaan sitä
- Löin siis kokeiluna aikaisemman esimerkin mukaan

```
SELECT * FROM users WHERE
username = 'SUPERADMIN'--
AND password = ''
```


![Näyttökuva 2024-11-05 002958](https://github.com/user-attachments/assets/edf7327b-d9a5-4981-993d-624f266782f3)

Takaisin piirrustuspöydälle -- 

Hetken lueskeltuani https://portswigger.net/web-security/sql-injection/union-attacks#using-a-sql-injection-union-attack-to-retrieve-interesting-data ja konsultoituani tekoälyä (ChatGPT), päädyin seuraavaan komentoon (muutaman mutkan ja variaation jälkeen)
![Näyttökuva 2024-11-04 221956](https://github.com/user-attachments/assets/e0448b88-daca-475d-afb8-38a610260401)

Se palautti arvoksi
<img width="701" alt="image" src="https://github.com/user-attachments/assets/c5ca9507-9230-4303-a958-329e74e526e1">




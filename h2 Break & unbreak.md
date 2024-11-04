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



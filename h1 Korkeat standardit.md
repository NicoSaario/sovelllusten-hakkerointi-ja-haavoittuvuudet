# Kotitehtävät

Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

### a) Tutustu kurssin sanastoon, joka on määritelty SFS-EN ISO/IEC 27000:2020:en standardissa, kappaleessa 3. Terms and Definitions. Selvitä seuraavien kappaleiden määritteet ja selitä omin sanoin mitä ne tarkoittavat: 3.2, 3.31, 3.56, 3.58, 3.77. (Ei edellytä teknisiä testejä tietokoneella)

**3.2 attack**
  - Kaikessa yksinkertaisuudessaan pyrkimys päästä käsiksi resursseihin ilman lupaa - joko tuhoamalla,     
   paljastamalla jotain tietoa, muuttamalla tai varastamalla sitä.. tai kaikkia näitä yhdessä.

**3.31 information security incident**
  - Yksi tai useampi tapahtuma liittyen mahdolliseen tietoturvaloukkaukseen tai muuhun turvallisuuden kannalta merkittävään seikkaan,
    jolla on todennäköisyys vaarantaa liiketoimintaa ja uhata tietoturvaa
  - Viitataan oikeastaan kaikkiin kohtiin, jotka jollain tavalla iittyy tietoturvaan ja sen vaarantumiseen.

**3.56 requirement**
  -  Voi olla joko määritelty vaatimus dokumentoituihin tietoihin tai rivien välistä luettava tarve tai odotus, joka on yhteinen tai vakiintunut ja jota kaikkien pitäisi noudattaa.
  -  Hyvänä esimerkkinä vaikkapa se, että viimeinen henkilö työpaikalta lähtiessä varmistaa tilojen sulkemiset ja valojen sammuttamisen - tähän toki yleensä ohjeistetaan, mutta kaikki tietävät sen olevan viimeisen lähtevän tehtävä.

 **3.58 review**
  - Toiminta, jolla arvioidaan kohteen sille asetettujen vaatimusten ja tavoitteiden täyttyminen. 
    Varmistaa siis sen, että suunnitellut toiminnot toteutuvat. Ne voivat olla pitkän tai lyhyen aikavälin       suunnitelmia tai tehtäviä.

 **3.77 vulnerability**
  - Heikkous resurssissa tai hallinnassa, jota yksi tai useampi uhka voi hyödyntää



### b) Tutustu standardiin ISO 27034-1 - 5. Selvitä mistä standardi kokonaisuudesta on kyse. (Ei edellytä teknisiä testejä tietokoneella)
- Käsittääkseni tuo -5 tarkoittaa standardisarjaa, jossa jokaisella on oma aiheensa. Se koostuu viidestä eri sarjasta:
1. Yleiskatsaus ja käsitteet
2. Organisaation normatiivinen kehys
3. Sovellustietoturvan hallintaprosessi
4. Sovellustietoturvan validointi
5. Protokollat ja sovellustietoturvan hallintakeinon tietorakenne

Yriykset voivat ja niiden kannattaa hyödyntää tätä standardia saamaan sovellustensa turvallisuus ajan tasalle. Standardi tarjoaa ohjekehyksen, jota noudattamalla lähes Raamatullisesti voi ansaita itselleen leiman (sertifikaatin). Koko standardin ideana on auttaa organisaatioita kartoittamaan joko nykyiset prosessinsa tai tulevat prosessit tarjoamalla mm. sovellustietoturvan käsitteitä, riskienhallintaa, varmennusmenetelmien hallintaa, tietoturvatoimien valitsemista jne.



### c) Kuuntele podcast: Meurman 2021: Laatulöpinät 30: Tietoturvallisuus ohjelmistokehityksessä (jakson mp3, Laatolöpinät RSS-syöte). Mitä mieltä olet podcastin väittämistä? (Ei edellytä teknisiä testejä tietokoneella)

Väittämät:

**1. Mikään ohjelmisto ei ole täysin tietoturvallinen**

Olen väittämän kanssa samaa mieltä, kuin podcastissa. Vaikka uusi ohjelmisto voisi olla hetken aikaa murtamaton, ei se tarkoita sitä, että se olisi täysin tietoturvallinen. Aina on jokin porsaanreikä tai aukko, jota on mahdollista hyödyntää.

**2. Hallinnollinen tietoturva on teknisen tietoturvan onnistumisen edellytys**

Edelleen samaa mieltä. Mikä edes on teknisen tietoturvan merkitys, jos hallinnollinen tietoturva ei ole siinä pohjana? Molemmat yhdessä toimivat, mutta kumpikaan ei toimi yksinään. Toki aina pitää muistaa, että yhden hengen yrityksellä on eri hallinnollisen tietoturvan tarpeet, mutta kyllä siihenkin on hyvä määrittää pohja.

**3. Automaatiotestaus on ohjelmiston tietoturvan kannalta erittäin tärkeää**

Mitä enemmän voidaan automaatiolla testata, sitä vähemmän se teettää työtä. Jos sitä pystytään automaatiolla testaamaan, onhan se huomattavasti nopeampi keino. Sanoisin, että automaatiotestaus on hyvä kevyiden haavoittuvuuksien karsintaan ja varmasti poistaa suurimman osan pelkistä 'kokeiluista' ohjelmistoon. Kuten podissakin mainittiin, siihen voidaan luottaa liian sokeasti ja unohtaa se automaatiolla tehty testaus, joka ei kuitenkaan jokaista aukkoa pysty tarkistamaan. Joten pidän sitä erittäin tärkeänä, mutta ei pidä unohdaa muita menetelmiä.

**4. Ohjelmistoa suunnitellessa voidaan tehdä paljonkin auttamaan käyttäjiä toimimaan tietoturvallisesti. Usein nämä toimenpiteet kuitenkin vaikuttavat negatiivisesti käytettävyyteen.**

Eipä tähän lisättävää. Se yleensä vaikuttaa negatiivisesti, mutta se usein auttaa ohjaamaan käyttäjiä oikeaan suuntaan. On tärkeää, että ohjelmisto suunnitellaan niin, ettei käyttäjä joutuisi tunnin työllä kirjautumaan sisään, mutta on tärkeä varmistaa myös tietoturva-aspektit.

**5. Ohjelmiston tietoturvallisuuden suunnitteluun vaikuttaa paljolti se, kuinka arkaluontoisia tietoja ohjelmistolla on tarkoitus käsitellä**

Osaltaan se vaikuttaa, mutta paljolti? Kuten podissa mainittiin - jos suunnittelun jättää vain sen varaan, voi olla liiketoiminnalle haitallista jos tietoturva on suunniteltu pelkästään arkaluontoisia tietoja silmällä pitäen. Arkaluontoiset tiedot kuitenkin pitää ottaa huomioon ja niiden suojaaminen nykypäivänä on erittäin tärkeää. 

**6. Ohjelmistokehittäjät näkevät omat ohjelmistonsa aina merkittävästi riskialttiimpina kuin muiden tekemät ohjelmistot**

En missään määrin ole ohjelmistokehittämisen parissa ollut, mutta voin kuvitella tämän molemmat puolet. Vertaisin ehkä tätä ihan käytännön elämään, jossa vaikkapa muistiipanoja tehdessä voi olla kriittisempi ja tunnistaa helpommin ne omat aukkonsa sekä tietää niiden puuttellisuuden, kuin muiden tekemien muistiinpanojen parissa. Kuitenkin syvällisemmin ajateltuna, oman ohjelmistonsa kehittäjä voi tulla sokeaksi omiin töihinsä, jolloin se ei välttämättä pidä paikkaansa.

### d) Asenna Debian 12-Bookworm virtuaalikoneeseen. Päivitä kaikki ohjelmat. (Poikkeuksellisesti tätä alakohtaa ei tarvitse raportoida, paitsi jos jokin ei toimi. Ympäristö tarvitaan seuraavalle oppitunnille. Joten tämän kohdan vastaukseksi riittää kuittaus, että Linux on asennettu.)

Asennettu on!


### Lähteet

- Laatulöpinät-podcast: Tietoturvallisuus ohjelmistokehityksessä – Tarkastele kokonaisuutta ja hyödynnä viitekehykset, Julkaistu 25.10.2021, Kuunneltavissa: https://www.arter.fi/podcast/laatulopinat-podcast-tietoturvallisuus-ohjelmistokehityksessa-tarkastele-kokonaisuutta-ja-hyodynna-viitekehykset/ (Kuunneltu 27.10.2024)

- Kurssilla jaettu ISO - materiaali

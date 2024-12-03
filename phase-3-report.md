# Varausjärjestelmän tietoturvaraportti

## Yleiskatsaus
Tämä raportti kuvaa viisi kriittisintä ongelmaa, jotka havaittiin varausjärjestelmässä. Ongelmat löytyivät käyttämällä ZAP (OWASP ZAP) -työkalua tietoturvatarkastukseen sekä järjestelmän manuaalisella testaamisella.

---

## Havaitut ongelmat

### 1. **Anti-CSRF-tunnisteiden puuttuminen**
   - **Riskitaso:** Keskitaso  
   - **Tapauksia:** 2  

   #### Mikä on vialla?  
   Järjestelmästä puuttuvat Anti-CSRF-tunnisteet kriittisistä lomakkeista ja pyynnöistä, mikä tekee sen alttiiksi Cross-Site Request Forgery (CSRF) -hyökkäyksille.

   #### Kuinka se löydettiin?  
   ZAP skannaus havaitsi, että kahdessa keskeisessä päätepisteessä ei ole CSRF-suojausta.

   #### Kuinka se tulisi korjata?  
   - Lisää CSRF-tunnisteet kaikkiin tilaa muuttaviin pyyntöihin (esim. varaukset, käyttäjätilien muutokset).
   - Varmista palvelinpuolella, että tunnisteet validoidaan ja pyynnöt tulevat aidoista istunnoista.

---

### 2. **Autentikointipyyntö tunnistettu**
   - **Riskitaso:** Informatiivinen  
   - **Tapauksia:** 1  

   #### Mikä on vialla?  
   Järjestelmän autentikointiprosessi tunnistettiin, mikä viittaa mahdollisiin puutteisiin käyttäjätietojen turvallisessa käsittelyssä.

   #### Kuinka se löydettiin?  
   ZAP havaitsi autentikointipyynnön skannauksen aikana ja suositteli tarkempaa tarkastelua.

   #### Kuinka se tulisi korjata?  
   - Varmista, että kaikki autentikointipyynnöt lähetetään HTTPS:n kautta suojaamaan tunnistetietoja.
   - Ota käyttöön mekanismit, kuten nopeuden rajoittaminen, turvallinen istuntojen hallinta ja salasanan hajautus.

---

### 3. **Virhe varauksia tehdessä**
   - **Riskitaso:** Korkea (toiminnallinen ongelma)  

   #### Mikä on vialla?  
   Käyttäjät eivät voi luoda uusia varauksia järjestelmässä. Tämä haittaa järjestelmän ydintoiminnallisuutta.

   #### Kuinka se löydettiin?  
   Tämä ongelma havaittiin manuaalisen testauksen aikana, kun yritettiin tehdä varausta. Järjestelmä palautti virheviestin: `"Error during reservations."`

   #### Kuinka se tulisi korjata?  
   - Tarkista palvelimen lokit ja sovelluksen debug-tulosteet ongelman syyn selvittämiseksi.
   - Varmista, että varausdata validoidaan ja käsitellään oikein.
   - Varmista, että tietokannan rakenne tukee kaikkia varauksille vaadittavia kenttiä.

---

### 4. **CSP: Yleinen ("Wildcard") direktiivi**
   - **Riskitaso:** Keskitaso  

   #### Mikä on vialla?  
   Sisällön turvakäytäntö (CSP) sisältää yleisen direktiivin (`*`), mikä mahdollistaa resurssien lataamisen mistä tahansa lähteestä. Tämä lisää riskiä haitallisten skriptien suorittamisesta.

   #### Kuinka se löydettiin?  
   ZAP liputti tämän tietoturvariskin skannauksen aikana.

   #### Kuinka se tulisi korjata?  
   - Korvaa CSP:n yleiset direktiivit tarkemmilla määrittelyillä, jotka sallivat resurssien lataamisen vain luotettavista lähteistä.
   - Käytä tiukkaa CSP-politiikkaa XSS- ja tietojen syöttöhyökkäysten ehkäisemiseksi.

---

### 5. **X-Content-Type-Options -otsikko puuttuu**
   - **Riskitaso:** Matala  
   - **Tapauksia:** 2  

   #### Mikä on vialla?  
   HTTP-vastauksista puuttuu `X-Content-Type-Options` -otsikko, mikä sallii selainten arvailla tiedostotyyppejä ja mahdollisesti suorittaa haitallista sisältöä.

   #### Kuinka se löydettiin?  
   ZAP havaitsi tämän otsikon puuttumisen kahdessa HTTP-vastauksessa.

   #### Kuinka se tulisi korjata?  
   - Lisää `X-Content-Type-Options: nosniff` -otsikko kaikkiin HTTP-vastauksiin MIME-tyyppien arvaamisen estämiseksi.
   - Varmista, että otsikko sovelletaan johdonmukaisesti kaikille resursseille.

---

## Yhteenveto
### Keskeiset havainnot:
1. **Anti-CSRF-tunnisteiden puuttuminen:** Keskitasoinen riski, edellyttää välitöntä korjaamista.
2. **Autentikointipyyntö tunnistettu:** Suosittelee autentikointiturvallisuuden tarkistamista.
3. **Varausvirhe:** Korkea prioriteetti; korjaa järjestelmän ydintoiminnallisuus.
4. **CSP: Yleinen direktiivi:** Keskitasoinen riski, tiukempi CSP-politiikka tarvitaan.
5. **X-Content-Type-Options -otsikko puuttuu:** Matala riski, mutta tärkeä tietoturvan täydentämiseksi.

### Seuraavat vaiheet:
1. Korjaa **varausvirhe** ensin järjestelmän toiminnan palauttamiseksi.
2. Ota käyttöön **Anti-CSRF-tunnisteet** CSRF-hyökkäysten torjumiseksi.
3. Tarkista autentikointiturvallisuus, CSP-politiikat ja HTTP-otsikot.

---

## Lokikirjan linkki
[Linkki lokikirjaan](#)  

# Varausjärjestelmän tietoturvaraportti

## Yleiskatsaus
Tämä raportti kuvaa havaittuja tietoturvaongelmia varausjärjestelmässä. Ongelmat löydettiin käyttämällä OWASP ZAP -työkalua sekä manuaalista testausta. Raportin tavoitteena on tuoda esiin kriittisimmät tietoturvariskit ja antaa suosituksia niiden korjaamiseksi.  

---

## Havaitut ongelmat

### 1. **Administrator-käyttäjätilin luominen ilman rajoituksia**
   - **Riskitaso:** Korkea  

   #### Mikä on vialla?  
   Sivustolle kuka tahansa voi luoda käyttäjätilin ja asettaa itsensä `administrator`-rooliin. Tämä antaa täydet hallintaoikeudet järjestelmään.

   #### Kuinka se löydettiin?  
   Manuaalisen testauksen aikana havaittiin, että roolin määrittelyä ei rajoiteta käyttäjän rekisteröintiprosessissa.

   #### Kuinka se tulisi korjata?  
   - Lisää palvelinpuolinen validointi, joka estää käyttäjiä luomasta `administrator`-rooleja ilman lupaa.
   - Käytä järjestelmänvalvojan hyväksyntää tai erillistä kutsuprosessia.

---

### 2. **Anti-CSRF-tunnisteiden puuttuminen**
   - **Riskitaso:** Keskitaso  
   - **Tapauksia:** 4  

   #### Mikä on vialla?  
   Järjestelmä ei sisällä Anti-CSRF-tunnisteita kriittisissä toiminnoissa, mikä altistaa sen CSRF-hyökkäyksille.  

   #### Kuinka se löydettiin?  
   OWASP ZAP liputti neljä tapausta, joissa CSRF-tunnisteet puuttuivat.

   #### Kuinka se tulisi korjata?  
   - Lisää CSRF-tunnisteet kaikkiin tilaa muuttaviin pyyntöihin.
   - Varmista palvelinpuolella, että tunnisteet tarkistetaan jokaisessa pyynnössä.

---

### 3. **Format String Error**
   - **Riskitaso:** Keskitaso  
   - **Tapauksia:** 1  

   #### Mikä on vialla?  
   Sovellus voi olla altis formaattiongelmille, mikä voi mahdollistaa haitallisen koodin suorittamisen tietyissä tilanteissa.

   #### Kuinka se löydettiin?  
   ZAP tunnisti tämän ongelman tietojen käsittelyyn liittyvässä osassa.

   #### Kuinka se tulisi korjata?  
   - Käytä turvallisia tietojen käsittelymekanismeja ja vältä suoria merkkijonon muotoiluoperaatioita.
   - Testaa ja validoi kaikki käyttäjän syöttämät tiedot.

---

### 4. **Persistent Cross-Site Scripting (XSS) JSON-vastauksissa**
   - **Riskitaso:** Matala  
   - **Tapauksia:** 2  

   #### Mikä on vialla?  
   Sovellus sisältää pysyvän XSS-haavoittuvuuden JSON-vastauksissa, mikä voi johtaa haitallisen koodin suorittamiseen käyttäjän selaimessa.  

   #### Kuinka se löydettiin?  
   ZAP havaitsi tämän JSON-vastauksista, jotka sisältävät käsittelemättömiä käyttäjän syöttämiä tietoja.

   #### Kuinka se tulisi korjata?  
   - Suodata ja koodaa kaikki käyttäjän syöttämät tiedot ennen niiden palauttamista vastauksissa.
   - Käytä Content Security Policy -otsikoita hyökkäysten torjumiseksi.

---

### 5. **Session Management Response Identified**
   - **Riskitaso:** Informatiivinen  
   - **Tapauksia:** 4  

   #### Mikä on vialla?  
   Istunnonhallintamekanismit on tunnistettu, mutta niiden turvallisuus voi vaatia tarkempaa analysointia.

   #### Kuinka se löydettiin?  
   ZAP havaitsi useita tapauksia, joissa istuntotietoja paljastui HTTP-vastauksissa.

   #### Kuinka se tulisi korjata?  
   - Varmista, että istuntotunnisteet salataan ja lähetetään vain HTTPS:n kautta.
   - Toteuta istuntojen aikakatkaisu ja suojattu hallinta.

---

## Yhteenveto

### Keskeiset havainnot:
1. **Administrator-käyttäjätilin luominen ilman rajoituksia:** Suurin tietoturvariski.
2. **Anti-CSRF-tunnisteiden puuttuminen:** Altistaa CSRF-hyökkäyksille.
3. **Format String Error:** Keskitasoinen riski; voi mahdollistaa haitallisen koodin suorittamisen.
4. **Persistent XSS JSON-vastauksissa:** Pieni, mutta merkittävä riski käyttäjien turvallisuudelle.
5. **Istuntotunnisteiden hallinta:** Informatiivinen huomio, joka vaatii jatkotoimenpiteitä.

---

## Suositellut toimenpiteet
1. Korjaa **administrator-roolin luontiongelma** estämällä roolien vapaa määrittely.
2. Lisää **Anti-CSRF-tunnisteet** ja tarkista palvelinpuolen validointi.
3. Korjaa **formaattiongelmat** käyttämällä turvallisia tietojen käsittelytapoja.
4. Suodata ja koodaa **käyttäjätiedot** pysyvän XSS:n estämiseksi.
5. Paranna **istuntotunnisteiden hallintaa** ja varmista niiden turvallisuus.

---

## Link to logbook
[[Linkki](https://github.com/jerejjj/probable-invention/blob/fe0cdd0d06daf74af03d1d11ed9c5297c0c2ae15/README.md)]  

# MQTT-pskreporter.info-Japan
MQTT pskreporter.info Japan active stations monitoring system
An advanced, web-based, and bandwidth-optimized real-time dashboard for tracking Japanese amateur radio stations (DXCC 339) via the **PSK Reporter MQTT** network, focusing on hunting for the **WAJA (Worked All Japan)** award.

Customized for station **9A5CW** with QTH locator **JN65XF**.
## Croatian:
Ovdje je detaljan pregled svih mogućnosti koje izvršava vaša skripta, a odmah ispod se nalazi i kompletan **`README.md`** na engleskom jeziku spreman za vaš GitHub repozitorij.

### 📋 Opis svih mogućnosti i funkcija skripte

Ova web aplikacija (Dashboard) dizajnirana je kao napredni radioamaterski alat za praćenje japanskih postaja (DXCC 339) i lov na **WAJA (Worked All Japan)** diplomu u stvarnom vremenu. Evo popisa ključnih funkcija koje skripta izvršava:

1. **Optimizirano primanje podataka (MQTT WSS):**
* Spaja se direktno na PSK Reporter WebSocket MQTT poslužitelj (`wss://mqtt.pskreporter.info:1886`) koristeći ciljane filtre (`pskr/filter/v2/.../339`) čime prima isključivo japanski promet bez opterećenja mrežne propusnosti.


2. **Dvosmjerni napredni brojači na gumbima opsega (`160M` – `6M` i `ALL`):**
* **Gornji red (`X/47`):** Prikazuje ukupan napredak za diplomu, umanjen za ukupan broj potrebnih prefektura definiranih u konfiguraciji (npr. `36/47`).
* **Donji red (`detektiranih / potrebnih`):** Prikazuje točan broj uspješno detektiranih/zabilježenih stanica u odnosu na ukupan broj traženih prefektura s vaše liste (npr. `7/11 needed`).


3. **Automatska sinkronizacija s `needed_prefs.txt`:**
* Prilikom učitavanja stranice, skripta čita datoteku `needed_prefs.txt` i automatski postavlja kvačiće u panelu prefektura, osiguravajući da je tekstualna datoteka uvijek jedini izvor istine (svaka promjena u datoteci automatski se reflektira na sučelje nakon osvježavanja).


4. **Pametno filtriranje i prepoznavanje prefektura:**
* Učitava CSV bazu (`japan_calls.csv`) s više od 500,000 zapisa kako bi automatski prepoznala kojoj japanskoj prefekturi pripada pojedini pozivni znak (`TX` ili `RX`).
* Provjerava zadovoljava li spot uvjete odabranog opsega i nalazi li se na popisu traženih prefektura.


5. **Zvučni i vizualni alarmi u stvarnom vremenu:**
* **Zvučni signal (Beep):** Generira jasan ton (Web Audio API) svaki put kada se detektira i zabilježi nova tražena prefektura.
* **Treperenje gumba (`redBlink`):** Ako se nova tražena prefektura pojavi na opsegu koji trenutno *nije* odabran, gumb tog opsega počinje treperiti u crvenoj boji kako bi vas upozorio.


6. **Trajna pohrana podataka (`localStorage`):**
* Sve pronađene i zabilježene potrebne stanice automatski se spremaju u memoriju preglednika (`localStorage`), čime se sprječava gubitak podataka prilikom osvježavanja stranice (F5).


7. **Upravljanje tablicama i izvoz podataka:**
* Omogućuje filtriranje prikaza broja redova u live streamu (zadnjih 20, 50, 100 ili 200 spotova).
* Mogućnost skrivanja/prikazivanja live stream tablice radi uštede resursa.
* **Izvoz u TXT:** Generira i preuzima tekstualnu datoteku sa svim zabilježenim postajama formatiranu s Windows završecima redaka (`\r\n`) za laku kompatibilnost s Notepadom.
* Mogućnost ručnog čišćenja zabilježenih stanica.



---

# 🚀 README.md (English Version for GitHub)

```markdown
# 9A5CW WAJA Logger (Japan DXCC 339 PSK Reporter Dashboard)

An advanced, web-based, and bandwidth-optimized real-time dashboard for tracking Japanese amateur radio stations (DXCC 339) via the **PSK Reporter MQTT** network, focusing on hunting for the **WAJA (Worked All Japan)** award.

Customized for station **9A5CW** with QTH locator **JN65XF**.

---

## 🌟 Key Features

* **Optimized Bandwidth:** Connects to the PSK Reporter WSS MQTT server using targeted filters (`pskr/filter/v2/.../339`) to receive exclusively Japanese spot traffic without network congestion.
* **Dual-Tier Dynamic Band Chips (`X/47` + `needed`):** 
  * Top row displays total award progress minus needed prefectures (e.g., `36/47`).
  * Bottom row displays detected vs. total needed prefectures per band (e.g., `7/11 needed`).
* **Single Source of Truth Configuration:** Automatically synchronizes checkbox states and filters directly from the `needed_prefs.txt` file on every load.
* **Smart Prefecture Matching:** Maps incoming callsigns to Japanese prefectures using a robust CSV database (`japan_calls.csv`).
* **Visual & Audio Alerts:** 
  * Audio beep notification (Web Audio API) upon logging a new needed prefecture.
  * Red blinking animation (`redBlink`) on band buttons when a needed station is spotted on an inactive band.
* **Persistent Storage (`localStorage`):** Automatically saves logged stations in the browser to prevent data loss on page refresh (F5).
* **Export & Control Tools:** 
  * One-click export of logged needed stations to a Windows-compatible `.txt` file (`\r\n` line endings).
  * Live stream row limiter and hide/show toggle.

---

## 📂 File Structure

Ensure the following files are placed in the same root directory:

```text
├── index.html          # Main web dashboard application
├── japan_calls.csv     # Database mapping Japanese callsigns to prefectures
└── needed_prefs.txt    # Configuration file for needed prefectures per band

```

---

## ⚙️ Configuration Format

### 1. `needed_prefs.txt`

Defines the required prefectures per band. Example format:

```text
160m: Aomori, Ishikawa, Fukushima, Okinawa, Oita, Tokushima, Aichi, Fukuoka, Gifu, Kagoshima, Wakayama
80m: Akita, Ibaraki, Ishikawa, Kagawa, Kagoshima, Kanagawa, Miyazaki, Nagasaki, Nara, Oita, Saitama, Shiga, Tottori, Yamagata, Yamaguchi

```

### 2. `japan_calls.csv`

Contains callsign-to-prefecture mappings (e.g., `JE7LHT, Fukushima`).

---

## 🛠️ Installation & Usage

Due to modern web browser security policies (CORS rules), running the app directly via double-click (`file:///`) is not recommended. Use a local web server.

### Running via Python (Recommended)

1. Open your terminal or Command Prompt in the project folder.
2. Start the local HTTP server:
```bash
python -m http.server 8080

```


3. Open your web browser and go to:
👉 **`http://localhost:8080`**

---

## 📋 Technologies Used

* **Frontend:** HTML5, CSS3, Vanilla JavaScript
* **Protocol:** MQTT (`mqtt.min.js`) over PSK Reporter WSS
* **Environment:** Python `http.server` (for local development)

---

## 📜 License

Developed for personal amateur radio operations (9A5CW).

```

<ElicitationsGroup>
</ElicitationsGroup>

```

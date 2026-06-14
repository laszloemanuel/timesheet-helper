# TimeSheet Segéd

Egyfájlos statikus web app, ami a Google Naptáradból összeszedi a megbeszéléseket, és a céges
TimeSheet formátumába (Dátum / Hét napja / Hónap / Hét / Projekt / Task / Task_leiras / Oraszam /
Task_id / Tudnivalók) készíti elő a sorokat — `.xlsx` letöltéssel **és** vágólapra másolható
táblával (beilleszthető a céges Excelbe).

## Adatvédelem
- A **kód** publikus (GitHub Pages), de a **naptáradból letöltött adatok soha nem hagyják el a
  böngésződet** — memóriában és `localStorage`-ban tárolódnak, semmilyen szerverre nem mennek.
- A Google hívás közvetlenül böngésző → Google Calendar API (OAuth access token a böngészőben).
- A repo-ban nincs Client ID, token vagy bármilyen személyes adat.

## Funkciók
- Google Calendar betöltése dátumtartományra (gyorsgombok: ez a hét / hónap / előző hónap).
- Dátum, nap, hónap, ISO-hét automatikus kitöltése a megadott formátumban; a meeting neve a
  `Task_leiras`, hossza az `Oraszam` (negyedórás lépték, szerkeszthető).
- **Projekt kódok**: tetszőleges projekt megnevezés, hozzá tartozó **Task_id**-k és **Tag**-ek.
- **Tag alapú felajánlás**: ha egy tag szerepel a meeting nevében, az app automatikusan felajánlja
  az adott projektet a sorhoz („javasolt" jelölés).
- **Recurring öröklés**: ismétlődő meetingnél elég egyszer beállítani a projektet — az összes
  példányra öröklődik (`recurringEventId` alapján). Az egyszer beállított hozzárendelések
  meg is maradnak újratöltés után.
- Egész napos / visszautasított események kihagyása.
- Export: `.xlsx` (SheetJS) + vágólap (hu-locale tizedesvesszővel a sima beillesztéshez).

## Egyszeri Google OAuth beállítás (~5 perc)
1. `console.cloud.google.com` → új projekt (vagy meglévő).
2. **APIs & Services → Library** → kapcsold be a **Google Calendar API**-t.
3. **OAuth consent screen** → External; töltsd ki a kötelező mezőket; add hozzá magad **Test user**-ként.
4. **Credentials → Create credentials → OAuth client ID → Web application**.
   Az **Authorized JavaScript origins**-hoz add hozzá az oldal URL-jét
   (pl. `https://laszloemanuel.github.io`).
5. Másold ki a **Client ID**-t, és illeszd be az appban a **⚙︎ Beállítások** alá. (Ez nem titok,
   csak a böngésződben tárolódik.)

## Használat
1. Nyisd meg az appot, állítsd be a Client ID-t (⚙︎ Beállítások → Mentés).
2. **Bejelentkezés Google-lel**.
3. Válassz dátumtartományt → **Naptár betöltése**.
4. Soronként válaszd ki a projektet (és Task_id-t). Új projektet a *Projektek kezelése* gombbal vagy
   a sor projekt-legördülőjében a *+ Új projekt…* opcióval hozhatsz létre.
5. **Excel (.xlsx)** letöltés, vagy **⧉ Vágólapra** → beillesztés a céges fájlba.

## Futtatás
Statikus fájl — csak nyisd meg az `index.html`-t, vagy hostold (GitHub Pages). Nincs build, nincs
dependency a két CDN-en kívül (Google Identity Services, SheetJS).

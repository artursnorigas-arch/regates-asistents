# Regates asistents — projekta apraksts

Stāvoklis 2026-10-31. Apkopots no repozitorija, `../ra-dati/`, izlaišanas prasmes un sarunām Claude Code. Atjaunini, kad mainās uzbūve vai plāni.

## Kas tas ir
Burāšanas regates asistents telefonam un planšetei (pārlūkā, iOS un Android lietotnē), ko izstrādā un lieto komanda uz **Gabriel LAT-3** (ORC Club 2026, sert. Nr. 040/26). Darbojas bez interneta; dati glabājas tikai ierīcē.

- Publicēts: https://artursnorigas-arch.github.io/regates-asistents/ (GitHub Pages, zars `main`)
- Repozitorijs: `artursnorigas-arch/regates-asistents`
- Artefakts claude.ai: "Regates asistents" — https://claude.ai/artifact/WAnAidDUNYe5jjmh3iKJBt
- Pašreizējā versija: `2026-10-25.7` (`APP_VER` kodā un `version.json`)
- Android APK: GitHub Release `android-latest` (testa versija planšetei)

## Faili
| Ceļš | Saturs |
|---|---|
| `index.html` | **Visa lietotne vienā failā** (~1400 rindu, HTML+CSS+JS, bez bibliotēkām). Tas ir "telefona fails": pirmkods ietīts `<!doctype>` skeletā. |
| `version.json` | `{"v","note"}` — lietotne to nolasa un rāda zilo joslu "Jauna versija" ar pogu Atjaunināt. |
| `mobile/` | Capacitor 8 čaula iOS un Android (`appId` `lv.regatesasistents.test`; spraudņi filesystem, share). `npm run sync` iekopē `../index.html` uz `www/` un sinhronizē. `ios/App/build/` ir būvējuma artefakti — nelabot ar roku. |
| `.github/workflows/android.yml` | Pēc push uz `main` (ja mainīts `index.html` vai `mobile/`) būvē debug APK un publicē to kā Release `android-latest`. |
| `../ra-dati/` | Braucienu dati: `IndexedDB.sqlite3` (izvilkta no ierīces), `dec.py` (dekodē IndexedDB ierakstus uz `sess.json`), `brauciens-*.csv`. Pagaidām divi īsi testa ieraksti no 2026-10-25. |

## Lietotnes uzbūve (`index.html`)
- **Cilnes:** Ekrāns · Distance · AIS · Braucieni · Iestatījumi.
- **Ekrāna skati:** starta skats (taimeris, līdz līnijai, rezerve, favorizētais gals, starta temps) un brauciena skati (Starts/Stūre u.c.) ar konfigurējamiem rādītājiem (`METRICS`, `BARS`).
- **Moduļi (`MODS`, `has(m)`):** Free — `core` (starts, līnija, kurss, ātrums, vēja maiņa pret halzi, ieraksts, eksports). Pro — `orc` (polāras, mērķa ātrums, sniegums pret ORC), `course` (distance, laylines, zīmju apņemšana, laiks līdz finišam), `instr` (Signal K), `ais`. Plānoti: `crew`, `boat`, `analysis`. Pro pagaidām ieslēdz ar "Ieslēgt Pro (izstrādes režīms)".
- **Stāvoklis:** `S` (iestatījumi, zīmes, polāras, halzes) → `localStorage["regate-v1"]`. Braucieni → IndexedDB `regate`, store `sess` (`{id,start,end,status,boat,course,pts[],ev}`).
- **Avoti:** telefona GPS (Geolocation), kustības sensori (sasvērums, garensvēre, kompass ar "Nulle" kalibrāciju; ķeksis "Telefons nostiprināts"), Signal K WebSocket (noklusējums `ws://10.10.10.1:3000/signalk/v1/stream`): SOG/COG, STW, heading, AWA/AWS, TWS/TWA. Wake Lock, vibrācija, skaņas signāli (AudioContext). No `.6`: pilnekrāna režīms, GPS brīdinājums, manevra noteikšana.
- **Starta procedūra:** 5/4 min, SINHR., signālu žurnāls (AP, X, 1. aizstāšana), PIN/RC fiksēšana vai koordinātas.
- **Vējš:** TWD no halžu kursiem vai instrumentiem; vēja maiņa (lift/header) pret atskaites kursu; straumes novērtējums.
- **Eksports:** CSV (`laiks_utc,lat,lon,sog_kn,cog_deg,novirze_no_halzes_deg,lift_deg,twd_deg,halze,sasverums_deg,garensvere_deg,kompass_deg,telefons_nostiprinats`), GPX, JSON.

## Izlaišana
Lieto prasmi `regates-asistents-izlaisana`. Galvenais: palielināt `APP_VER` (`GGGG-MM-DD.N`) → `node --check` skriptam → Playwright simulācijas tests 390×844 (Free un Pro) → artefakts → `index.html` + `version.json` → commit un push uz `main`. Sezonā (maijs–septembris) pirms nedēļas nogales tikai labojumi.

## Plāni un idejas (no 2026-10-31 sarunas)
Prioritāte:
1. **Savas polāras no ierakstiem** — šūnas TWS × TWA, 90. percentile, bez manevriem; režīmi ORC / Savas / Jaukts.
2. **Liels "% no polāras" skats** (rādītājs `perf` jau ir; vajag atsevišķu lielu skatu).
3. **Sasvērums pret vēju** — labākā sasvēre katrai šūnai, brīdinājums par pārtrimmēšanu (ierakstos sasvere jau ir).

Citas idejas: regulācijas žurnāls ar ātrajām pogām; "labākā zināmā regulācija" līdzīgos apstākļos; A/B tests; viļņu indekss no akselerometra; drifts (leeway); reakcija uz brāzmām; balss paziņojumi; manevru vērtējums; lomas komandā; ekrāni katram komandas loceklim; foto un balss piezīmes ar laiku; automātisks atskats pēc brauciena; salīdzinājums ar konkurentiem caur AIS.

Aparatūras virziens: Raspberry Pi kā Signal K serveris ar savu Wi-Fi, NMEA 0183/2000 multipleksers, ekrāns pie kajītes ieejas.

## Kas nav pieejams Claude Code sesijās
- claude.ai projekta "Regates asistents" Instructions, Context (4 faili, t.sk. `claude/regates-asistents.html`, `claude/README-projekts.md`), projekta karte un lēmumu žurnāls.
- Agrākās sarunas: "Gabriel NMEA 2000 datu pieslēgšana", "Burāšanas sniegumu rādītāji", "Burāšanas regates aprēķins".

Ja tur ir lēmumi vai dati, kas vajadzīgi kodam, pārnes tos uz šo failu vai `CLAUDE.md`.

## Atvērti jautājumi
- Izmaiņas tiek veiktas arī citur (versijas `.6`–`.7` nāca no citas vides). Pirms darba vienmēr `git pull`.
- Repozitorijā nav automātisko testu; simulācijas tests tiek rakstīts izlaišanas laikā.
- Prasme min `regates-asistents.html` (pirmkods bez skeleta), bet repozitorijā ir tikai ietītais `index.html`; labojot kodu, rediģē `index.html` tieši.

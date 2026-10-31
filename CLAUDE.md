# Regates asistents

Burāšanas regates asistenta lietotne (web + `mobile/` iOS/Android, Capacitor). Pirms darba `git pull` — izmaiņas nāk arī no citām vidēm. Publicēta caur GitHub Pages: https://artursnorigas-arch.github.io/regates-asistents/

- Jaunas versijas izlaišanai lieto prasmi `regates-asistents-izlaisana` (testi, artefakts, telefona fails, `version.json`).
- `mobile/ios/App/build/` ir būvējuma artefakti, tos nelabo ar roku.
- Pilns apraksts (uzbūve, dati, plāni): `docs/2026-10-31-projekta-apraksts.md`. Izlasi pirms lielākām izmaiņām un atjaunini, kad mainās uzbūve vai plāni.
- Visa lietotne ir viens fails `index.html` (bez bibliotēkām); `mobile/www/index.html` ir tā kopija (`cd mobile && npm run sync`).
- Laiva: Gabriel LAT-3, ORC Club 2026, sert. 040/26. Braucienu dati: `../ra-dati/`.

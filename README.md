# Acoll-IA — Desplegament a Vercel

App d'aprenentatge de català per a alumnat nouvingut. PWA estàtica desplegable en qualsevol CDN.

## Contingut del paquet

| Fitxer | Funció |
|---|---|
| `index.html` | App completa (corregit error de sintaxi a línia 5374) |
| `manifest.json` | Manifest PWA (nom, icones, colors) |
| `sw.js` | Service Worker (funcionament offline) |
| `vercel.json` | Configuració de Vercel (headers, cache) |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png` | Icones PWA |

## Recomanació: desplegament via GitHub

És la millor opció a llarg termini per al teu cas: cada vegada que actualitzes el codi al repo, Vercel re-desplega automàticament. Tens historial de canvis, pots fer rollback i et serveix com a còpia de seguretat.

### Passos (15 minuts la primera vegada)

**1. Crea el repositori a GitHub**
- Entra a https://github.com → "New repository" → nom: `acoll-ia` → Public o Private → Create.

**2. Puja els fitxers**
- Opció fàcil (sense terminal): a la pàgina del repo nou → "uploading an existing file" → arrossega tots els fitxers d'aquest paquet → Commit.
- Opció amb terminal:
  ```bash
  cd /ruta/al/paquet
  git init
  git add .
  git commit -m "Acoll-IA initial deploy"
  git branch -M main
  git remote add origin https://github.com/EL_TEU_USUARI/acoll-ia.git
  git push -u origin main
  ```

**3. Connecta Vercel**
- Vés a https://vercel.com → "Sign up" amb el teu compte de GitHub.
- "Add New..." → "Project" → tria el repo `acoll-ia` → "Import".
- A la pantalla de configuració:
  - **Framework Preset**: Other
  - **Root Directory**: `./` (per defecte)
  - **Build Command**: deixa en blanc
  - **Output Directory**: deixa en blanc
- Clica "Deploy". En 30 segons tens URL pública (tipus `acoll-ia.vercel.app`).

**4. (Opcional) Domini personalitzat**
- A Vercel: Project Settings → Domains → afegeix el teu domini si en tens un de l'institut.

### Actualitzacions futures

Quan vulguis canviar alguna cosa: edites el fitxer al teu ordinador, fas `git commit` + `git push` (o ho puges per la web de GitHub), i Vercel re-desplega sol en menys d'un minut.

## Alternativa ràpida (sense GitHub)

Si només vols veure-ho funcionant un moment sense configurar res:
1. Comprimeix la carpeta del paquet en ZIP.
2. Entra a https://vercel.com → arrossega el ZIP a "Deploy".
3. Tens URL pública immediatament.

Inconvenient: cada actualització has de tornar a arrossegar tot. Per això recomano GitHub per a ús real al centre.

## Verificació post-desplegament

Quan estigui en línia, comprova:

- [ ] La pàgina carrega i veus la pantalla de benvinguda.
- [ ] Obre DevTools (F12) → pestanya **Console** → no hi ha errors vermells.
- [ ] DevTools → **Application** → **Manifest**: veus el nom "Acoll-IA" i les icones.
- [ ] DevTools → **Application** → **Service Workers**: `sw.js` està registrat i actiu.
- [ ] En mòbil, opció "Afegir a la pantalla d'inici" disponible al navegador.
- [ ] Mode avió: l'app continua funcionant (gràcies al Service Worker).

## Notes tècniques

**Error corregit en el codi original:**
- Línia 5374: `throw new Error('Format no reconegut. El fitxer no és d'Acoll-IA.');`
- L'apòstrof catalán a `d'Acoll-IA` trencava la cadena (delimitada amb cometes simples). Canviat a cometes dobles.

**Dependències externes (CDN):**
- jsPDF 2.5.1 (cdnjs.cloudflare.com) — generació de diplomes PDF.
- OpenDyslexic font (fonts.cdnfonts.com) — accessibilitat.

Aquestes dependències es cachegen al Service Worker després de la primera visita, així que l'app funciona offline a partir de la segona càrrega.

**Permisos del navegador:**
La política `Permissions-Policy` de `vercel.json` permet `microphone=(self)` perquè la funcionalitat de reconeixement de veu (si està implementada) funcioni en el mateix origen.

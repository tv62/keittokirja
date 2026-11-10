# Perheen keittokirja – ohjeet Claudelle

Tämä tiedosto on keittokirjan ajantasainen kuvaus. **Päivitä se samassa commitissa aina, kun muutat
sovellusta, tietokantaa tai sääntöjä.** Vastaa käyttäjälle suomeksi.

> Repo on JULKINEN. Älä koskaan kirjoita tänne (tai mihinkään repon tiedostoon) sähköpostiosoitteita,
> salasanoja, avaimia (paitsi selaimen julkinen avain, joka on jo sivulla), reseptejä tai muuta perheen dataa.

## Yleiskuva

- **Sivu:** https://tv62.github.io/keittokirja/keittokirja.html (GitHub Pages, julkaisu `main`-haarasta)
- **Koodi:** yksi tiedosto `keittokirja.html` (HTML + CSS + vanilla JS). Ei build-vaihetta.
- **Data ja kirjautuminen:** Supabase-projekti `keittokirja`, ref `uzeaweqwthoofbmodutf`, alue eu-north-1, ilmaistaso.
- **Kirjasto:** `@supabase/supabase-js@2.45.4` (UMD, jsDelivr). Sivulla vain julkinen publishable-avain.
- `.nojekyll` pitää GitHub Pagesin julkaisemassa tiedostot sellaisenaan. Älä poista.
- Käyttäjä käyttää enimmäkseen iPadia ja iPhonea (Safari). Suunnittele mobiili edellä.

## Käyttäjä ja tehdyt päätökset

- Keittokirja on perheen omaan käyttöön. Ylläpitäjä (ainoa admin) on repon omistaja.
  Hän ei ole ohjelmoija: anna ohjeet vaiheittain ja selkokielellä, mieluiten iPadin Safarin näkökulmasta.
  Hän lähettää usein kuvakaappauksia, joten neuvo niiden perusteella.
- **Kaikki ilmaiseksi:** GitHub Free, GitHub Pages, Supabase Free. Älä ehdota maksullisia palveluita ilman painavaa syytä.
- **Yksityisyys:** reseptit, menut ja viinit vain perheelle (RLS). Julkinen repo on hyväksytty, koska vain koodi on julkista.
- **Käyttäjä haluaa, että Claude tekee muutokset suoraan** (Supabase + push GitHubiin), ei tiedostojen siirtelyä.
- **Käyttöliittymäpäätökset, joita ei muuteta kysymättä:**
  - Etusivulla neljä korttia: 🥘 Arkiruoka, 🥂 Viikonloppuherkut, 📜 Menut, 🍷 Viinit.
  - Päärajaukset aina näkyvissä, pikatagit piilossa "🏷 Tagit" -napin takana.
  - Top-listan ⭐ reseptirivin oikeassa laidassa. Menumerkki rivin oikeassa laidassa ja reseptin oikeassa yläkulmassa.
  - Kuvitetut menut näytetään täsmälleen suunnitellun näköisinä. Ei uudelleenpiirretä.
  - Viineissä rinnakkain arvio lähteestä (teksti + lähde + linkki) ja oma arvio 1–5 tähteä.
- **Reseptilähteet:**
  - Apple Muistiinpanot: vienti Pikakomennoilla (Etsi muistiinpanot → Yhdistä teksti, erotin `=====` → Tallenna tiedosto)
    ja tiedosto chattiin. iCloud-jakolinkkejä ei voi lukea.
  - Verkkosivut (esim. Yhteishyvä, HS): linkkejä ei yleensä voi lukea, joten pyydä käyttäjää liittämään teksti.
  - Claude Design -canvasit (menukortit ja viinilistat): ks. "Kuvitetun menun tuominen".

## Työn tila (päivitä, kun jokin muuttuu)

- Reseptit: Lapponia alla piemontese -menun 3 reseptiä (viikonloppuherkut) ja Apple Muistiinpanojen arkiruoat, erä 1 (11 reseptiä).
- Seuraavaksi todennäköisesti: viikonloppuherkut Apple Muistiinpanoista, uudet menut Claude Designista.
- Viinikirjassa 3 viiniä menun viinilistalta. Juontipäivä, omat tähdet ja lähteen arvio puuttuvat.
- Top-lista on vielä tyhjä (käyttäjä valitsee suosikit).
- "Tonnikalapasta liedellä": alkuperäisessä luki "noin 50 kapriksia". Mahdollisesti 50 g, käyttäjä ei ole vahvistanut.

## Työnkulku uudessa keskustelussa

1. Liitä repo: `add_repo` (owner `tv62`, repo `keittokirja`, access `push`), kloonaa ja lue tämä tiedosto.
2. Datamuutokset (reseptit, viinit, menut) tehdään suoraan Supabaseen MCP-työkaluilla (`execute_sql`).
   Niihin ei tarvita GitHubia.
3. Sivun muutokset: muokkaa `keittokirja.html`, testaa, commit + push `main`-haaraan.
   GitHub Pages julkaisee 1–3 minuutissa. Commitin tekijä: `tv62 <tv62@users.noreply.github.com>`.
4. Skeemamuutokset `apply_migration`-työkalulla, sen jälkeen `get_advisors` (security). Päivitä tämä tiedosto.

### Testaus ennen julkaisua

- `node --check` sivun `<script>`-lohkolle.
- Playwright (esiasennettu): korvaa supabase-js-skripti stubilla, joka palauttaa testidataa
  (`createClient` → `auth.onAuthStateChange`, `auth.getSession`, `from(t).select().order()/eq()`,
  `insert/update/delete`, `rpc`). Aja 390×844-näkymässä ja katso kuvakaappaukset.
- Reitityksen testaus: `page.go_back()` = takaisin-pyyhkäisy. Huom. `page.goto(sama_url + "#/…")` on vain
  #-osan vaihto (ei uudelleenlatausta). Oikeaan uudelleenlataukseen `goto("about:blank")` ensin tai `reload()`.
  Vahvistusikkunat (`confirm`) käsitellään `page.on("dialog", …)`.
- Headless-Chromium ei anna Wake Lockia, joten kokkaustila näyttää siinä "ei pysty pitämään näyttöä päällä".
- Google Fonts ei lataudu testiympäristössä (verkkoesto); ne virheet voi ohittaa.

## Sovelluksen rakenne (keittokirja.html)

Näkymät ovat `<section>`-elementtejä, joita `show(view)` vaihtaa (lista `VIEWS`):
`loadView`, `authView`, `homeView`, `listView`, `detailView`, `editView`, `adminView`, `menusView`, `menuView`,
`winesView`, `wineView`, `wineEditView`. Lisää uusi näkymä `VIEWS`-listaan ja reititykseen.

### Osoitteet ja takaisin-ele (reititys)

Jokaisella näkymällä on oma `#/`-osoite, joten iPhonen pyyhkäisy taaksepäin, sivun päivitys ja linkin jakaminen toimivat.

| Osoite | Näkymä |
|---|---|
| `#/` | etusivu |
| `#/reseptit`, `#/reseptit/arkiruoka`, `#/reseptit/viikonloppuherkut` | reseptilista |
| `#/resepti/<id>`, `#/resepti/<id>/muokkaa` | resepti, muokkaus |
| `#/uusi-resepti`, `#/uusi-resepti/<kategoria>` | uusi resepti |
| `#/menut`, `#/menut/<nimi, encodeURIComponent>` | menut, yksi menu |
| `#/viinit`, `#/viinit/<id>`, `#/viinit/<id>/muokkaa`, `#/uusi-viini` | viinit |
| `#/hallinta` | käyttäjien hallinta (vain admin) |

- **Siirtymät aina `go(hash)`-funktiolla** (pushState + `route()`), ei `show()`:lla suoraan. `go(hash, {replace:true})`
  korvaa nykyisen historiarivin (esim. uusi resepti tallennettu → lomake korvataan reseptillä).
- **Takaisin-napit `back(fallback)`:** `history.back()`, jos sovelluksen sisällä on edellinen sivu (`history.state.d > 0`),
  muuten mennään `fallback`-osoitteeseen (suora linkki). Poiston jälkeen `leaveDeleted()` = `history.go(-2)`.
- `route()` lukee osoitteen ja kutsuu `renderRoute()`:a. Tuntematon tai poistettu kohde → etusivu.
  Osoitteet, jotka eivät ala `#/` (esim. Supabasen `#access_token=…`), tulkitaan etusivuksi.
- Vierityskohta tallennetaan `history.state.y`:hyn ja palautetaan takaisin tultaessa.
- Listan rajaukset (kategoria, päärajaus, tagi, haku) säilyvät, kun reseptistä palataan. Ne nollataan,
  kun lista avataan etusivun kortista (`resetListFilters()`). Sama viineissä (`wineType`, `wq`).
- Data (reseptit, menut, viinit, admin-tieto) ladataan kerralla `loadAll()`-funktiossa ennen ensimmäistä `route()`-kutsua.

### Näkymät

- **Etusivu:** hakukenttä (`#hq`) ja kortit Arkiruoka, Viikonloppuherkut (`data-cat`), Menut ja Viinit (`data-nav`).
  Haku etsii resepteistä (nimi, kuvaus, ainekset, tagit, menu), menuista ja viineistä. Haun aikana kortit
  piilotetaan ja tulokset näytetään ryhmiteltynä. Haku säilyy, kun tuloksesta palataan. Otsikko "🍲 Keittokirja"
  vie etusivulle ja tyhjentää haun. `data-home`-elementti = takaisin etusivulle.
- **Haku kaikkialla:** `words()` + `matches()`: jokaisen hakusanan pitää löytyä (esim. "kana pasta").
- **Reseptilista:** päärajaukset `PRIMARY`-vakiossa kategorioittain, pikatagit piilossa "🏷 Tagit" -napin takana.
  "Menut"-rajaus (`@menu`) ryhmittelee menuittain, järjestys `COURSE_ORDER` = alkuruoka → illallinen → makeat.
- **Reseptirivi** (`recipeItem`): menumerkki ja ⭐ (top) rivin oikeassa laidassa.
- **Resepti:** menumerkki oikeassa yläkulmassa avaa menun. Napit "⭐ Lisää Top-listalle" ja "Muokkaa"
  (Poista on lomakkeen alaosassa, ettei sitä paina vahingossa).
  - **Annosmäärä:** −/+ -napit (vain jos `servings` on annettu). `scaleLine()` kertoo ainesrivin kaikki luvut
    (`1/2`, `½`, `1,5`, `2–3`, sulkeissa olevat `(400 g)`), mutta ei prosentteja eikä asteita. Kerrotut luvut
    korostetaan. Väliotsikoita (rivi päättyy `:`) ei muuteta. Ohjeen tekstin määriä ei muuteta, ja sivu kertoo sen.
  - **Kokkaustila:** "🍳 Aloita kokkaus" pitää näytön päällä (Screen Wake Lock API, haetaan uudelleen kun sovellus
    palaa näkyviin) ja aineksen tai vaiheen voi napauttaa tehdyksi (yliviivaus). Ensimmäinen tekemätön vaihe korostetaan.
  - Annosmäärä ja kokkaustilan rastit säilyvät muistissa (`cook`-Map) reseptikohtaisesti, kunnes sivu ladataan
    uudelleen, kokkaus lopetetaan tai resepti tallennetaan.
- **Reseptilomake:** tagit valitaan napautettavista chipeistä (`editTags`). Valitun kategorian päärajaukset ensin,
  sitten muut käytössä olevat tagit aakkosjärjestyksessä. Uuden tagin voi kirjoittaa (pilkulla useita) ja lisätä
  "Lisää"-napilla tai Enterillä. Kirjoitettu mutta lisäämätön tagi tallentuu myös. Metatageja ei voi lisätä käsin.
- **Tallentamattomat muutokset:** resepti- ja viinilomake ottavat avattaessa tilannekuvan (`editSnap`, `wineSnap`).
  Peruuta, takaisin-pyyhkäisy ja muut siirtymät kysyvät "Hylätäänkö tallentamattomat muutokset?", jos lomaketta on muutettu.
- **Hallinta** (vain admin): perheen pääsylista RPC-funktioiden kautta.
- **Menut:** menukortti- ja viinilistasivut välilehtinä, alla menun reseptit.
- **Viinit:** lista (haku, tyyppirajaus, omat tähdet), viinin sivu (Muokkaa), lomake (tähtivalitsin 1–5, Poista alaosassa).
- Napautettavat chipit ovat vähintään 44 px korkeita (sormelle sopiva koko).
- Käyttäjän syöte escapetaan aina `esc()`-funktiolla ennen `innerHTML`:ää. URLit `safeUrl()`-funktiolla.

## Tietokanta (Supabase)

Kaikissa julkisissa tauluissa on RLS. Säännöt: select/insert/update/delete `to authenticated` ehdolla
`(select private.is_family())`. **Uudelle taululle samat neljä policya + `revoke all ... from anon`.**

| Taulu | Sarakkeet |
|---|---|
| `recipes` | id, title, description, servings, prep_minutes, ingredients (jsonb, lista tekstirivejä), instructions (jsonb, lista vaiheita), tags (text[]), source, image_url, notes, created_by, created_at, updated_at |
| `menus` | id, name (uniikki, = reseptien `menu:`-tagin nimi), subtitle, description, sort |
| `menu_pages` | id, menu_id, kind (`menu`/`viinilista`/`muu`), title, html, width, height, sort |
| `wines` | id, name, producer, vintage, wine_type (`punainen`/`valkoinen`/`rosé`/`kuohuviini`/`jälkiruokaviini`/`muu`), grapes, region, country, drunk_on (date), occasion, price, source_rating (teksti, esim. "92/100"), source_name, source_url, own_rating (1–5), own_notes, description, food_pairing, image_url |

Yksityinen skeema `private` (ei API:ssa):
- `family_members(email, is_admin, added_at)` – pääsylista. Vain listalla olevat sähköpostit näkevät datan.
- `is_family()`, `is_admin()` – security definer -apufunktiot RLS:ää varten.
- `touch_updated_at()` – trigger.

RPC-funktiot (public, security definer, tarkistavat `is_admin()`:n itse): `am_i_admin`, `admin_list_users`,
`admin_add_member`, `admin_remove_member` (ei omaa), `admin_set_admin` (ei omia oikeuksia pois).
Advisorin varoitus "authenticated security definer function executable" näistä on tarkoituksellinen.

Rekisteröityminen on avoin, mutta ilman pääsylistaa tunnus ei näe mitään. Auth-asetuksissa Site URL ja
Redirect URL = sivun osoite.

## Tagisäännöt (recipes.tags)

- **Kategoria (yksi):** `arkiruoka` tai `viikonloppuherkut`
- **Top-lista:** `top`
- **Menu:** `menu:<Menun nimi>` (iso alkukirjain säilyy, sama kuin `menus.name`)
- **Päärajaukset:** arki = `top`, `kana`, `nauta`, `kasvis`; viikonloppu = `top`, `alkuruoka`, `illallinen`, `makeat`, `pasta`
- **Muut tagit** pienillä kirjaimilla: esim. `kala`, `keitto`, `uuniruoka`, `pasta`, `poro`, `feta`, `linssi`, `marjat`
- Kategoria-, top- ja menu-tagit ovat "metatageja" (`isMeta`), eikä niitä näytetä tavallisina tageina.

## Reseptien lisääminen

- Säilytä ainekset ja ohjeet sisällöltään ennallaan. Siisti numerointi ja muotoilu, korjaa kirjoitusvirheet.
- Jos vaiheissa mainittu aines (esim. vesi) puuttuu luettelosta, lisää se luetteloon.
- Väliotsikot aineslistassa omana rivinään, esim. `"Kastike:"`.
- `prep_minutes` = valmistusaika ilman marinointia tai hyytymistä. Mainitse ne `notes`-kentässä.
- Jos kategoriaa ei kerrota, päättele se ja kerro valinta. Lisää sopivat päärajaustagit (kana/nauta/kasvis jne.).
- Pelkkä linkki → tallenna `source` ja pyydä käyttäjää liittämään ohjeen teksti (iCloud ja monet sivut estävät lukemisen).
- Käytä SQL:ssä dollar-quotingia (`$t$...$t$`, `$j$[...]$j$::jsonb`).
- Raportoi lopuksi lyhyesti, mitä lisättiin ja mitä tulkintoja tehtiin.

## Kuvitetun menun tuominen (Claude Design -canvas)

1. Lue canvasin tiedostot Artifact-työkalulla (`project/canvas.json` ja artboardit `project/*.dc.html`).
2. Muunna jokainen artboard tavalliseksi HTML:ksi: `<helmet>`-sisältö `<head>`-osaan, `<x-dc>` puretaan,
   kaikki `<script>`-tagit pois, lisätään `<style>html,body{margin:0;overflow:hidden}</style>`.
   Kuvat upotetaan data-URI:na. Pakkaa PNG (esim. PIL `quantize(16)`), jotta SQL-lause pysyy pienenä.
3. Lisää `menus`-rivi (name = sama kuin reseptien `menu:`-tagi) ja `menu_pages`-rivit
   (kind, title, html, width/height = artboardin koko, sort). Tarkista `md5(html)` paikalliseen tiedostoon.
4. Sivut näytetään `<iframe sandbox="" srcdoc=…>`-kehyksessä skaalattuna näytön levyiseksi. Skriptit eivät aja.
5. Menun reseptit linkittyvät `menu:<nimi>`-tagilla. Viinilistan viinit voi lisätä myös `wines`-tauluun.

Nykyiset menut: **Lapponia alla piemontese** (menukortti + viinilista, 3 reseptiä).

## Herätys (Supabase ei pausetu)

- Supabase pausettaa ilmaisprojektin, jos käyttöä ei ole 7 päivään. Estetään GitHub Actionsilla:
  `.github/workflows/supabase-heratys.yml` kutsuu päivittäin RPC:tä `public.keepalive()` julkisella avaimella.
- `keepalive()` on security definer ja anonin kutsuttavissa. Se päivittää rivin `private.heartbeat`
  (korkeintaan kerran tunnissa) ja palauttaa vain kellonajan, ei dataa. Advisorin varoitus siitä on tarkoituksellinen.
- Viimeisimmän herätyksen näkee kyselyllä `select * from private.heartbeat;`.
- Jos repoon ei tule committeja 45 päivään, workflow päivittää `.github/heratys.txt`-tiedoston, koska GitHub
  poistaa ajastukset käytöstä julkisissa repoissa 60 päivän hiljaisuuden jälkeen.
- Jos projekti on silti pausella: Supabasen hallintapaneeli → "Resume project". Data säilyy.

## Tunnetut asiat ja ideat

- "Leaked password protection" ei ole päällä (saattaa vaatia maksullisen Supabase-tason).
- Kuvien lataus (Supabase Storage) puuttuu. Kuvat ovat nyt URL-osoitteita.
  **Seuraavaksi sovittu:** kuvat suoraan kameralla (Supabase Storage, pikkukuvat listaan).
- Ehdotettuja, ei vielä tehty (ulkoasu): ainesten väliotsikot ("Kastike:") otsikoiksi; "Hallinta" ja
  "Kirjaudu ulos" vievät iPhonella kaksi riviä (uloskirjautuminen Hallinta-sivulle?); iPadilla vaakatasossa
  ainekset ja ohje rinnakkain; ohjeen vaiheisiin isommat numerot; oma Koti-valikon kuvake (apple-touch-icon).
- Myöhemmin ehkä: toimii ilman verkkoa (service worker), ostoslista Muistutuksiin, ajastin ohjeen aikoihin,
  viinien ja reseptien yhdistäminen ruokapareiksi.

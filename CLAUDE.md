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
- **Oikeudet:** uusi jäsen saa oletuksena vain lukea. Admin antaa muokkausoikeuden Hallinta-sivulla. Admin saa aina muokata.
- **Käyttäjä haluaa, että Claude tekee muutokset suoraan** (Supabase + push GitHubiin), ei tiedostojen siirtelyä.
- **Käyttöliittymäpäätökset, joita ei muuteta kysymättä:**
  - Etusivulla neljä korttia: 🥘 Arkiruoka, 🥂 Viikonloppuherkut, 📜 Menut, 🍷 Viinit.
  - Päärajaukset aina näkyvissä, pikatagit piilossa "🏷 Tagit" -napin takana.
  - Top-listan ⭐ reseptirivin oikeassa laidassa. Menumerkki rivin oikeassa laidassa ja reseptin oikeassa yläkulmassa.
  - Kuvitetut menut näytetään täsmälleen suunnitellun näköisinä. Ei uudelleenpiirretä.
  - Viineissä rinnakkain arvio lähteestä (teksti + lähde + linkki) ja oma arvio 1–5 tähteä.
  - Resepti- ja viinikuvat aina neliöitä ja värillisiä. Listalla pikkukuva rivin vasemmassa laidassa (ilman kuvaa nimen
    alkukirjain). Puhelimessa kuva sivun lopussa (ennen nappeja), iPadilla/isolla näytöllä ylhäällä.
    (Tyylittelyä kokeiltiin: piirros-, akvarelli- ja mustavalkotyylit hylättiin, värikuva valittiin.)
- **Reseptilähteet:**
  - Apple Muistiinpanot: vienti Pikakomennoilla (Etsi muistiinpanot → Yhdistä teksti, erotin `=====` → Tallenna tiedosto)
    ja tiedosto chattiin. iCloud-jakolinkkejä ei voi lukea.
  - Verkkosivut (esim. Yhteishyvä, HS): linkkejä ei yleensä voi lukea, joten pyydä käyttäjää liittämään teksti.
  - Claude Design -canvasit (menukortit ja viinilistat): ks. "Kuvitetun menun tuominen".

## Työn tila (päivitä, kun jokin muuttuu)

- Reseptit: Lapponia alla piemontese -menun 3 reseptiä (viikonloppuherkut) ja Apple Muistiinpanojen arkiruoat, erä 1 (11 reseptiä),
  sekä yksittäin lisättyjä arkiruokia ja lisukkeita.
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
- Kuvat: stubiin `storage.from(b).upload/createSignedUrls/remove` (upload → `URL.createObjectURL(blob)`).
  Kuvan valinta `page.set_input_files("#f_file" / "#w_file", polku)`, tila `recipePic.state` / `winePic.state`. Ennen rajauksen raahausta `scroll_into_view_if_needed()`,
  muuten hiiri osuu näkymän ulkopuolelle. Kuvien koon voi tarkistaa `createImageBitmap`-funktiolla.
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
- **Kuvat (reseptit ja viinit):** `imgUrl(item, small)`, `thumbHtml(item, nimi)` (listan pikkukuva `.thumb` 60 px tai
  alkukirjain `.thumb.ph`, rivillä luokka `.item.withpic`) ja `bigPic(item, alt, "rimg-top"|"rimg-end")`. Iso kuva
  renderöidään kahteen kohtaan, ja CSS näyttää alle 600 px leveällä sivun lopussa (ennen nappeja), muuten ylhäällä
  (max 440 px). Vanha `image_url` toimii varalla, jos `image_path` puuttuu.
- **Kuvan lisäys lomakkeessa:** yhteinen `imagePicker(p)` (`recipePic` = "f", `winePic` = "w"; elementit
  `<p>_imgprev, _cropwrap, _crop, _zoom, _imgnote, _imgpick, _imgundo, _imgdel, _file`). "📷 Lisää kuva" on
  `<label for="<p>_file">`, `accept="image/*"` → iPhonessa kamera tai kuvakirjasto. Neliörajaus canvasilla: siirto
  sormella, suurennus liukusäätimellä (1–3×) tai kahdella sormella, oletuksena keskeltä. `pic.upload(kansio)`:
  1200×1200 JPEG (laatu 0,85, n. 350 kt) ja 240×240 pikkukuva (n. 16 kt), pienennys vaiheittain.
  Tallennus: `applyPic()` ennen rivin tallennusta, `finishPic()` sen jälkeen (vanha kuva poistetaan vasta onnistuneen
  tallennuksen jälkeen; epäonnistunut tallennus poistaa juuri ladatut). Rivin poisto poistaa myös kuvat.
  Tila `pic.kind`: `none` / `keep` / `new` / `removed` (Peru palauttaa edellisen). Kuvan valinta tekee lomakkeesta muutetun.
- **Reseptilomake:** tagit valitaan napautettavista chipeistä (`editTags`). Valitun kategorian päärajaukset ensin,
  sitten muut käytössä olevat tagit aakkosjärjestyksessä. Uuden tagin voi kirjoittaa (pilkulla useita) ja lisätä
  "Lisää"-napilla tai Enterillä. Kirjoitettu mutta lisäämätön tagi tallentuu myös. Metatageja ei voi lisätä käsin.
- **Tallentamattomat muutokset:** resepti- ja viinilomake ottavat avattaessa tilannekuvan (`editSnap`, `wineSnap`).
  Peruuta, takaisin-pyyhkäisy ja muut siirtymät kysyvät "Hylätäänkö tallentamattomat muutokset?", jos lomaketta on muutettu.
- **Hallinta** (vain admin): perheen pääsylista RPC-funktioiden kautta. Jokaisella rivillä oikeusmerkki
  (admin / saa muokata / vain luku / ei pääsyä) ja napit Salli muokkaus / Poista muokkausoikeus, Tee adminiksi /
  Poista admin, Poista pääsy. Lisäyslomakkeessa "Saa lisätä ja muokata" on oletuksena pois (admin-ruutu valitsee
  ja lukitsee sen). "Anna lukuoikeus" listalta puuttuvalle = vain luku.
- **Vain lukuoikeus** (`canEdit` = RPC `can_i_edit`): `body.readonly` piilottaa `.edit-only`-napit (+ Uusi).
  Reseptin Top- ja Muokkaa-napit sekä viinin Muokkaa jätetään renderöimättä. Lomakkeiden osoitteet
  (`…/muokkaa`, `#/uusi-resepti`, `#/uusi-viini`) ohjautuvat etusivulle. Annosmäärä ja kokkaustila toimivat kaikille.
  Tietokanta estää muokkauksen joka tapauksessa (RLS), käyttöliittymä vain siistii näkymän.
- **Menut:** menukortti- ja viinilistasivut välilehtinä, alla menun reseptit.
- **Viinit:** lista (pikkukuva, haku, tyyppirajaus, omat tähdet), viinin sivu (kuva, Muokkaa), lomake (kuva,
  tähtivalitsin 1–5, Poista alaosassa).
- Napautettavat chipit ovat vähintään 44 px korkeita (sormelle sopiva koko).
- Käyttäjän syöte escapetaan aina `esc()`-funktiolla ennen `innerHTML`:ää. URLit `safeUrl()`-funktiolla.

## Tietokanta (Supabase)

Kaikissa julkisissa tauluissa on RLS. Säännöt `to authenticated`:
- **select** ehdolla `(select private.is_family())` – kaikki perheen jäsenet näkevät.
- **insert/update/delete** ehdolla `(select private.can_edit())` – vain muokkausoikeus tai admin.

**Uudelle taululle samat neljä policya (lukeminen is_family, kirjoitus can_edit) + `revoke all ... from anon`.**

**Storage:** yksityinen bucket `recipe-images` (vain `image/jpeg`, max 5 Mt). `storage.objects`-säännöt samalla
mallilla: select `is_family()`, insert/update/delete `can_edit()` (ehtona `bucket_id = 'recipe-images'`).
Reseptit `<uuid>.jpg` (1200×1200) ja `<uuid>-pk.jpg` (240×240), viinit samoin kansiossa `viinit/`. Sivu hakee allekirjoitetut osoitteet
(`createSignedUrls`, voimassa 7 vrk) kaikille kuville kerralla `loadAll()`:ssa (`signImages()`).
Ilmaistasossa 1 Gt tilaa ja 5 Gt siirtoa kuukaudessa riittävät tuhansille kuville.

| Taulu | Sarakkeet |
|---|---|
| `recipes` | id, title, description, servings, prep_minutes, ingredients (jsonb, lista tekstirivejä), instructions (jsonb, lista vaiheita), tags (text[]), source, image_path (Storage-polku, esim. `<uuid>.jpg`), image_url (vanha ulkoinen osoite, ei enää lomakkeessa), notes, created_by, created_at, updated_at |
| `menus` | id, name (uniikki, = reseptien `menu:`-tagin nimi), subtitle, description, sort |
| `menu_pages` | id, menu_id, kind (`menu`/`viinilista`/`muu`), title, html, width, height, sort |
| `wines` | id, name, producer, vintage, wine_type (`punainen`/`valkoinen`/`rosé`/`kuohuviini`/`jälkiruokaviini`/`muu`), grapes, region, country, drunk_on (date), occasion, price, source_rating (teksti, esim. "92/100"), source_name, source_url, own_rating (1–5), own_notes, description, food_pairing, image_path (esim. `viinit/<uuid>.jpg`), image_url (vanha, ei lomakkeessa) |

Yksityinen skeema `private` (ei API:ssa):
- `family_members(email, is_admin, can_edit, added_at)` – pääsylista. Vain listalla olevat sähköpostit näkevät datan.
  `can_edit` oletus `false` (vain luku). Admin saa muokata `can_edit`-arvosta riippumatta.
- `is_family()`, `is_admin()`, `can_edit()` (= is_admin or can_edit) – security definer -apufunktiot RLS:ää varten.
- `touch_updated_at()` – trigger.

RPC-funktiot (public, security definer):
- `am_i_admin()`, `can_i_edit()` – kirjautuneen omat oikeudet (sivu piilottaa napit niiden mukaan).
- Vain admin (tarkistavat `is_admin()`:n itse): `admin_list_users()` (palauttaa myös `can_edit`, adminilla aina true),
  `admin_add_member(p_email, p_admin = false, p_can_edit = false)` (ei omia admin-oikeuksia pois),
  `admin_set_editor(p_email, p_can_edit)`, `admin_remove_member(p_email)` (ei omaa),
  `admin_set_admin(p_email, p_admin)` (ei omia oikeuksia pois; adminiksi tehty saa `can_edit = true`, joka jää voimaan
  admin-oikeuden poistuessa).
- `backup_family_members()` – pääsylista varmuuskopiota varten. Vain `service_role` (salainen avain) voi kutsua.
- Advisorin varoitus "authenticated security definer function executable" näistä on tarkoituksellinen.
- RLS-testaus SQL:llä: `do $$ … insert testijäsen; set_config('request.jwt.claims', …); set local role authenticated;
  … raise exception 'TULOS …' $$` – lopun virhe peruu kaiken, ja tulos näkyy virheviestissä.

Rekisteröityminen on avoin, mutta ilman pääsylistaa tunnus ei näe mitään. Auth-asetuksissa Site URL ja
Redirect URL = sivun osoite.

## Tagisäännöt (recipes.tags)

- **Kategoria (yksi):** `arkiruoka` tai `viikonloppuherkut`
- **Top-lista:** `top`
- **Menu:** `menu:<Menun nimi>` (iso alkukirjain säilyy, sama kuin `menus.name`)
- **Päärajaukset:** arki = `top`, `kana`, `nauta`, `kasvis`, `lisuke` (otsikko "Lisukkeet"); viikonloppu = `top`, `alkuruoka`, `illallinen`, `makeat`, `pasta`
- **Muut tagit** pienillä kirjaimilla: esim. `kala`, `keitto`, `uuniruoka`, `pasta`, `poro`, `feta`, `linssi`, `marjat`, `peruna`
- Lisukkeet (perunat, salaatit ym.) saavat `lisuke`-tagin. Kasvisruoka-lisuke voi saada myös `kasvis`-tagin.
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

## Varmuuskopiot

Supabase ei varmuuskopioi ilmaistason projektia, ja tietokannan varmuuskopio ei sisältäisi Storagen kuvia.

- **Automaattinen:** yksityinen repo `tv62/keittokirja-varmuuskopio` (EI julkinen, sisältää perheen datan).
  GitHub Actions (`.github/workflows/varmuuskopio.yml`) ajaa `varmuuskopioi.py`:n sunnuntaisin 01.37 UTC, käsin
  (Actions → Varmuuskopio → Run workflow) ja aina kun ohjelma tai workflow muuttuu.
  - Hakee REST-rajapinnasta taulut `recipes`, `wines`, `menus`, `menu_pages` (→ `data/<taulu>.json`), RPC:llä
    `backup_family_members()` (→ `data/family_members.json`) ja bucketin `recipe-images` kuvat (→ `kuvat/`, vain uudet;
    poistetut poistetaan, historia säilyttää). Lisäksi `viimeisin.txt`, jolloin jokainen ajo on oma committinsa.
  - Avain: repon salaisuus `SUPABASE_SECRET_KEY` (Supabasen salainen avain nimeltä "varmuuskopio", `sb_secret_…`).
    Lähetetään vain `apikey`-otsakkeessa. Ohjelma keskeyttää, jos avain ei ole salainen tai tulos olisi tyhjä.
  - Jos lisäät uuden taulun, lisää se `TABLES`-listaan `varmuuskopioi.py`:ssä.
  - Ajon tilan näkee: `gh run list -R tv62/keittokirja-varmuuskopio`. Repo pitää liittää sessioon (`add_repo`).
- **Pyynnöstä ("tee varmuuskopio"):** hae rivit `execute_sql`:llä (`row_to_json(x)::text`) ja per rivi `md5`,
  kirjoita tiedostoon ja tarkista md5:t (kopiointivirheiden varalta), kokoa JSON + luettava HTML ja lähetä käyttäjälle.
  Kuvia ei saa tällä tavalla mukaan. Helpompi: lue tuorein tila suoraan varmuuskopioreposta.
- **Palautus:** rivit ovat JSON-muodossa sellaisinaan → `insert … on conflict (id) do update` (`execute_sql`, dollar-quoting).
  Kuvat palautetaan bucketiin samoille poluille (vaatii salaisen avaimen tai käyttäjän lataamaan sivun kautta).

## Tunnetut asiat ja ideat

- "Leaked password protection" ei ole päällä (saattaa vaatia maksullisen Supabase-tason).
- Ehdotettuja, ei vielä tehty (ulkoasu): ainesten väliotsikot ("Kastike:") otsikoiksi; "Hallinta" ja
  "Kirjaudu ulos" vievät iPhonella kaksi riviä (uloskirjautuminen Hallinta-sivulle?); iPadilla vaakatasossa
  ainekset ja ohje rinnakkain; ohjeen vaiheisiin isommat numerot; oma Koti-valikon kuvake (apple-touch-icon).
- Myöhemmin ehkä: toimii ilman verkkoa (service worker), ostoslista Muistutuksiin, ajastin ohjeen aikoihin,
  viinien ja reseptien yhdistäminen ruokapareiksi.

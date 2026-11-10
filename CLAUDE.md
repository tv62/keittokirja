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
- **Kuvake** (valittu 11/2026, vaihtoehto E: punainen valurautapata sivulta valkoisella pohjalla, oma piirros, ei
  tuotemerkkiä): lähde `kuvake.svg` → `apple-touch-icon.png` (180, kotivalikko), `icon-192.png`, `favicon-32.png`.
  Uudelleen: `rsvg-convert -w 1024 -h 1024 kuvake.svg -o k.png` ja pienennys PIL:llä (LANCZOS). Läpinäkymätön, ei pyöristystä
  (iOS pyöristää). Jo tallennettu kotivalikon pikakuvake ei päivity: poista ja lisää uudelleen Safarin Jaa-valikosta.
  Otsikon logo `pata.svg` = sama pata läpinäkyvällä taustalla ilman varjoa, viewBox rajattu pataan (`h1 .logo`, 1.15em).
- Käyttäjä käyttää enimmäkseen iPadia ja iPhonea (Safari). Suunnittele mobiili edellä.

## Käyttäjä ja tehdyt päätökset

- Keittokirja on perheen omaan käyttöön. Ylläpitäjä (ainoa admin) on repon omistaja.
  Hän ei ole ohjelmoija: anna ohjeet vaiheittain ja selkokielellä, mieluiten iPadin Safarin näkökulmasta.
  Hän lähettää usein kuvakaappauksia, joten neuvo niiden perusteella.
- **Kaikki ilmaiseksi:** GitHub Free, GitHub Pages, Supabase Free. Älä ehdota maksullisia palveluita ilman painavaa syytä.
- **Yksityisyys:** reseptit, menut ja viinit vain perheelle (RLS). Julkinen repo on hyväksytty, koska vain koodi on julkista.
- **Oikeudet:** uusi jäsen saa oletuksena vain lukea. Admin antaa muokkausoikeuden Hallinta-sivulla. Admin saa aina muokata.
- **Osioiden näkyvyys (admin päättää, 11/2026):** admin voi piilottaa jäseneltä etusivun osioita (Arkiruoka, Viikonloppuherkut,
  Vauvan ruoat, Menut, Viinit) Hallinta-sivulla. Piilotus on täysi: kortti, osoitteet ja haku pois, ja RLS estää datan
  latautumisen. Admin näkee aina kaiken. **Uusi jäsen ei näe oletuksena mitään** (käyttäjän päätös 11/2026): admin avaa osiot
  Hallinta-sivulla. Ennen muutosta lisätyt jäsenet näkevät kaiken.
- **Käyttäjä haluaa, että Claude tekee muutokset suoraan** (Supabase + push GitHubiin), ei tiedostojen siirtelyä.
- **Käyttöliittymäpäätökset, joita ei muuteta kysymättä:**
  - Etusivulla viisi korttia: 🥘 Arkiruoka, 🥂 Viikonloppuherkut, 📜 Menut, 🍷 Viinit ja niiden alla oma osio
    🍼 Vauvan ruoat (luokka `wide`: iPadilla molempien sarakkeiden levyinen).
  - **Arkiruoka avautuu valintasivuna** (hyväksytty 11/2026): 2 saraketta ruutuja (iPadilla 3), emoji + nimi + määrä:
    Top-lista, Kana, Liha, Poro, Kala, Kasvis, Keitot, Pastat, Lisukkeet, Kastikkeet + leveä "Kaikki arkiruoat".
    Tyhjät ryhmät piilotetaan. Ryhmän listassa ylhäällä "← Arkiruoka" ja päärajaukset yhdellä pyyhkäistävällä rivillä.
  - Muissa kategorioissa päärajaukset aina näkyvissä. Pikatagit aina piilossa "🏷 Tagit" -napin takana.
  - Top-listan ⭐ reseptirivin oikeassa laidassa. Menumerkki rivin oikeassa laidassa ja reseptin oikeassa yläkulmassa.
  - Kuvitetut menut näytetään täsmälleen suunnitellun näköisinä. Ei uudelleenpiirretä.
  - Viineissä rinnakkain arvio lähteestä (teksti + lähde + linkki) ja oma arvio 1–5 tähteä.
  - Resepti- ja viinikuvat aina neliöitä ja värillisiä. Listalla pikkukuva rivin vasemmassa laidassa (ilman kuvaa nimen
    alkukirjain). Puhelimessa kuva sivun lopussa (ennen nappeja), iPadilla/isolla näytöllä ylhäällä.
    (Tyylittelyä kokeiltiin: piirros-, akvarelli- ja mustavalkotyylit hylättiin, värikuva valittiin.)
- **Reseptilähteet:**
  - Apple Muistiinpanot: vienti Pikakomennoilla (Etsi muistiinpanot → Yhdistä teksti, erotin `=====` → Tallenna tiedosto)
    ja tiedosto chattiin. iCloud-jakolinkkejä ei voi lukea.
    Muistiinpanojen kuvat eivät tule viennissä mukaan, eikä Claude voi ladata kuvia Storageen (ei avainta). Käyttäjä lisää ne
    itse: Muistiinpanoissa kuvaa pitkään → Kopioi → keittokirjassa Muokkaa → 📋 Liitä kuva (tai iPadilla raahaus).
  - Verkkosivut (esim. Yhteishyvä, HS): linkkejä ei yleensä voi lukea, joten pyydä käyttäjää liittämään teksti.
  - Claude Design -canvasit (menukortit ja viinilistat): ks. "Kuvitetun menun tuominen".

## Työn tila (päivitä, kun jokin muuttuu)

- Reseptit (n. 73): Lapponia alla piemontese -menun 3 reseptiä, Apple Muistiinpanojen arkiruoat erä 1 (11) ja
  erä 2 (marraskuu 2026: Keitot 6, Kastikkeet 9, Perjantaiherkut 15, Arkiruoka 28) sekä yksittäin lisättyjä.
  Muistiinpanojen kansiot: Keitot ja Kastikkeet → `arkiruoka` (+ `keitto` / `kastike`), Perjantaiherkut → `viikonloppuherkut`.
  Tyhjinä jätettiin pois: Hummus leivän päälle, Sitruunainen kantarellispagetti, Valkosipulimarinoidut
  lampaankyljykset, Nopea linssibolognese. Wokkikastikkeesta puuttuu ohje (vain ainekset).
- Vauvan ruoat: ei vielä omia reseptejä. 11 arkiruokaa on merkitty myös vauvalle sopiviksi (lisätagi `vauvanruoat`,
  vauvaohje `baby_notes`-kentässä).
  Päärajauksena vain Top-lista; muut (esim. ikä) sovitaan käyttäjän kanssa.
- Seuraavaksi todennäköisesti: puuttuvat ohjeet tyhjiin muistiinpanoihin, uudet menut Claude Designista.
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
  `insert/update/delete`, `rpc`; `my_hidden_sections` → taulukko, esim. `["viinit"]`). Aja 390×844-näkymässä ja katso kuvakaappaukset.
- Reitityksen testaus: `page.go_back()` = takaisin-pyyhkäisy. Huom. `page.goto(sama_url + "#/…")` on vain
  #-osan vaihto (ei uudelleenlatausta). Oikeaan uudelleenlataukseen `goto("about:blank")` ensin tai `reload()`.
  Vahvistusikkunat (`confirm`) käsitellään `page.on("dialog", …)`.
- Headless-Chromium ei anna Wake Lockia, joten kokkaustila näyttää siinä "ei pysty pitämään näyttöä päällä".
- Kuvat: stubiin `storage.from(b).upload/createSignedUrls/remove` (upload → `URL.createObjectURL(blob)`).
  Kuvan valinta `page.set_input_files("#f_file" / "#w_file", polku)`, tila `recipePic.state` / `winePic.state`. Ennen rajauksen raahausta `scroll_into_view_if_needed()`,
  muuten hiiri osuu näkymän ulkopuolelle. Kuvien koon voi tarkistaa `createImageBitmap`-funktiolla.
  Leikepöytä: tarjoa sivu `https://testi.local/…`-osoitteesta (`ctx.route` + `fulfill`, turvallinen konteksti) ja
  `ctx.grant_permissions(["clipboard-read","clipboard-write"], origin=…)`; kuva leikepöydälle `navigator.clipboard.write([new ClipboardItem(…)])`.
  Raahaus/⌘V: `new DataTransfer()` + `items.add(new File(…))` ja `DragEvent("dragenter"/"dragover"/"drop")` / `ClipboardEvent("paste")`.
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
| `#/reseptit`, `#/reseptit/viikonloppuherkut`, `#/reseptit/vauvanruoat` | reseptilista |
| `#/reseptit/arkiruoka`, `#/reseptit/arkiruoka/<ryhmä>`, `#/reseptit/arkiruoka/kaikki` | arkiruoan valintasivu, ryhmän lista |
| `#/resepti/<id>`, `#/resepti/<id>/muokkaa` | resepti, muokkaus |
| `#/resepti/<id>/vauva` | resepti vauvan ruoista avattuna (näyttää `baby_notes`) |
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

- **Etusivu:** hakukenttä (`#hq`) ja kortit Arkiruoka, Viikonloppuherkut, Vauvan ruoat (`data-cat`), Menut ja Viinit (`data-nav`).
  Haku etsii resepteistä (nimi, kuvaus, kategorian nimi, ainekset, tagit, menu), menuista ja viineistä. Haun aikana kortit
  piilotetaan ja tulokset näytetään ryhmiteltynä. Haku säilyy, kun tuloksesta palataan. Otsikko (pata-logo + "Keittokirja")
  vie etusivulle ja tyhjentää haun. `data-home`-elementti = takaisin etusivulle.
- **Haku kaikkialla:** `words()` + `matches()`: jokaisen hakusanan pitää löytyä (esim. "kana pasta").
- **Kategoriat** `CATS`-vakiossa. Uusi kategoria: `CATS` + `PRIMARY` (tai `GRID`) + etusivun kortti (`data-cat`, laskuri `cnt_<kategoria>`)
  + `<option>` lomakkeen `#f_category`-valintaan. Reititys, lista, haku ja lomake toimivat sen jälkeen automaattisesti.
- **Valintasivu (`GRID`):** `GRID[kategoria] = { all, groups: [[avain, emoji, otsikko, tagit]] }` (nyt vain arkiruoka).
  Ryhmään kuuluu resepti, jolla on mikä tahansa ryhmän tageista (`gridGroup()`). Ryhmä tulee osoitteesta:
  `openCategory(cat, group)` asettaa `activePrimary = group`. `renderList()`: `gridMode` = GRID-kategoria ilman ryhmää,
  hakua ja tagia → `#subGrid` näkyy, chipit ja lista piiloon. Haku valintasivulla näyttää suoraan listan.
  Ruutu → `go(.../<ryhmä>)` (uusi historiarivi), chip → `go(..., {replace:true})`, joten takaisin-ele vie aina valintasivulle.
  `#toGrid` ("← Arkiruoka") = `back(#/reseptit/<kategoria>)`; `#listHome` piilossa ryhmälistassa. Ryhmästä valintasivulle
  palatessa haku ja tagi nollataan; reseptistä palatessa ne säilyvät. Chip-rivi `.chips.scroll`, valittu keskitetään.
- **Reseptilista:** päärajaukset `PRIMARY`-vakiossa kategorioittain (GRID-kategorioissa ryhmät), pikatagit piilossa "🏷 Tagit" -napin takana.
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
  `<p>_imgbox, _imgprev, _cropwrap, _crop, _zoom, _imgnote, _imgpick, _imgpaste, _imgundo, _imgdel, _file`). "📷 Lisää kuva" on
  `<label for="<p>_file">`, `accept="image/*"` → iPhonessa kamera tai kuvakirjasto. Kaikki lähteet → `loadBlob(blob)` → rajaus.
  - **📋 Liitä kuva** (`_imgpaste`, 11/2026, esim. Apple Muistiinpanojen kuville: kuvaa pitkään → Kopioi): `navigator.clipboard.read()`
    kutsutaan suoraan napautuksessa (Safari vaatii; iOS näyttää vielä "Liitä"-kuplan). Nappi näkyy vain, jos selain tukee.
    Ei kuvaa → punainen ohje `_imgnote`:ssa (`.hint.err`). Myös ⌘V/Ctrl+V lomakkeessa (`paste`-tapahtuma, vain jos lomake auki;
    tekstikenttään liitetty teksti ohitetaan).
  - **Raahaus** (iPad: Muistiinpanot ja Safari rinnakkain): kuvatiedoston voi pudottaa mihin tahansa lomakkeen `<section>`iin.
    Raahatessa luokka `.dropon` (katkoviivareunus + kiinteä "Pudota kuva tähän" -kupla). Tekstin raahaus kenttiin ennallaan
    (vain `Files`/`image/*`-tyypit otetaan kiinni).
  Neliörajaus canvasilla: siirto
  sormella, suurennus liukusäätimellä (1–3×) tai kahdella sormella, oletuksena keskeltä. `pic.upload(kansio)`:
  1200×1200 JPEG (laatu 0,85, n. 350 kt) ja 240×240 pikkukuva (n. 16 kt), pienennys vaiheittain.
  Tallennus: `applyPic()` ennen rivin tallennusta, `finishPic()` sen jälkeen (vanha kuva poistetaan vasta onnistuneen
  tallennuksen jälkeen; epäonnistunut tallennus poistaa juuri ladatut). Rivin poisto poistaa myös kuvat.
  Tila `pic.kind`: `none` / `keep` / `new` / `removed` (Peru palauttaa edellisen). Kuvan valinta tekee lomakkeesta muutetun.
- **Reseptilomake:** tagit valitaan napautettavista chipeistä (`editTags`). Valitun kategorian päärajaukset ensin,
  sitten muut käytössä olevat tagit aakkosjärjestyksessä. Uuden tagin voi kirjoittaa (pilkulla useita) ja lisätä
  "Lisää"-napilla tai Enterillä. Ruudut ⭐ Top-listalla (`f_top`) ja 🍼 Näytä myös vauvan ruoissa (`f_baby`); kun jälkimmäinen on valittu,
  näkyy kenttä 🍼 Vauvaohje (`f_babynotes` → `baby_notes`, tallennetaan vain ristiinmerkitylle, muuten null). Kirjoitettu mutta lisäämätön tagi tallentuu myös. Metatageja ei voi lisätä käsin.
- **Tallentamattomat muutokset:** resepti- ja viinilomake ottavat avattaessa tilannekuvan (`editSnap`, `wineSnap`).
  Peruuta, takaisin-pyyhkäisy ja muut siirtymät kysyvät "Hylätäänkö tallentamattomat muutokset?", jos lomaketta on muutettu.
- **Piilotetut osiot:** `hidden` (Set) = RPC `my_hidden_sections()` `loadAll()`:ssa (adminilla tyhjä). `applyHidden()` piilottaa
  etusivun kortit (`SECTIONS`-avaimet; `data-nav` menus→menut, wines→viinit) ja lomakkeen menukentän. `renderRoute()` ohjaa
  piilotetun kategorian, `menut`/`viinit`/`uusi-viini`-osoitteet etusivulle; `…/vauva` ilman vauvaohjetta, jos vauva piilossa.
  Menumerkit, "Menut"-rajaus ja `allMenus()` tyhjenevät, jos menut piilossa. Lomakkeen kategoriavalinta rakennetaan näkyvistä
  kategorioista + reseptin nykyinen (`CAT_ICON`). Piilossa olevat `f_baby`/`f_babynotes`/`f_menu` säilyttävät arvonsa tallennuksessa.
  Kaikki osiot piilossa (`allHidden()`) → etusivulla "Tervetuloa! Sinulle ei ole vielä avattu…". Ei yhtään reseptikategoriaa →
  "Kaikki reseptit" ja "+ Uusi resepti" piiloon.
- **Hallinta** (vain admin): perheen pääsylista RPC-funktioiden kautta. Jokaisella rivillä oikeusmerkki
  (admin / saa muokata / vain luku / ei pääsyä) ja napit Salli muokkaus / Poista muokkausoikeus, Tee adminiksi /
  Poista admin, Poista pääsy. Lisäyslomakkeessa "Saa lisätä ja muokata" on oletuksena pois (admin-ruutu valitsee
  ja lukitsee sen). "Anna lukuoikeus" listalta puuttuvalle = vain luku. Ei-admin-jäsenillä rivi "Näkyy etusivulla": 5 chippiä
  (`data-act="vis"`, päällä = näkyy, yliviivattu `.chip.off` = piilossa) → `admin_set_hidden`; tilarivillä "piilotettu: …".
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
- **select** ehdolla `(select private.is_family())` – kaikki perheen jäsenet näkevät. Poikkeukset (osioiden näkyvyys):
  `wines` = `can_see('viinit')`; `menus`, `menu_pages` = `can_see('menut')`; `recipes` = is_family ja (ei kategoriatagia TAI
  `tags && my_visible_cats()` TAI menu:-tagi ja `can_see('menut')`). Storage: `viinit/`-kuvat vain `can_see('viinit')`.
- **insert/update/delete** ehdolla `(select private.can_edit())` – vain muokkausoikeus tai admin.

**Uudelle taululle samat neljä policya (lukeminen is_family, kirjoitus can_edit) + `revoke all ... from anon`.**
Jos taulu kuuluu johonkin osioon, lukusäännöksi `can_see('<osio>')`. Uusi osio: lisää se check-rajoitteeseen ja `SECTIONS`-listaan.

**Storage:** yksityinen bucket `recipe-images` (vain `image/jpeg`, max 5 Mt). `storage.objects`-säännöt samalla
mallilla: select `is_family()`, insert/update/delete `can_edit()` (ehtona `bucket_id = 'recipe-images'`).
Reseptit `<uuid>.jpg` (1200×1200) ja `<uuid>-pk.jpg` (240×240), viinit samoin kansiossa `viinit/`. Sivu hakee allekirjoitetut osoitteet
(`createSignedUrls`, voimassa 7 vrk) kaikille kuville kerralla `loadAll()`:ssa (`signImages()`).
Ilmaistasossa 1 Gt tilaa ja 5 Gt siirtoa kuukaudessa riittävät tuhansille kuville.

| Taulu | Sarakkeet |
|---|---|
| `recipes` | id, title, description, servings, prep_minutes, ingredients (jsonb, lista tekstirivejä), instructions (jsonb, lista vaiheita), tags (text[]), source, image_path (Storage-polku, esim. `<uuid>.jpg`), image_url (vanha ulkoinen osoite, ei enää lomakkeessa), notes, baby_notes (vauvaohje, näkyy vain `…/vauva`-näkymässä), created_by, created_at, updated_at |
| `menus` | id, name (uniikki, = reseptien `menu:`-tagin nimi), subtitle, description, sort |
| `menu_pages` | id, menu_id, kind (`menu`/`viinilista`/`muu`), title, html, width, height, sort |
| `wines` | id, name, producer, vintage, wine_type (`punainen`/`valkoinen`/`rosé`/`kuohuviini`/`jälkiruokaviini`/`muu`), grapes, region, country, drunk_on (date), occasion, price, source_rating (teksti, esim. "92/100"), source_name, source_url, own_rating (1–5), own_notes, description, food_pairing, image_path (esim. `viinit/<uuid>.jpg`), image_url (vanha, ei lomakkeessa) |

Yksityinen skeema `private` (ei API:ssa):
- `family_members(email, is_admin, can_edit, hidden_sections text[], added_at)` – pääsylista. Vain listalla olevat sähköpostit näkevät datan.
  `hidden_sections` ⊆ {arkiruoka, viikonloppuherkut, vauvanruoat, menut, viinit} (check-rajoite), oletus = kaikki viisi
  (uusi jäsen `admin_add_member`-funktiolla ei näe mitään; `on conflict` ei muuta näkyvyyttä).
  `can_edit` oletus `false` (vain luku). Admin saa muokata `can_edit`-arvosta riippumatta.
- `is_family()`, `is_admin()`, `can_edit()` (= is_admin or can_edit) – security definer -apufunktiot RLS:ää varten.
- `my_hidden()` (adminilla `{}`), `can_see(osio)` (= is_family ja osio ei piilossa), `my_visible_cats()` – näkyvyys-RLS:ää varten.
- `touch_updated_at()` – trigger.

RPC-funktiot (public, security definer):
- `am_i_admin()`, `can_i_edit()`, `my_hidden_sections()` – kirjautuneen omat oikeudet ja piilotetut osiot.
- Vain admin (tarkistavat `is_admin()`:n itse): `admin_list_users()` (palauttaa myös `can_edit`, adminilla aina true, ja
  `hidden_sections`), `admin_set_hidden(p_email, p_hidden text[])` (ei adminille: "Admin näkee aina kaiken"),
  `admin_add_member(p_email, p_admin = false, p_can_edit = false)` (ei omia admin-oikeuksia pois),
  `admin_set_editor(p_email, p_can_edit)`, `admin_remove_member(p_email)` (ei omaa),
  `admin_set_admin(p_email, p_admin)` (ei omia oikeuksia pois; adminiksi tehty saa `can_edit = true`, joka jää voimaan
  admin-oikeuden poistuessa).
- `backup_family_members()` – pääsylista (myös `hidden_sections`) varmuuskopiota varten. Vain `service_role` (salainen avain) voi kutsua.
- Advisorin varoitus "authenticated security definer function executable" näistä on tarkoituksellinen.
- RLS-testaus SQL:llä: `do $$ … insert testijäsen; set_config('request.jwt.claims', …); set local role authenticated;
  … raise exception 'TULOS …' $$` – lopun virhe peruu kaiken, ja tulos näkyy virheviestissä.

Rekisteröityminen on avoin, mutta ilman pääsylistaa tunnus ei näe mitään. Auth-asetuksissa Site URL ja
Redirect URL = sivun osoite.

## Tagisäännöt (recipes.tags)

- **Kategoria (yksi):** `arkiruoka`, `viikonloppuherkut` tai `vauvanruoat`
- **Myös vauvalle:** `vauvanruoat` voi olla toisena kategoriana pääkategorian lisäksi. Resepti näkyy molemmissa listoissa;
  `catOf()` palauttaa ensimmäisen `CATS`-järjestyksessä, joten pääkategoria voittaa (takaisin-nappi, lomake). Lomakkeessa ruutu
  "🍼 Näytä myös vauvan ruoissa" (`f_baby`, piilossa jos kategoria on jo vauvan ruoat).
  **Vauvalisäykset näkyvät vain vauvan ruoista avattuna** (käyttäjän toive 11/2026): vauvaohje on omassa sarakkeessa
  `baby_notes` (EI `notes`-kentässä, EI ohjeen vaiheissa). Vauvalistan rivi avaa `#/resepti/<id>/vauva` → `detailBaby`
  → laatikko "🍼 Vauvalle" (`.babybox`) heti metarivin alle; takaisin-nappi → vauvan ruoat. Muualta avattuna resepti on
  ennallaan (ei vauvaohjetta, ei vauvamerkkiä).
  Alle 1-vuotiaalle EI: suola, liemikuutiot, hunaja, pinaatti, punajuuri, nokkonen, chili, alkoholi, makkarat/pekoni/kinkku,
  feta/oliivit/kaprikset/soija.
- **Top-lista:** `top`
- **Menu:** `menu:<Menun nimi>` (iso alkukirjain säilyy, sama kuin `menus.name`)
- **Päärajaukset:** arki = valintasivun ryhmät (`GRID`): `top`, `kana`, Liha = `nauta`|`lammas`, `poro`, Kala = `kala`|`katkarapu`,
  `kasvis`, Keitot = `keitto`, Pastat = `pasta`, `lisuke`, `kastike`; viikonloppu = `top`, `alkuruoka`, `illallinen`, `makeat`, `pasta`;
  vauvan ruoat = `top`
- **Muut tagit** pienillä kirjaimilla: esim. `kala`, `keitto`, `uuniruoka`, `pasta`, `poro`, `feta`, `linssi`, `marjat`, `peruna`,
  `kastike`, `marinadi`, `katkarapu`, `pizza`, `lammas`, `papu`, `helppo`, `sienet`
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
  ainekset ja ohje rinnakkain; ohjeen vaiheisiin isommat numerot.
- Myöhemmin ehkä: toimii ilman verkkoa (service worker), ostoslista Muistutuksiin, ajastin ohjeen aikoihin,
  viinien ja reseptien yhdistäminen ruokapareiksi.

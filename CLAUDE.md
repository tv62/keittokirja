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
  Vauvan ruoat, Menut, Viinikellari, Viikkosuunnitelma) Hallinta-sivulla. Piilotus on täysi: kortti, osoitteet ja haku pois, ja RLS estää datan
  latautumisen. Admin näkee aina kaiken. **Uusi jäsen ei näe oletuksena mitään** (käyttäjän päätös 11/2026): admin avaa osiot
  Hallinta-sivulla. Ennen muutosta lisätyt jäsenet näkevät kaiken.
- **Käyttäjä haluaa, että Claude tekee muutokset suoraan** (Supabase + push GitHubiin), ei tiedostojen siirtelyä.
- **Käyttöliittymäpäätökset, joita ei muuteta kysymättä:**
  - Etusivulla ylimpänä **🗓️ Viikkosuunnitelma** (käyttäjän päätös 11/2026, leveä kortti, jossa tämän päivän lounas ja päivällinen),
    sitten viisi korttia: 🥘 Arkiruoka, 🥂 Viikonloppuherkut, 📜 Menut, 🍷 Viinikellari (käyttäjän nimeämä 11/2026) ja niiden alla oma osio
    🍼 Vauvan ruoat (luokka `wide`: iPadilla molempien sarakkeiden levyinen).
  - **Viikkosuunnitelma** (11/2026, ulkoasu hyväksytty Claude Design -luonnoksesta "Viikkosuunnitelma – luonnos"): viikko ma–su,
    joka päivä lounas ja päivällinen (myös arkena). Menneet päivät harmaina ja tiivistettynä (vain päivä + ruokien nimet),
    napautus avaa täyteen kokoon (käyttäjän toive). Tähteet: reseptiä lisätessä "Riittää 1 / 2 / 3 ateriaan", ehdotus =
    annosmäärä ÷ perheen annosta per ateria (oletus 2), lisäateriat merkinnällä "🔁 tähteet" seuraaviin vapaisiin kotiaterioihin:
    **arkena (ma–pe) seuraavaksi päivälliseksi** (lounas syödään yleensä töissä tai lounasravintolassa), viikonloppuna myös
    lounaaksi. **Tähteet aina seuraavasta päivästä alkaen** (lounaalle valmistetun ruoan tähteitä ei samalle päivälle, käyttäjän
    päätös 11/2026; Riittää-sivulla saman päivän päivällisen voi valita itse). **La ja su päivällinen ovat omia ruokiaan**
    (la pääherkkupäivä ja viikon työläin illallinen, su iso satsi seuraavalle viikolle), joten tähteitä ei laiteta niille
    automaattisesti. **Perjantain päivällinen on tavallinen arkipäivällinen**: sille voi tulla tähteitä (käyttäjän päätös 11/2026,
    esim. torstain lounaan tähteet → pe päivällinen); jos se on tyhjä, sille ehdotetaan tavallista arkiruokaa.
    **Siirto raahaamalla** (käyttäjän toive 11/2026): ruokaa tai ehdotusta pidetään hetki sormella ja raahataan toiselle aterialle;
    jos siellä on jo ruoka tai ehdotus, paikat vaihtuvat. Jos tähteet joutuisivat ennen ruoan valmistusta tai yli 2 päivän päähän
    valmistuksesta, kortti näkyy punaisena eikä siirtoa tehdä (ilmoitus kertoo syyn).
    Vain muokkausoikeudella voi muokata. Ostoslista ja toiveet myöhemmin (backlog).
  - **Ruokaehdotukset** (11/2026, käyttäjän päätökset): "💡 Ehdota ruokia" täyttää viikon tyhjät päivälliset ja viikonlopun
    lounaat (tänään ja tulevat) ehdotuksilla. **Arkilounaita ei ehdoteta automaattisesti** (syödään töissä), vain 💡-napista,
    joka on jokaisessa tyhjässä ateriassa "+ Lisää" -napin vieressä. Ehdotuskortissa ✓ Hyväksy, ↻ Toinen (näyttää heti toisen)
    ja oikeassa yläkulmassa "Jätä tyhjäksi ✕" (ei ehdotusta, esim. syödään muualla; kohta jää tyhjäksi), ylhäällä
    "✓ Hyväksy kaikki". **Perjantaina tavallista arkiruokaa; lauantai on pääherkkupäivä ja viikon työläin illallinen**
    (viikonloppuherkkuja vain la–su päivälliselle); lounaiksi kevyttä ja nopeaa (myös viikonloppuna; ei työlästä lounasta ja
    päivällistä samana päivänä). **Sunnuntain päivällinen ehdotetaan 3 aterialle** isona satsina, joka riittää seuraavalle viikolle
    (tähteet maanantain ja tiistain päivällisiksi). Ehdotuksen tähteet näkyvät samalla korostusvärillä kuin ehdotus.
    Rivitasolla pyydetyn ehdotuksen tähteet tulevat heti näkyviin: hyväksytyt ruoat pysyvät paikallaan, hyväksymätön ehdotus
    siirtyy seuraavaan samanlaiseen paikkaan (arkiruoka ei siirrä la–su illallista; pe päivällinen on arkipäivällinen).
    Ehdotukset ovat vain ehdottajan laitteella, kunnes ne hyväksytään (koko perheelle näkyvät ehdotukset = Toiveet, backlog).
  - **Arkiruoka avautuu valintasivuna** (hyväksytty 11/2026): 2 saraketta ruutuja (iPadilla 3), emoji + nimi + määrä:
    Top-lista, Kana, Liha, Poro, Kala, Kasvis, Keitot, Pastat, Lisukkeet, Kastikkeet + leveä "Kaikki arkiruoat".
    Tyhjät ryhmät piilotetaan. Ryhmän listassa ylhäällä "← Arkiruoka" ja päärajaukset yhdellä pyyhkäistävällä rivillä.
  - **Viikonloppuherkut avautuu samanlaisena valintasivuna** (käyttäjän pyyntö 11/2026): Top-lista, Menut, Alkuruoka,
    Illallinen, Makeat, Poro, Liha, Kala, Kasvis, Pastat, Lisukkeet + leveä "Kaikki viikonloppuherkut".
  - Vauvan ruoissa (ei valintasivua) päärajaukset aina näkyvissä. Pikatagit aina piilossa "🏷 Tagit" -napin takana.
  - **Reseptien järjestys** (11/2026, käyttäjä valitsi "molemmat"): jokaisen oma lajitteluvalinta listan yläpuolella
    (Uusimmat ensin / Oma järjestys / Aakkosjärjestys / Vanhimmat ensin / Nopeimmat ensin) + oma yhteinen järjestys, jota
    muokkaajat raahaavat (↕ Järjestä näkyy vain Oma järjestys -valinnassa). **Oletus kaikissa listoissa Uusimmat ensin**
    (käyttäjän päätös 11/2026). Uusimmat/Vanhimmat = lisäyspäivä `added_on`; ilman päivää olevat loppuun.
  - **Valmistusaika** (11/2026): alle 60 min minuutteina ("45 min"), 60 min tai yli tunteina puolen tunnin tarkkuudella
    aina ylöspäin pyöristäen (käyttäjän päätös; 60 → "1 h", 65 → "1,5 h", 160 → "3 h") listarivillä ja reseptisivulla (`fmtTime(m)`). Lomakkeessa aika syötetään minuutteina.
  - **Lisäyspäivä** (11/2026): reseptisivulla "📅 Lisätty 18.1.2023", lomakkeessa kenttä Lisäyspäivä (uudelle tänään),
    listarivillä päivä rivin oikeassa laidassa (11/2026, ennen ⭐:ä) vain päivälajittelussa (oletuksena siis näkyy).
  - Top-listan ⭐ reseptirivin oikeassa laidassa. Menumerkki rivin oikeassa laidassa ja reseptin oikeassa yläkulmassa.
  - **Tagit listarivillä pelkkinä kuvakkeina** (käyttäjän toive 11/2026, esim. 🐔 = kana). Tagit-valinnassa, lomakkeessa ja
    reseptisivulla kuvake + nimi, jotta merkitykset oppii. **Päärajaus-chipit (yläbanneri) pelkkinä emojeina** (11/2026),
    "Kaikki" tekstinä, Tagit-nappi "🏷 ▾" (valittu tagi: "🏷 🥕 ▴").
  - Kuvitetut menut näytetään täsmälleen suunnitellun näköisinä. Ei uudelleenpiirretä.
  - Viineissä rinnakkain arvio lähteestä (teksti + lähde + linkki) ja oma arvio 1–5 tähteä.
  - **Viinikellari, versio 1** (käyttäjän valinta 12.11.2026, "ryhmä 1"; muut viini-ideat backlogissa): viinikaappi **5 hyllyä ×
    3 pulloa** (hylly 1 = ylin, paikka 1 = vasen), yleensä 5–10 pulloa, ostot pääosin Alkosta (ostopaikan oletus "Alko").
    Pullomäärä listalla (🍾 2) ja rajaus **Kaapissa / Juodut / Kaikki** (oletus Kaapissa, jos pulloja on). **🍷 Avaa pullo** kysyy
    päivän, tilaisuuden, tähdet ja muistiinpanon → pullo siirtyy juontihistoriaan. **Kaappikartta** 5 × 3 -ruudukkona (napautus
    avaa viinin, tyhjä paikka → valitse viini → pullot samaan paikkaan). Ostotiedot pullokohtaisesti (päivä, paikka, hinta) ja
    Alkon tuotenumero + linkki viinille. Viinin oma arvio = uusimman tähdillä arvioidun juontikerran tähdet.
  - Resepti- ja viinikuvat aina neliöitä ja värillisiä. Listalla pikkukuva rivin vasemmassa laidassa (ilman kuvaa nimen
    alkukirjain). Puhelimessa kuva sivun lopussa (ennen nappeja), iPadilla/isolla näytöllä ylhäällä; reseptisivulla iPad
    vaakatasossa pienenä (210 px) otsikon oikealla puolella.
    (Tyylittelyä kokeiltiin: piirros-, akvarelli- ja mustavalkotyylit hylättiin, värikuva valittiin.)
  - **Yläpalkki** (käyttäjän toive 11/2026): adminilla yläpalkissa vain ⚙️ (Hallinta, 44 px), **Kirjaudu ulos on Hallinta-sivun
    ylärivillä**. Muilla jäsenillä (ei Hallinta-sivua) Kirjaudu ulos yläpalkissa. Palkki yhden rivin korkuinen, yläpuolella 10 px väli
    (käyttäjän toive 11/2026: iOS 26 Safarin yläreunan sumennus osui otsikkoon; sumennuksen korkeutta sivu ei voi säätää).
  - **Reseptisivu iPadilla vaakatasossa** (11/2026): ainekset vasemmalla ja ohje + muistiinpanot oikealla rinnakkain; ainekset
    pysyvät näkyvissä, kun ohjetta vieritetään. Pystyasennossa ja puhelimessa allekkain kuten ennen.
  - **Ohjeen vaiheet isoilla numeroilla** (käyttäjä valitsi vaihtoehdon B "iso numero" 11/2026; hylätyt: A täytetty pallo,
    C rengas): iso Georgia-numero korostusvärillä ilman palloa, kokkaustilassa tehdyn vaiheen numeron tilalla ✓.
  - **Ainesten väliotsikot otsikkoina** (11/2026): "Kastike:" näytetään lihavoituna Georgia-otsikkona ilman luettelomerkkiä ja
    ilman kaksoispistettä.
- **Reseptilähteet:**
  - Apple Muistiinpanot: vienti Pikakomennoilla (Etsi muistiinpanot → Yhdistä teksti, erotin `=====` → Tallenna tiedosto)
    ja tiedosto chattiin. iCloud-jakolinkkejä ei voi lukea.
    Alkuperäinen päivä: Pikakomento Etsi muistiinpanot (Mikä tahansa: Kansio on …) → Toista kullekin → Teksti
    "[Toistokohde: Nimi] | [Toistokohde: Luontipäivä]" → Yhdistä teksti (Toiston tulokset, uusi rivi) → Kopioi leikepöydälle.
    Lista "Nimi | 18.1.2023 klo 9.31" → `added_on` täsmäyttämällä nimet (pienet kirjaimet, sulut ja muut kuin kirjaimet/numerot pois).
    Muistiinpanojen kuvat eivät tule viennissä mukaan, eikä Claude voi ladata kuvia Storageen (ei avainta). Käyttäjä lisää ne
    itse: Muistiinpanoissa kuvaa pitkään → Kopioi → keittokirjassa Muokkaa → 📋 Liitä kuva (tai iPadilla raahaus).
  - Verkkosivut (esim. Yhteishyvä, HS): linkkejä ei yleensä voi lukea, joten pyydä käyttäjää liittämään teksti.
  - Claude Design -canvasit (menukortit ja viinilistat): ks. "Kuvitetun menun tuominen".

## Työn tila (päivitä, kun jokin muuttuu)

- Lisäyspäivä täytetty 69 reseptille Muistiinpanojen luontipäivistä (11/2026), Lapponia-menun 3 reseptille 8.11.2026
  (käyttäjän antama). Ilman päivää: Hölskypotut. Muistiinpanoissa vielä tuomatta: kansio Jälkiruoat.
- Reseptit (n. 78): Lapponia alla piemontese -menun 3 reseptiä, Apple Muistiinpanojen arkiruoat erä 1 (11) ja
  erä 2 (marraskuu 2026: Keitot 6, Kastikkeet 9, Perjantaiherkut 15, Arkiruoka 28) sekä yksittäin lisättyjä.
  Muistiinpanojen kansiot: Keitot ja Kastikkeet → `arkiruoka` (+ `keitto` / `kastike`), Perjantaiherkut → `viikonloppuherkut`.
  Tyhjinä jätettiin pois: Hummus leivän päälle, Sitruunainen kantarellispagetti, Valkosipulimarinoidut
  lampaankyljykset, Nopea linssibolognese. Wokkikastikkeesta puuttuu ohje (vain ainekset).
  Kaikilla resepteillä on annosmäärä: 28 puuttuvaa arvioitiin 11/2026 käyttäjän hyväksymänä (epävarmimmat: Poropizza 4 =
  puolikas 25 cm pizza/hlö, Osso buco 6 = kiekko/hlö). Kaikilla on myös aika-arvio: 47 puuttuvaa arvioitiin 11/2026
  ohjeen vaiheista (käyttäjän pyyntö, "suuntaa-antava riittää"; pizzoissa ei pohjan/taikinan aikaa).
- Vauvan ruoat: 5 omaa reseptiä Muistiinpanojen kansiosta Soseet ja sormiruoat (11/2026, tagit `sose` / `sormiruoka`,
  lisäyspäivät mukana). Annosmäärät arvioitu vauvan ateriana (pihvit 5, lihasose 8, peruna-kesäkurpitsa 6,
  peruna-porkkana-linssi 8, kesäkurpitsa-parsakaali-herne 4). Peruna-kesäkurpitsasta puuttuu ohje (vain ainekset ja
  tarjoiluvinkit). Lisäksi 11 arkiruokaa on merkitty myös vauvalle sopiviksi (lisätagi `vauvanruoat`, vauvaohje `baby_notes`-kentässä).
  Päärajaukset (11/2026): ⭐ Top-lista, 🥄 Soseet, 🖐️ Sormiruoat. Omat vauvan reseptit ovat omassa järjestyksessä
  ristiinmerkittyjen arkiruokien jälkeen (muokkaaja voi raahata).
- 🗓️ Viikkosuunnitelma versio 1 julkaistu 11.11.2026 (ilman ostoslistaa), ruokaehdotukset samana päivänä. Kaikki nykyiset
  jäsenet näkevät osion (heillä ei ollut piilotuksia); uusi jäsen ei näe sitä oletuksena.
- Seuraavaksi todennäköisesti: Viikkosuunnitelman ostoslista (backlog), puuttuvat ohjeet tyhjiin muistiinpanoihin, uudet menut
  Claude Designista.
- Viinikellarissa 3 viiniä menun viinilistalta; ne on juotu Lapponia-illallisella (juontihistoriassa ilman päivää, siirretty
  vanhasta occasion-kentästä). Pulloja kaapissa ei vielä ole (käyttäjä lisää). Lähteen arviot ja Alko-numerot puuttuvat.
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
  (`createClient` → `auth.onAuthStateChange`, `auth.getSession`, `from(t).select().order()/eq()/in()`,
  `insert/update/delete/upsert`, `.single()`, `rpc`; `my_hidden_sections` → taulukko, esim. `["viinit"]`). Aja 390×844-näkymässä ja katso kuvakaappaukset.
  Viikkosuunnitelman testeissä kiinteä päivä: `ctx.clock.set_fixed_time("2026-11-11T10:00:00")` (menneet päivät, "Tänään").
  Raahaus testataan hiirellä (pointer-tapahtumat): `mouse.down()`, odota 450 ms, `mouse.move(…, steps=10)`, `mouse.up()`;
  korkea näkymä (esim. 390×3000), jotta koko viikko mahtuu ilman vieritystä. Ehdotuksissa on sattumaa: aja tilastotestit useasti.
- Reitityksen testaus: `page.go_back()` = takaisin-pyyhkäisy. Huom. `page.goto(sama_url + "#/…")` on vain
  #-osan vaihto (ei uudelleenlatausta). Oikeaan uudelleenlataukseen `goto("about:blank")` ensin tai `reload()`.
  Vahvistusikkunat (`confirm`) käsitellään `page.on("dialog", …)`.
- Viinikellarin testeissä tilallinen stub (insert/update/delete muuttavat muistissa olevaa dataa `window.__DB`; wine_bottles-paikan
  yksilöllisyys → virhe `23505`), jotta lisäys → avaus → historia voidaan testata peräkkäin.
- iPadin vaakataso: näkymä 1180×820 (pystyssä 820×1180). Vaakatasoasettelu vaatii `orientation: landscape` ja ≥ 960 px.
  Stubin `auth.signOut` kannattaa kutsua tallennettua `onAuthStateChange`-callbackia (`"SIGNED_OUT", null`), jotta uloskirjautumisen voi testata.
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
`winesView`, `wineView`, `wineEditView`, `cellarView`, `winePickView`, `bottleView`, `tastingView`,
`planView`, `planAddView`, `planPickView`, `planItemView`.
Lisää uusi näkymä `VIEWS`-listaan ja reititykseen.

### Osoitteet ja takaisin-ele (reititys)

Jokaisella näkymällä on oma `#/`-osoite, joten iPhonen pyyhkäisy taaksepäin, sivun päivitys ja linkin jakaminen toimivat.

| Osoite | Näkymä |
|---|---|
| `#/` | etusivu |
| `#/reseptit`, `#/reseptit/vauvanruoat` | reseptilista |
| `#/reseptit/arkiruoka`, `#/reseptit/arkiruoka/<ryhmä>`, `#/reseptit/arkiruoka/kaikki` | arkiruoan valintasivu, ryhmän lista |
| `#/reseptit/viikonloppuherkut`, `…/viikonloppuherkut/<ryhmä>` (esim. `menut`), `…/kaikki` | viikonloppuherkkujen valintasivu, ryhmän lista |
| `#/resepti/<id>`, `#/resepti/<id>/muokkaa` | resepti, muokkaus |
| `#/resepti/<id>/vauva` | resepti vauvan ruoista avattuna (näyttää `baby_notes`) |
| `#/uusi-resepti`, `#/uusi-resepti/<kategoria>` | uusi resepti |
| `#/menut`, `#/menut/<nimi, encodeURIComponent>` | menut, yksi menu |
| `#/viinit`, `#/viinit/<id>`, `#/viinit/<id>/muokkaa`, `#/uusi-viini` | viinikellari, viini, viinilomake |
| `#/viinit/kaappi`, `#/viinit/kaappi/<viini>` | kaappikartta (viinin pullot korostettuina) |
| `#/viinit/kaappi/paikka/<hylly>-<paikka>` | tyhjä paikka → valitse viini (vain muokkaaja) |
| `#/viinit/<id>/pullot/uusi[/<hylly>-<paikka>]`, `#/viinit/<id>/pullo/<pullo>` | pulloja kaappiin, pullon muokkaus (vain muokkaaja) |
| `#/viinit/<id>/avaa/<pullo>`, `#/viinit/<id>/juotu/<pullo>`, `#/viinit/<id>/juotu/uusi` | avaa pullo, juontikerran muokkaus / lisäys (vain muokkaaja) |
| `#/hallinta` | käyttäjien hallinta (vain admin) |
| `#/viikko`, `#/viikko/<maanantai YYYY-MM-DD>` | viikkosuunnitelma (tämä viikko / muu viikko) |
| `#/viikko/lisaa/<päivä>/<lounas\|paivallinen>` | ruoan lisäys (vain muokkaaja) |
| `#/viikko/lisaa/<päivä>/<ateria>/<reseptin id>` | riittää 1–3 ateriaan, annokset (vain muokkaaja) |
| `#/viikko/ruoka/<id>` | suunnitellun ruoan muokkaus: siirto, annokset, poisto (vain muokkaaja) |

- **Siirtymät aina `go(hash)`-funktiolla** (pushState + `route()`), ei `show()`:lla suoraan. `go(hash, {replace:true})`
  korvaa nykyisen historiarivin (esim. uusi resepti tallennettu → lomake korvataan reseptillä).
- **Takaisin-napit `back(fallback)`:** `history.back()`, jos sovelluksen sisällä on edellinen sivu (`history.state.d > 0`),
  muuten mennään `fallback`-osoitteeseen (suora linkki). Poiston jälkeen `leaveDeleted()` = `history.go(-2)`.
- `route()` lukee osoitteen ja kutsuu `renderRoute()`:a. Tuntematon tai poistettu kohde → etusivu.
  Osoitteet, jotka eivät ala `#/` (esim. Supabasen `#access_token=…`), tulkitaan etusivuksi.
- Vierityskohta tallennetaan `history.state.y`:hyn ja palautetaan takaisin tultaessa.
- Listan rajaukset (kategoria, päärajaus, tagi, haku) säilyvät, kun reseptistä palataan. Ne nollataan,
  kun lista avataan etusivun kortista (`resetListFilters()`). Sama viineissä (`wineType`, `wq`).
- Data (reseptit, menut, viinit, pullot `bottleQuery()`, admin-tieto, viikkosuunnitelma `loadPlan()`) ladataan kerralla `loadAll()`-funktiossa ennen
  ensimmäistä `route()`-kutsua. Viikkosuunnitelma haetaan lisäksi uudelleen (`refreshPlan()`), kun etusivu tai viikko avataan
  (enintään 15 s välein) ja kun sovellus palaa näkyviin, jotta muiden muutokset näkyvät.
- Viikkosuunnitelman sisäiset siirtymät: viikon vaihto `‹ ›` korvaa historiarivin (takaisin-ele → etusivu). Reseptin tallennus
  riittää-sivulta ja sen Peruuta palaavat `leaveDeleted()`-funktiolla kaksi askelta (ohi lisäyssivun) viikkoon.

### Näkymät

- **Etusivu:** hakukenttä (`#hq`) ja kortit Arkiruoka, Viikonloppuherkut, Vauvan ruoat (`data-cat`), Menut ja Viinikellari (`data-nav`;
  laskuri "N pulloa kaapissa", ilman pulloja "N viiniä").
  Haku etsii resepteistä (nimi, kuvaus, kategorian nimi, ainekset, tagit, menu), menuista ja viineistä. Haun aikana kortit
  piilotetaan ja tulokset näytetään ryhmiteltynä. Haku säilyy, kun tuloksesta palataan. Otsikko (pata-logo + "Keittokirja")
  vie etusivulle ja tyhjentää haun. `data-home`-elementti = takaisin etusivulle.
- **Haku kaikkialla:** `words()` + `matches()`: jokaisen hakusanan pitää löytyä (esim. "kana pasta").
- **Kategoriat** `CATS`-vakiossa. Uusi kategoria: `CATS` + `PRIMARY` (tai `GRID`) + etusivun kortti (`data-cat`, laskuri `cnt_<kategoria>`)
  + `<option>` lomakkeen `#f_category`-valintaan. Reititys, lista, haku ja lomake toimivat sen jälkeen automaattisesti.
- **Valintasivu (`GRID`):** `GRID[kategoria] = { all, groups: [[avain, emoji, otsikko, tagit]] }` (arkiruoka ja viikonloppuherkut).
  Ryhmään kuuluu resepti, jolla on mikä tahansa ryhmän tageista (`hasTag(r, t)`; erikoistagi `"@menu"` = reseptillä on menu,
  ryhmä näytetään menuittain ryhmiteltynä `menuMode`, ja `groupOff(g)` piilottaa sen, jos menut on piilotettu).
  `gridGroup(cat, k)` ei palauta piilotettua ryhmää, joten sen osoite ohjautuu etusivulle. Ryhmä tulee osoitteesta:
  `openCategory(cat, group)` asettaa `activePrimary = group`. `renderList()`: `gridMode` = GRID-kategoria ilman ryhmää,
  hakua ja tagia → `#subGrid` näkyy, chipit ja lista piiloon. Haku valintasivulla näyttää suoraan listan.
  Ruutu → `go(.../<ryhmä>)` (uusi historiarivi), chip → `go(..., {replace:true})`, joten takaisin-ele vie aina valintasivulle.
  `#toGrid` ("← Arkiruoka") = `back(#/reseptit/<kategoria>)`; `#listHome` piilossa ryhmälistassa. Ryhmästä valintasivulle
  palatessa haku ja tagi nollataan; reseptistä palatessa ne säilyvät. Chip-rivi `.chips.scroll`, valittu keskitetään.
- **Reseptilista:** päärajaukset `PRIMARY`-vakiossa kategorioittain (nyt vain vauvan ruoat; GRID-kategorioissa ryhmät), pikatagit
  piilossa "🏷 Tagit" -napin takana. "Menut"-ryhmä (`@menu`) ryhmittelee menuittain, järjestys `COURSE_ORDER` = alkuruoka → illallinen → makeat.
- **Järjestys** (`#sortBar`, piilossa valintasivulla ja Menut-ryhmittelyssä): `sortMode` (`SORTS`: oma = `sort_order`, az,
  uusin/vanhin = `added_on` (null loppuun, sitten `created_at`), nopein = `prep_minutes`, tasatilanteessa nimi), oletus `uusin`,
  muistetaan laitteella `localStorage`-avaimella `kk_jarjestys` (vanha `kk_sort` poistetaan, ettei aiempi "oma" peitä oletusta). Apurit `fiDate(iso)` ja `todayIso()`.
  "↕ Järjestä" (`#orderBtn`, vain muokkaajalle ja vain omassa järjestyksessä) → `ordering`: rivit `<div class="item sorting">`
  + ≡-kahva (`.grip`, `touch-action:none`), napautus ei avaa reseptiä. Raahaus pointer-tapahtumilla (sormi ja hiiri),
  automaattivieritys reunoilla. `saveOrder(ids)`: näkyvät reseptit jakavat keskenään nykyiset `sort_order`-numeronsa uudessa
  järjestyksessä (muiden listojen suhteellinen järjestys säilyy) → RPC `set_recipe_order(p_ids, p_pos)`; virheessä palautus.
- **Reseptirivi** (`recipeItem`): oikeassa laidassa (`.side`) menumerkki, lisäyspäivä `.rdate` (vain uusin/vanhin-lajittelussa)
  ja ⭐ (top). Metarivillä aika · tagit
  kuvakkeina `tagIcons(r)`: järjestys `TAG_ICON`-vakion mukaan (pääaines → ruokalaji → tapa), sama kuvake yhdistetään
  (esim. 🥔 = peruna + lisuke), nimi `aria-label`/`title`-attribuutissa, kuvakkeeton tagi tekstinä perässä.
  `tagLabel(t)` = kuvake + nimi (Tagit-chipit, lomake, reseptisivu). Päärajaus-chipit (`#primaryChips`): pelkkä emoji,
  luokka `.chip.ico` (52×44 px), nimi `aria-label`/`title`; emoji `PRIM_ICON` (top, @menu), muuten `TAG_ICON`, GRID-chipeissä
  ryhmän emoji. "Kaikki" tekstinä.
- **Resepti:** menumerkki oikeassa yläkulmassa avaa menun. Napit "⭐ Lisää Top-listalle" ja "Muokkaa"
  (Poista on lomakkeen alaosassa, ettei sitä paina vahingossa).
  - **Rakenne** (`renderDetail`): `.dtop` (luokka `.haspic`, jos kuva) = `rimg-top` + `.dinfo` (otsikko, kuvaus, meta,
    vauvalaatikko, annos- ja kokkausnapit); `.dcols` (luokka `.two`, jos sekä ainekset että ohje/muistiinpanot/lähde) =
    `.dcol-ing` (Ainekset + skaalausohje) ja `.dcol-main` (Ohje, Muistiinpanot, Lähde); sitten `rimg-end` ja napit.
    **Vaakataso** `@media (min-width: 960px) and (orientation: landscape)`: `show()` asettaa `body.v-detail` reseptisivulla →
    `.wrap` 1160 px (muut sivut 760 px); `.dcols.two` grid 2fr/3fr, `.dcol-ing` `position: sticky` (top 12 px, max-korkeus
    näytön korkeus, oma vieritys); `.dtop.haspic` grid, kuva 210 px oikealle; annos- ja kokkausnappi vierekkäin vasemmalla.
  - **Vaiheiden numerot:** `ol.steps` ilman selaimen numerointia, CSS-laskuri `counter(step)` `li::before`-elementissä
    (1,7rem Georgia, `--accent`, 34 px oikealle tasattu, teksti 44 px sisennyksellä; kokkaustilassa 60 px). `li.done::before` = ✓.
  - **Ainesten väliotsikot:** `isSub(t)` (rivi päättyy `:`) → `<li class="sub">` ilman kaksoispistettä (data säilyy ennallaan),
    ei luettelomerkkiä, sisennys −20 px (tasan Ainekset-otsikon kanssa), kokkaustilassa ei napautettava eikä alaviivaa.
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
  (max 440 px; reseptisivulla vaakatasossa ≥ 960 px otsikon oikealla, 210 px). Vanha `image_url` toimii varalla, jos `image_path` puuttuu.
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
- **Yläpalkki** (`header`): otsikko vasemmalla, `.hbtns` oikealla: `#adminBtn` (⚙️, luokka `.gear`, vain adminille, ei
  Hallinta-sivulla) ja `#logoutBtn` (vain muille kuin adminille, `show()` piilottaa `isAdmin`-tiedolla).
- **Hallinta** (vain admin): ylärivillä (`.admintop`) ← Etusivu ja Kirjaudu ulos (`#adminLogoutBtn`, sama `sb.auth.signOut()`).
  Perheen pääsylista RPC-funktioiden kautta. Jokaisella rivillä oikeusmerkki
  (admin / saa muokata / vain luku / ei pääsyä) ja napit Salli muokkaus / Poista muokkausoikeus, Tee adminiksi /
  Poista admin, Poista pääsy. Lisäyslomakkeessa "Saa lisätä ja muokata" on oletuksena pois (admin-ruutu valitsee
  ja lukitsee sen). "Anna lukuoikeus" listalta puuttuvalle = vain luku. Ei-admin-jäsenillä rivi "Näkyy etusivulla": 6 chippiä (`SECTIONS`)
  (`data-act="vis"`, päällä = näkyy, yliviivattu `.chip.off` = piilossa) → `admin_set_hidden`; tilarivillä "piilotettu: …".
- **Vain lukuoikeus** (`canEdit` = RPC `can_i_edit`): `body.readonly` piilottaa `.edit-only`-napit (+ Uusi).
  Reseptin Top- ja Muokkaa-napit sekä viinin Muokkaa jätetään renderöimättä. Lomakkeiden osoitteet
  (`…/muokkaa`, `#/uusi-resepti`, `#/uusi-viini`) ohjautuvat etusivulle. Annosmäärä ja kokkaustila toimivat kaikille.
  Tietokanta estää muokkauksen joka tapauksessa (RLS), käyttöliittymä vain siistii näkymän.
- **Viikkosuunnitelma** (osio `viikko`, 11/2026). Tila: `planItems` (meal_plan-rivit), `planNotes` (Map päivä → muistiinpano),
  `ppm` (annosta per ateria), `planWeek` (näytettävän viikon maanantai), `openPast` (avatut menneet päivät), `noteEdit`.
  Päivät ovat paikallisia `YYYY-MM-DD`-merkkijonoja: apurit `isoOf`, `dateOf`, `addDays`, `mondayOf`, `isoWeek` (ISO-viikko),
  `dayShort` ("ke 11.11."), `dayLabel` ("Ke 11.11."), `weekRange`, `weekHash` (tämä viikko → `#/viikko`). `MEALS` = lounas/päivällinen.
  - **Etusivun kortti** `#wkCard` (`renderPlanCard`): viikko + tämän päivän ateriat; ruoan napautus avaa reseptin.
  - **Viikkonäkymä** (`renderPlan`, `dayHtml`, `planItemHtml`): päiväkortit, tänään korostettu (`.day.now`). Menneet päivät
    `.day.compact` (päivä + ruokien nimet), napautus (`data-expand`) avaa, "Pienennä" (`data-collapse`) sulkee. Ruoka-rivi:
    pikkukuva tai alkukirjain (vapaa teksti: katkoviivareunus), nimi, metarivi (aika · tagikuvakkeet · annokset, tähteillä
    "🔁 tähteet ti päivällisestä"). Rivin napautus `openFromPlan(it)` asettaa reseptin annosmäärän (`cookState(r).servings`,
    tähteillä alkuperäisen rivin `servings`) ja avaa reseptin; vapaa teksti avaa muokkauksen. Muokkaajalle `+` (lisää aterialle),
    `⋯` (muokkaus), tyhjässä ateriassa "+ Lisää lounas" ja päivän otsikossa 📝 (muistiinpano, `upsert`/`delete` meal_plan_notes).
    Lukijalle ei nappeja, tyhjä ateria "–".
  - **Lisäys** (`openPlanAdd`, `renderPlanAdd`): ✏️ Oma merkintä (vapaa teksti) tai resepti: haku (kaikista näkyvistä
    resepteistä), kategoriachipit (näkyvät kategoriat), ryhmächipit (`GRID`-ryhmät / vauvalla `PRIMARY`), järjestys (sama `sortMode`).
  - **Riittää** (`openPick`, `renderPick`, `savePick`, tila `pick`): 1 / 2 / 3 ateriaan, ehdotus `suggestMeals` = floor(servings ÷ ppm),
    annokset `servingsFor` = max(reseptin annokset, ateriat × ppm). Tähteet `leftoverSlots(päivä, ateria, n, keep, busy, maxDay)`:
    seuraavat vapaat paikat aikajärjestyksessä (`slotsAfter`) seuraavasta päivästä alkaen (ei koskaan samalle päivälle), joille `leftoverOk` = kotiateria
    (`isHomeMeal`: päivällinen tai la/su lounas) eikä oma päivällinen (`isOwnDinner`: la/su päivällinen); ilman `maxDay`:ta
    varalla seuraavat päivälliset (valitut säilyvät, kun ateriamäärää muutetaan); jokaisen paikan voi vaihtaa valinnasta (myös arkilounaaksi)
    (5 viikkoa eteenpäin, varatut merkitty). "noin [2] annosta" -valinta tallentaa `plan_settings.portions_per_meal`.
  - **Muokkaus** (`openPlanItem`, `renderPlanItem`, tila `piState`): päivä + ateria (siirto, uusi `sort` kohdeaterian loppuun),
    annokset (vain alkuperäinen reseptirivi), vapaan tekstin nimi, 📖 Avaa resepti, Poista. Poisto kysyy erikseen, poistetaanko
    myös tähteet; jos ei, tähteet jäävät (FK `leftover_of` → null).
  - Näytettävä nimi on reseptin nykyinen nimi (`titleOf`), `meal_plan.title` on varalla (vapaa teksti, poistettu tai piilotettu resepti).
  - **Ehdotukset** (vain muistissa, ei tietokannassa ennen hyväksyntää; nollautuvat viikkoa vaihdettaessa `resetSuggestions()`).
    Tila: `suggOn` (koko viikko), `suggAsked` (yksittäin pyydetyt / hylätyt paikat), `suggPick` (paikka → valittu resepti,
    pysyy kunnes hyväksytään tai hylätään), `suggRejected` (viikon hylätyt reseptit), `suggSkip` (tyhjäksi jätetyt paikat,
    `skipSuggestion`; 💡 poistaa paikan sieltä). `computeSuggestions()` (ajetaan joka
    `renderPlan()`:ssa) laskee `sugg` (paikka → { rid, meals, servings, slots }) ja `suggReserved` (ehdotuksen tähteiden paikat,
    näkyvät korostettuna "💡 Ehdotuksen tähteet", `.sresv` sama tausta kuin `.sugg`), vain tänään ja tulevat päivät:
    1) päivällisille valitaan ensin resepti (`suggPick`), jotta lounas tietää saman päivän päivällisen työläyden (`ctx.dinnerPrep`);
    2) paikat **aikajärjestyksessä** (ma lounas, ma päivällinen, …). Automaattisesti (`suggOn`) vain kotiateriat, arkilounas vain
    `suggAsked`-pyynnöstä. Tähteet `leftoverSlots(…, busy = varatut/tyhjäksi jätetyt, maxDay)` enintään 2 pv päähän ja viikon
    sisällä (vain sunnuntain päivällisen tähteet ma–ti); ateriamäärä = 1 + löytyneet paikat. Suunnitellut ruoat estävät tähteet
    (pysyvät paikallaan); jos tähteet tulevat hyväksymättömän ehdotuksen paikalle, sen resepti siirtyy (`carry`) seuraavaan saman
    tyypin (`slotKind`: arkipäivällinen ma–pe / viikonlopun lounas) vapaaseen paikkaan (avain vaihtuu, `suggAsked` siirtyy mukana);
    viikon lopussa siirtymättä jääneet pudotetaan. Ateriat `suggMealsFor` = sunnuntain päivällinen 3, muuten `suggestMeals`.
    Valinta `chooseSuggestion`: joukko `suggPool` (pääruoat: ei `NOT_MAIN` = kastike/marinadi/lisuke/alkuruoka/makeat;
    arkiruoka + la–su päivälliselle viikonloppuherkkujen illallinen/pasta), pois hylätyt, muiden paikkojen valinnat ja ±14 päivän
    sisällä suunnitellut (jos ei löydy, sallitaan muut kuin saman viikon). Painotettu arvonta `suggScore`: lounas kevyt ja nopea
    (≤ 20 min ×1,5, ≤ 30 ×1, keitto/kasvis/helppo/pasta ×1,3, uuni/pata ×0,5; jos saman päivän päivällinen ≥ 60 min, lounas ≤ 20 min),
    arkipäivällinen (myös pe) ≤ 45 min, la pääherkku ja työläs (≥ 90 min ×2,5, ≥ 60 ×1,6, viikonloppuherkut ×4), su iso satsi
    (≥ 6 annosta ×3, pata/keitto/uuni/linssi ×2, muut ×0,3, viikonloppuherkut ×0,3), Top ×2, pääaines (`proteinOf`: kala/poro/liha/kana/kasvis) ei samaa viereisinä päivinä (×0,15), ≥ 2 kertaa viikossa ×0,4,
    puuttuva kala tai kasvis ×1,8. ✓ = `acceptSuggestions(keys)` (alkuperäiset yhdellä insertillä, tähteet toisella),
    ↻ Toinen = `rejectSuggestion`, Jätä tyhjäksi = `skipSuggestion`, nimen napautus = `openSuggestion` → Riittää-sivu ehdotuksen arvoilla (`pick.fromSugg`: tallennus ja
    Peruuta palaavat yhden askeleen). `suggPinned` = raahaamalla paikalleen siirretyt ehdotukset: tähteet eivät vie paikkaa
    (`busy`) eikä niitä siirretä (`carry` ohittaa); poistuu, kun paikka suunnitellaan, tyhjennetään tai ehdotukset piilotetaan.
  - **Siirto raahaamalla** (vain muokkaajalle; vihje `#dragHint`): raahattavat `[data-drag]` = suunniteltu ruoka `p:<id>` (`.pi`)
    ja ehdotus `s:<paikka>` (`.sugg`), kohteet `.meal[data-slot="päivä|ateria"]` (tiivistetyt menneet päivät eivät ole kohteita).
    Pitkä painallus (`DRAG_DELAY` 350 ms, liike > 10 px ennen sitä = vieritys) → haamukortti `.dragghost` seuraa sormea,
    lähde `.dragsrc`, automaattinen vieritys reunoilla, iPhonessa `touchmove` estetään raahauksen aikana; pitkän painalluksen
    jälkeinen klikkaus ohitetaan (`dragJustEnded`). `planMove(src, kohde, kohteenRuoka)` = kuivaharjoitus: ruoka → tyhjä
    (ehdotus siirtyy lähtöpaikalle, jos kohteessa oli ehdotus) / ruoka ↔ ruoka (sormen alla oleva tai ensimmäinen) /
    ehdotus ↔ koko ateria (kaikki kohteen ruoat lähtöpaikalle) / ehdotus ↔ ehdotus / ehdotus → tyhjä (vanha paikka `suggSkip`).
    Tarkistus kaikille siirtyville riveille: tähteet (`leftover_of`-parit, uusilla paikoilla) oltava valmistuksen jälkeen
    (`slotRank`) ja enintään 2 pv päässä → muuten `{ ok: false, reason }`: kohde `.meal.drop-bad` ja haamu `.dragghost.bad`
    punaisina, ilmoitus `toast()` (alhaalla, `#toast`), pudotuksessa lähdekortti välähtää punaisena (`.flashbad`), ei muutoksia.
    `applyMove`: ehdotusten avaimet (`suggPick`, kiinnitys `pinSlot`) + rivien `day/meal/sort`-päivitykset (vaihdossa sort vaihtuu).
- **Menut:** menukortti- ja viinilistasivut välilehtinä, alla menun reseptit.
- **Viinikellari** (osio `viinit`): vakiot `SHELVES` = 5, `SLOTS` = 3, `PLACE_RE` (paikka "h-p"); sama rajaus tietokannan check-ehdoissa.
  Tila `bottles` (wine_bottles), apurit `inCellar(viini?)` (status kaapissa, järjestys hylly/paikka), `tastings(viini)` (status juotu,
  uusin päivä ensin, päivätön loppuun), `takenPlaces()`, `freePlaces(paitsiPullo)`, `placeTxt`, `pullot(n)`, `eur`, `parsePrice`,
  `alkoUrl(nro)` = `https://www.alko.fi/tuotteet/<nro>/`, `WCOLOR` (ruudun värikaista viinin tyypin mukaan). `refreshCellar()`
  hakee viinit ja pullot taustalla, kun lista tai kaappikartta avataan.
  - **Lista** (`renderWines`): Kaappikartta-nappi (`#cellarBtn`, "3 / 15 paikkaa käytössä"), haku, rajaus `wineStatus`
    (`#wineStatus`: kaapissa / juodut = ei kaapissa / kaikki; oletus kaapissa, jos pulloja on), tyyppichipit (yksi pyyhkäistävä rivi,
    "Kaikki tyypit"). Rivi `wineItem`: pikkukuva, nimi + vuosikerta, tuottaja · tyyppi · alue, oikealla allekkain 🍾 määrä ja tähdet.
  - **Viinin sivu** (`openWine` → `renderWine`, `curWineId`): arviot, tiedot (Hinta, Alko-linkki, Ruokapari), "🗄️ Kaapissa · 2 pulloa"
    (rivit `bottleRow`: paikka, ostotiedot, 🍷 Avaa ja ⋯ muokkaus; "Näytä kaapissa ›" → `#/viinit/kaappi/<id>`), "+ Lisää pulloja
    kaappiin", "🍷 Juontihistoria" (`tastingRow`, muokkaajalla napautus muokkaa) ja "+ Lisää juontikerta" (ilman pulloa, esim. ravintola).
    Napit `data-wact` (yksi `#wineDetail`-käsittelijä).
  - **Kaappikartta** (`openCellar(korostus)`, `renderCellar`): `.cellar` > `.shelf` (otsikko "Hylly n" + 3 `.slot`): täysi ruutu
    `.slot.full` (nimi 2 riviä, vuosikerta, värikaista `--wc`), tyhjä `.slot.empty` (muokkaajalle "+"-nappi `data-place`, lukijalle div).
    Korostus: viinin ruudut `.hl`, muut `.dim`. Alla "Ilman paikkaa" -viinit.
  - **Tyhjä paikka** (`openWinePick`, `pickPlace`): viinin valinta korvaa valintasivun historiassa → pullolomake; "+ Uusi viini" →
    `#/uusi-viini` (korvaa) ja `pendingPlace` → `openWineEditor(null, true)` ottaa sen `newWinePlace`:ksi → tallennuksen jälkeen
    pullolomake samaan paikkaan (korvaa). Näin tallennuksen jälkeinen takaisin vie kaappikarttaan.
  - **Pullolomake** (`openBottleForm(viini, pullo?, paikka?)`, tila `bState`): uusille määrä −/+ (1–24) ja jokaiselle paikka
    (`<select>`: vapaat paikat + "Ei paikkaa", ehdotuksena seuraavat vapaat), ostopäivä (tänään), hinta/pullo (viinin hinta), ostopaikka
    ("Alko"). Muokkauksessa yksi paikka + "Poista pullo kaapista" (ei juontikertaa, esim. lahja). Varattu paikka (`23505`) → ilmoitus.
  - **Avaa pullo / juontikerta** (`openTasting(viini, pullo, "avaa"|"uusi"|"muokkaa")`, `tState`, tähdet `tStars`): päivä (tänään),
    tilaisuus, tähdet, muistiinpanot. Avaus = update `status: "juotu"`, paikka null, ehto `status = kaapissa` (ei kahta avausta).
    Tallennuksen jälkeen `syncWineRating(viini)`: viinin `own_rating` = uusimman tähdillä arvioidun juontikerran tähdet.
  - **Viinilomake:** kentät kuten ennen + Alkon tuotenumero (`w_alko`, 3–8 numeroa); Juotu- ja Missä-kentät poistettu (→ juontihistoria).
    Viinin poisto poistaa myös pullot ja juontihistorian (FK on delete cascade; vahvistus kertoo sen).
- Napautettavat chipit ovat vähintään 44 px korkeita (sormelle sopiva koko).
- Käyttäjän syöte escapetaan aina `esc()`-funktiolla ennen `innerHTML`:ää. URLit `safeUrl()`-funktiolla.

## Tietokanta (Supabase)

Kaikissa julkisissa tauluissa on RLS. Säännöt `to authenticated`:
- **select** ehdolla `(select private.is_family())` – kaikki perheen jäsenet näkevät. Poikkeukset (osioiden näkyvyys):
  `wines`, `wine_bottles` = `can_see('viinit')`; `menus`, `menu_pages` = `can_see('menut')`; `recipes` = is_family ja (ei kategoriatagia TAI
  `tags && my_visible_cats()` TAI menu:-tagi ja `can_see('menut')`); `meal_plan`, `meal_plan_notes`, `plan_settings` = `can_see('viikko')`.
  Storage: `viinit/`-kuvat vain `can_see('viinit')`.
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
| `recipes` | id, title, description, servings, prep_minutes, ingredients (jsonb, lista tekstirivejä), instructions (jsonb, lista vaiheita), tags (text[]), source, image_path (Storage-polku, esim. `<uuid>.jpg`), image_url (vanha ulkoinen osoite, ei enää lomakkeessa), notes, baby_notes (vauvaohje, näkyy vain `…/vauva`-näkymässä), added_on (date, lisäyspäivä, oletus current_date), sort_order (oma järjestys, uusi = max+1 triggerillä `recipes_default_sort`), created_by, created_at, updated_at |
| `menus` | id, name (uniikki, = reseptien `menu:`-tagin nimi), subtitle, description, sort |
| `menu_pages` | id, menu_id, kind (`menu`/`viinilista`/`muu`), title, html, width, height, sort |
| `meal_plan` | id, day (date), meal (`lounas`/`paivallinen`), sort (järjestys aterian sisällä), title (reseptin nimi tallennushetkellä tai vapaa teksti), recipe_id (null = vapaa teksti; FK recipes, on delete set null), servings (vain alkuperäisellä reseptirivillä), leftover (bool, tähteet), leftover_of (alkuperäinen rivi; FK, on delete set null), created_by, created_at, updated_at (trigger `touch_updated_at`) |
| `meal_plan_notes` | day (date, pääavain), note (1–300 merkkiä), updated_at |
| `plan_settings` | id (aina 1, yksi rivi), portions_per_meal (numeric, oletus 2: perhe syö aterialla noin näin monta annosta) |
| `wines` | id, name, producer, vintage, wine_type (`punainen`/`valkoinen`/`rosé`/`kuohuviini`/`jälkiruokaviini`/`muu`), grapes, region, country, price, alko_id (Alkon tuotenumero, 3–8 numeroa), source_rating (teksti, esim. "92/100"), source_name, source_url, own_rating (1–5), own_notes, description, food_pairing, image_path (esim. `viinit/<uuid>.jpg`), image_url (vanha, ei lomakkeessa), created_at, updated_at. (drunk_on ja occasion poistettu 11/2026 → wine_bottles.) |
| `wine_bottles` | id, wine_id (FK wines, on delete cascade), status (`kaapissa` = pullo kaapissa / `juotu` = juontikerta), shelf (1–5) + slot (1–3) (molemmat tai ei kumpaakaan; vain kaapissa; yksilöllinen paikka kaapissa olevilla: osittainen indeksi `wine_bottles_place_uniq`), purchased_on, purchase_place, price (per pullo), opened_on (juontipäivä, voi puuttua), occasion, rating (1–5), notes, created_by, created_at, updated_at (trigger `touch_updated_at`) |

Yksityinen skeema `private` (ei API:ssa):
- `family_members(email, is_admin, can_edit, hidden_sections text[], added_at)` – pääsylista. Vain listalla olevat sähköpostit näkevät datan.
  `hidden_sections` ⊆ {arkiruoka, viikonloppuherkut, vauvanruoat, menut, viinit, viikko} (check-rajoite), oletus = kaikki kuusi
  (uusi jäsen `admin_add_member`-funktiolla ei näe mitään; `on conflict` ei muuta näkyvyyttä).
  `can_edit` oletus `false` (vain luku). Admin saa muokata `can_edit`-arvosta riippumatta.
- `is_family()`, `is_admin()`, `can_edit()` (= is_admin or can_edit) – security definer -apufunktiot RLS:ää varten.
- `my_hidden()` (adminilla `{}`), `can_see(osio)` (= is_family ja osio ei piilossa), `my_visible_cats()` – näkyvyys-RLS:ää varten.
- `touch_updated_at()` – trigger. `recipes_default_sort()` – trigger (security definer): uuden reseptin `sort_order` = max + 1.

RPC-funktiot (public, security definer):
- `am_i_admin()`, `can_i_edit()`, `my_hidden_sections()` – kirjautuneen omat oikeudet ja piilotetut osiot.
- `set_recipe_order(p_ids uuid[], p_pos int[])` – security invoker (RLS), tarkistaa `can_edit()`; päivittää `sort_order`-arvot.
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
  `kasvis`, Keitot = `keitto`, Pastat = `pasta`, `lisuke`, `kastike`; viikonloppu = valintasivun ryhmät: `top`, Menut = `menu:`-tagi,
  `alkuruoka`, `illallinen`, `makeat`, `poro`, Liha = `nauta`|`lammas`, Kala = `kala`|`katkarapu`, `kasvis`, Pastat = `pasta`, `lisuke`;
  vauvan ruoat = `top`, `sose`, `sormiruoka`
- **Muut tagit** pienillä kirjaimilla: esim. `kala`, `keitto`, `uuniruoka`, `pasta`, `poro`, `feta`, `linssi`, `marjat`, `peruna`,
  `kastike`, `marinadi`, `katkarapu`, `pizza`, `lammas`, `papu`, `helppo`, `sienet`; vauvan ruoissa `sose`, `sormiruoka`
- Lisukkeet (perunat, salaatit ym.) saavat `lisuke`-tagin. Kasvisruoka-lisuke voi saada myös `kasvis`-tagin.
- Kategoria-, top- ja menu-tagit ovat "metatageja" (`isMeta`), eikä niitä näytetä tavallisina tageina.
- **Kuvakkeet** `TAG_ICON`-vakiossa (keittokirja.html). Uudelle tagille lisää kuvake sinne (muuten se näkyy listarivillä tekstinä).
  Nykyiset: kana 🐔, nauta 🐄, lammas 🐑, poro 🦌, kala 🐟, katkarapu 🦐, kasvis 🥕, linssi 🫛, papu 🫘, peruna 🥔, sienet 🍄,
  feta 🧀, marjat 🫐, keitto 🍲, pasta 🍝, pizza 🍕, kastike 🥣, marinadi 🫙, lisuke 🥔, sose 🥄, sormiruoka 🖐️, alkuruoka 🥗,
  illallinen 🍽️, makeat 🍰, uuniruoka 🔥, pata 🫕, helppo 👌.

## Reseptien lisääminen

- Säilytä ainekset ja ohjeet sisällöltään ennallaan. Siisti numerointi ja muotoilu, korjaa kirjoitusvirheet.
- Jos vaiheissa mainittu aines (esim. vesi) puuttuu luettelosta, lisää se luetteloon.
- Väliotsikot aineslistassa omana rivinään, esim. `"Kastike:"` (kaksoispiste mukaan; sivu näyttää rivin otsikkona ilman sitä).
- `prep_minutes` = valmistusaika ilman marinointia, hyytymistä tai huoneenlämpöön ottamista. Mainitse ne `notes`-kentässä.
  Aina mukaan: jos lähteessä ei ole, arvioi vaiheiden ajoista (esivalmistelu + kypsennys) ja kerro arvio.
- `servings` aina mukaan (annosmäärä tarvitaan −/+ -skaalaukseen). Jos lähteessä ei ole, arvioi ainesten perusteella ja
  kerro arvio: pääruoka n. 100–150 g lihaa/kalaa tai 75–100 g kuivaa pastaa per hlö, keitto n. 2,5 dl nestettä per hlö,
  kastikkeet/dipit/marinadit lisukkeena (tyypillisesti 4), alkuruoka-bruschetta 2 palaa per hlö,
  vauvan sose n. 1 dl per ateria (lihasose 1–1½ rkl), vauvan pikkupihvit 2–3 per ateria.
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
  - Hakee REST-rajapinnasta taulut `recipes`, `wines`, `wine_bottles`, `menus`, `menu_pages`, `meal_plan`, `meal_plan_notes`, `plan_settings`
    (→ `data/<taulu>.json`; `TABLES` = taulu → järjestyssarake, meal_plan_notes järjestetään `day`-sarakkeella), RPC:llä
    `backup_family_members()` (→ `data/family_members.json`) ja bucketin `recipe-images` kuvat (→ `kuvat/`, vain uudet;
    poistetut poistetaan, historia säilyttää). Lisäksi `viimeisin.txt`, jolloin jokainen ajo on oma committinsa.
  - Avain: repon salaisuus `SUPABASE_SECRET_KEY` (Supabasen salainen avain nimeltä "varmuuskopio", `sb_secret_…`).
    Lähetetään vain `apikey`-otsakkeessa. Ohjelma keskeyttää, jos avain ei ole salainen tai tulos olisi tyhjä.
  - Jos lisäät uuden taulun, lisää se `TABLES`-sanakirjaan `varmuuskopioi.py`:ssä. Repo on iso (kuvat): kloonaa kevyesti
    `git clone --depth 1 --filter=blob:none --sparse …` (vain juuren tiedostot).
  - Ajon tilan näkee: `gh run list -R tv62/keittokirja-varmuuskopio`. Repo pitää liittää sessioon (`add_repo`).
- **Pyynnöstä ("tee varmuuskopio"):** hae rivit `execute_sql`:llä (`row_to_json(x)::text`) ja per rivi `md5`,
  kirjoita tiedostoon ja tarkista md5:t (kopiointivirheiden varalta), kokoa JSON + luettava HTML ja lähetä käyttäjälle.
  Kuvia ei saa tällä tavalla mukaan. Helpompi: lue tuorein tila suoraan varmuuskopioreposta.
- **Palautus:** rivit ovat JSON-muodossa sellaisinaan → `insert … on conflict (id) do update` (`execute_sql`, dollar-quoting).
  Kuvat palautetaan bucketiin samoille poluille (vaatii salaisen avaimen tai käyttäjän lataamaan sivun kautta).

## Tunnetut asiat ja backlog

- "Leaked password protection" ei ole päällä (saattaa vaatia maksullisen Supabase-tason).
- **Backlog** (käyttäjän toive 11/2026): toteuttamattomat ideat ja suunnitteilla olevat ominaisuudet ovat claude.ai-projektin
  "Keittokirja" dokumentissa `claude/backlog.md` (Projects-työkalu `project_read` / `project_write`), EI tässä tiedostossa.
  - Kun käyttäjä siirtää idean myöhemmäksi, lisää se backlogiin. Kun idea toteutetaan, poista se backlogista ja kuvaa
    toteutus tähän tiedostoon.
  - "Näytä backlog" → lue dokumentti ja listaa lyhyesti. Jos Projects-työkalua ei ole, kerro se käyttäjälle.

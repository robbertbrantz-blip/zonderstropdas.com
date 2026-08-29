# Onderhoud zonderstropdas.com

*Laatst bijgewerkt: 29 augustus 2026 — homepage omgebouwd tot boeklandingspagina*

## Hoe de site in elkaar zit

| Onderdeel | Waar | Details |
|-----------|------|---------|
| Bestanden (bron) | Deze map: `Documents/Claude/Projects/zonderstropdas.com` | Lokale werkkopie |
| Hosting | GitHub Pages | Repo: `github.com/robbertbrantz-blip/zonderstropdas.com`, branch `main`, map `/ (root)` |
| Domein & DNS | Cloudflare (account robbert) | Nameservers: ivan + tia.ns.cloudflare.com |
| Verkeer | Cloudflare proxy (oranje wolkjes) → GitHub Pages | HTTPS via Cloudflare edge-certificaat |
| E-mail | Google Workspace | MX: smtp.google.com — staat los van de website |
| Domeinregistratie | Mijndomein | Alleen nameservers staan daar; verder niets aanraken |

**Belangrijk principe:** de site is statisch (pure HTML). Geen WordPress, geen database, geen plugins, niets dat kan verouderen of gehackt worden. Een wijziging = een bestand aanpassen en opnieuw naar GitHub brengen.

## Een wijziging doorvoeren (de normale route)

1. Pas het bestand aan in deze map (zelf of met Claude in Cowork).
2. Ga naar `github.com/robbertbrantz-blip/zonderstropdas.com` → open het bestand → potlood-icoon (Edit) → plak de nieuwe inhoud → **Commit changes**. Of bij meerdere bestanden: **Add file → Upload files** → sleep de gewijzigde bestanden erin (overschrijven mag) → Commit.
3. Wacht 1–2 minuten: GitHub bouwt automatisch opnieuw.
4. Controleer op https://zonderstropdas.com — zie je de wijziging niet, hard-refresh met Cmd+Shift+R (Cloudflare cachet de pagina's).

> Cache hardnekkig? In Cloudflare: zonderstropdas.com → Caching → **Purge Everything**.

## Bestandsoverzicht

| Bestand | Wat het is |
|---------|------------|
| `index.html` | De hoofdpagina — boekpromotie *AI Won't Wipe My Mother's Ass* (Engels). Alle styling zit erin (geen losse CSS). |
| `about.html` | Het persoonlijke verhaal, Engelse versie |
| `about-nl.html` | Het persoonlijke verhaal, Nederlandse versie (was voorheen `index.html`) |
| `sitemap.xml` | Sitemap voor Google — indienen via Search Console |
| `cover-ai-wont-wipe.jpg` | Voorkant boekcover (1000×1600) |
| `cover-ai-wont-wipe-back.jpg` | Achterkant boekcover met flaptekst |
| `og-image.jpg` | Deelafbeelding 1200×630 voor LinkedIn/WhatsApp/X |
| `look-inside.pdf` | Inkijkexemplaar, 9 pagina's |
| `innercalm.html` | Projectpagina InnerCalm+ (incl. App Store-knop) |
| `bmgrb.html` | Projectpagina BMGRB (incl. knop naar bmgrb.com) |
| `nomoclub.html` | Projectpagina NOMOCLUB Music (incl. link nomoclub.com) |
| `dogsymmetry.html` | Projectpagina Dogsymmetry |
| `logo.png` | Het echte Zonder Stropdas-logo (hand laat stropdas vallen). Ook favicon op alle pagina's. |
| `bluez.jpg` | Bluez op het strand bij Arromanches (sectie "Wat het leven deed") |
| `certificaat-human-machine.jpg` / `certificaat-drivers.jpg` | De twee InnoCentive-certificaten (2012) |
| `straight-from-the-heart.jpg` | Campagnebeeld op de NOMOCLUB-pagina |
| `carel-ahfinance-10jaar.mp4` | Video Carel bij A&H-jubileum (gecomprimeerd, 7,9 MB) |
| `whynot.pdf` | Het *why not*-boekje (2014), downloadbaar vanaf de site |
| `zonderstropdas-verhaal.pdf` | De sitetekst als pdf (wordt opnieuw gegenereerd na tekstwijzigingen) |
| `CNAME` | Bevat alleen `zonderstropdas.com` — **nooit verwijderen**, koppelt het domein aan GitHub Pages |
| `ONDERHOUD.md` | Dit document (staat niet op de site zelf, alleen in de repo/map) |

## Huisstijl & conventies

- **Kleuren** (CSS-variabelen bovenin elk html-bestand, afgeleid van het logo): accent `#00a0c6` (turquoise), accent-diep `#007a99`, tekst `#3f4347` (antraciet), achtergronden `#f8fbfc` / `#eef3f5`.
- **Typografie:** Georgia (serif) voor lopende tekst; systeem-sans voor labels/knoppen.
- **Toon:** eerlijk, geen pitch. Mislukkingen horen erbij. Geen superlatieven zonder bewijs.
- **Terugkerende motieven** (bewust, niet weghalen): de boom die gesnoeid moet worden, het blok hout / de ruwe vierkante bol, de hand die loslaat (logo), "de mens in het proces" (footer op elke pagina).
- **Privacy-afspraken:** kinderen worden niet bij naam genoemd of gedetailleerd ("het is hun verhaal"); de ex-partner wordt niet benoemd; over de vrouw alleen wat zij goed vindt.
- **Projectpagina's:** vaste opbouw — gedachte/visie → wat het is / hoe het werkt → hoe het gebouwd is → contact met "of dit concept verder brengen dan ik?" (de stille verkoopdeur).

## DNS & mail (Cloudflare → zonderstropdas.com)

| Record | Waarde | Status |
|--------|--------|--------|
| A (4×) `@` | 185.199.108–111.153 (GitHub Pages) | Proxied (oranje) |
| CNAME `www` | robbertbrantz-blip.github.io | Proxied |
| MX (5×) | aspmx.l.google.com e.a. | DNS only — **nooit proxien** |
| TXT SPF | `v=spf1 a mx include:spf.mijndomeinhosting.nl include:_spf.google.com ~all` | ✓ |
| TXT `google._domainkey` | DKIM-sleutel Google | ✓ |
| TXT `_dmarc` | `v=DMARC1; p=none;` | ⚠️ zie actiepunten |

## Actiepunten / nog open

- [ ] **DMARC zonderstropdas.com**: rapportage-adres toevoegen → Edit TXT `_dmarc` naar `v=DMARC1; p=none; rua=mailto:robbert@zonderstropdas.com`
- [ ] **Cloudflare SSL/TLS**: Minimum TLS Version op 1.2 zetten (SSL/TLS → Edge Certificates)
- [ ] **HSTS** aanzetten (zelfde scherm) — pas nu alles stabiel via HTTPS draait; max-age 6 maanden, geen preload
- [ ] **DMARC's aanscherpen** (over ±4 weken, na schone rapporten): beide domeinen van `p=none` naar `p=quarantine`
- [ ] **LinkedIn-bericht** aan 1000+ connecties — de site is er klaar voor

## Werken met Claude aan de site

Open Cowork met deze map als project en zeg wat er anders moet — Claude kent dit document en de geschiedenis van de site (geheugen: "Robbert profiel"). Na tekstwijzigingen: vraag ook de pdf opnieuw te genereren en herinner aan de GitHub-upload, anders staat de wijziging alleen lokaal.

---

*Mail bmgrb.com is apart geregeld (11 juni 2026): SPF met Google ✓, DKIM ✓, DMARC `p=none; rua=mailto:info@bmgrb.com` ✓ — rapporten komen binnen op info@bmgrb.com.*


## Boekpagina — wat je zelf nog kunt aanpassen

- **Winkellinks.** De winkels staan als lopende tekst in de sectie *Get the book*. Wil je later directe productlinks, zet die dan in die alinea.
- **LinkedIn.** De knop *Start the conversation on LinkedIn* wijst naar `linkedin.com/in/robbert-brantz-6021399/`.
- **Prijs, ISBN, datum** staan zowel in de feitentabel als in de structured data (`<script type="application/ld+json">` bovenin `index.html`). Pas je er één aan, pas dan allebei aan.

## Na een wijziging aan de structuur

Sitemap opnieuw indienen in Google Search Console: `https://zonderstropdas.com/sitemap.xml`.

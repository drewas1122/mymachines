# Fem «avanserte» teknikker i index.html

Denne forklaringen tar utgangspunkt i hele `index.html`: HTML-strukturen, CSS-oppsettet og React-eksempelet. Teknikkene er litt mer avanserte enn enkel tekst og styling, men forklares med konkrete eksempler fra siden.

## 1. Semantisk HTML og intern navigasjon

Siden bruker elementer som beskriver hvilken rolle innholdet har: `aside` for sidemenyen, `main` for hovedinnholdet, `section` for temaene og `footer` for avslutningen. Dette gjør strukturen lettere å forstå både for utviklere og hjelpemidler som skjermlesere.

Lenkene i sidemenyen kobles til seksjonene gjennom `id`:

```html
<a href="#hva-er-git" class="sidebar-link active">1. Hva er Git?</a>

<section id="hva-er-git">
  <h2>1. Hva er GIT?</h2>
</section>
```

Når brukeren klikker på lenken, navigerer nettleseren til elementet med samme `id`. Dette fungerer uten JavaScript. Klassen `active` gir den første lenken et markert utseende; i dagens kode flyttes ikke markeringen automatisk når man blar eller klikker på andre lenker.

**Hvorfor teknikken brukes:** En lang guide blir enklere å navigere i, og innholdet får en tydelig struktur.

## 2. Flexbox kombinert med en fast sidemeny

Flexbox brukes flere steder for å plassere innhold i rader og fordele plass. Hovedoppsettet har følgende CSS:

```css
.layout {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  width: 280px;
  position: fixed;
  top: 0;
  bottom: 0;
  overflow-y: auto;
}

.main {
  margin-left: 280px;
  flex: 1;
  padding: 64px;
  max-width: 1100px;
}
```

`min-height: 100vh` gjør at oppsettet minst fyller høyden på nettleservinduet. `position: fixed` holder sidemenyen på samme sted når siden rulles. Fordi en fast plassert meny tas ut av den vanlige dokumentflyten, reserverer `margin-left: 280px` plass til den i hovedinnholdet. `overflow-y: auto` lar menyen rulles dersom den blir høyere enn vinduet.

I GitHub-modellen brukes også `display: flex`, `justify-content: space-between` og `align-items: center` til å plassere repository-navnet og handlingsfeltet på hver sin side, sentrert vertikalt. Kommandolistene bruker `gap` og `align-items: baseline` for luft og justering av tekst.

**Hvorfor teknikken brukes:** Layouten blir ryddig uten at hvert element må plasseres med egne koordinater. GitHub- og VS Code-modellene er visuelle illustrasjoner; feltene og «knappene» der utfører ingen Git-operasjoner.

## 3. Responsivt design med media queries

Siden endrer oppsett når vinduet er 768 piksler bredt eller smalere:

```css
@media (max-width: 768px) {
  .sidebar { display: none; }
  .main { margin-left: 0; padding: 32px; }
  .hero h1 { font-size: 36px; }
  .command-list li { flex-direction: column; gap: 4px; }
  .command-name { min-width: auto; }
}
```

Dette kalles en media query. På små skjermer skjules sidemenyen, og den tilhørende venstremargen fjernes. Hovedinnholdet får mindre padding, og overskriften blir mindre. Kommandonavn og forklaring stables under hverandre i stedet for å stå i samme rad.

I HTML-hodet finnes også:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Denne innstillingen gjør at mobilnettleseren bruker enhetens bredde som utgangspunkt for visningen.

**Hvorfor teknikken brukes:** Den samme siden kan tilpasses både datamaskin og mobil. Tilpasningen dekker hovedoppsettet og kommandolistene; de brede GitHub- og VS Code-modellene kan fortsatt trenge flere tilpasninger på svært smale skjermer.

## 4. Visning av kildekode med HTML-entiteter og white-space

Guiden viser både terminalutskrift og HTML-kode. For å vise HTML som tekst må spesialtegn skrives som HTML-entiteter:

```html
<div class="vscode-editor">1  &lt;!DOCTYPE html&gt;
2  &lt;html lang="no"&gt;</div>
```

`&lt;` vises som `<`, og `&gt;` vises som `>`. Dermed viser nettleseren taggene som tekst i stedet for å tolke dem som elementer. Terminaleksempelet bruker også `&#9;`, som representerer et tabulatortegn.

CSS styrer hvordan mellomrom og linjeskift vises:

```css
.code-block-body {
  white-space: pre-wrap;
}

.vscode-editor {
  white-space: pre;
}
```

`pre-wrap` bevarer linjeskift og mellomrom, men tillater at lange linjer brytes. `pre` bevarer formateringen uten automatisk linjebryting. Sistnevnte kan derfor gi innhold som blir bredere enn feltet på små skjermer.

**Hvorfor teknikken brukes:** Kodeeksemplene beholder en lesbar struktur og vises som kode, uten å bli tolket som en del av siden.

## 5. React-komponent med state, hendelser og modulimport

React-eksempelet bruker en komponent kalt `CommitCounter`. En komponent er en funksjon som beskriver en del av brukergrensesnittet. Her lagrer `useState` antall klikk:

```javascript
const [commits, setCommits] = useState(0);
```

`commits` er den gjeldende verdien, som starter på `0`. `setCommits` oppdaterer verdien og får React til å rendre komponenten på nytt.

Knappen kobler en klikkhendelse til oppdateringen:

```javascript
React.createElement("button", {
  type: "button",
  onClick: () => setCommits(count => count + 1)
}, "Lag commit")
```

`count => count + 1` beregner neste verdi fra forrige state. «Nullstill» bruker `setCommits(0)`. Telleren demonstrerer state; den oppretter ingen faktiske Git-commits og tilbakestilles når siden lastes på nytt.

Komponenten bruker `React.createElement` til å lage elementene direkte i JavaScript. Dermed trenger eksempelet ingen JSX-kompilering. React kobles til et bestemt område i HTML-en:

```javascript
createRoot(document.getElementById("react-root"))
  .render(React.createElement(CommitCounter));
```

Resten av siden er vanlig HTML. React styrer bare innholdet i `#react-root`.

Bibliotekene importeres gjennom et modulskript og en import map:

```html
<script type="importmap">
  { "imports": { "react": "https://esm.sh/react@18.2.0" } }
</script>
<script type="module">
  import React, { useState } from "https://esm.sh/react@18.2.0";
  import { createRoot } from "https://esm.sh/react-dom@18.2.0/client?external=react";
</script>
```

Import map-en forteller nettleseren hvilken URL modulnavnet `react` skal vise til. React DOM-importen bruker `external=react`, slik at React hentes via denne mappingen. Versjonene er låst til `18.2.0`. Oppsettet krever internettilgang og en nettleser som støtter import maps.

Komponenten har også `role: "status"` på telleren for å gjøre oppdateringer tilgjengelige for skjermlesere. CSS-regelen `:focus-visible` gir knappene en tydelig fokusramme ved tastaturnavigasjon.

**Hvorfor teknikken brukes:** Siden får en liten interaktiv funksjon som viser komponenter, state og hendelser, uten å kreve et eget byggeoppsett.

# 5 teknikker brukt i index.html

## 1. HTML-struktur og lenker
Siden bruker `main` for hovedinnholdet og `section` for hvert tema. Menyen bruker lenker som `href="#hva-er-git"` for å hoppe til riktig del av siden.

## 2. Flexbox og fast meny
`display: flex` plasserer elementer ved siden av hverandre. Dette brukes blant annet i kommandolistene. `position: fixed` gjør at sidemenyen står stille når du blar.

## 3. Tilpasning til mobil
`@media (max-width: 768px)` endrer utseendet på små skjermer. Menyen skjules, overskriften blir mindre, og kommandolistene vises under hverandre.

## 4. Vise kode som tekst
`&lt;` og `&gt;` viser tegnene `<` og `>`. Da kan siden vise HTML-kode uten at nettleseren bruker den som ekte HTML. `white-space` bevarer mellomrom og linjeskift i eksemplene.

## 5. React-teller
React brukes til telleren nederst på siden. `useState(0)` lagrer tallet, som starter på 0. «Lag commit» øker tallet, og «Nullstill» setter det tilbake til 0. Dette er bare en øvelse og lager ingen ekte Git-commits.

# Estrarre i Leoni d'oro da Wikipedia a riga di comando

## requisiti

- il gigante [curl](https://curl.haxx.se/);
- il comodo [pup](https://github.com/ericchiang/pup);
- il magico [jq](https://stedolan.github.io/jq/);
- l'incredibile [miller](https://github.com/johnkerl/miller).

```bash
# accedi alla pagina
curl "https://it.wikipedia.org/wiki/Leone_d%27oro_al_miglior_film" | \
# filtra soltantto i record della tabella con i leoni e estraila in JSON
pup 'div.mw-parser-output > table.wikitable > tbody > tr:not(:first-child) json{}' | \
# estrai i dati di base
jq '[.[]|{anno:.children[0].children[0].text,titolo:.children[1].children[0].children[0].text,regista:.children[2].children[0].text,nazione:.children[3].children[1].text}]' | \
# convertili in CSV
mlr --j2c filter '($titolo != "")' >./leoni.csv
# conteggia per nazione
<./leoni.csv mlr --csv count-distinct -f nazione -o conteggio then sort -nr conteggio >./leoniPerNazione.csv
# stampa il risultato anche in markdown per questo readme
<./leoniPerNazione.csv mlr --c2m cat  >./leoniPerNazione.md
```

## output (al netto della issue #3)

| nazione | conteggio |
| --- | --- |
| Francia | 10 |
| Italia | 10 |
| Stati Uniti | 8 |
| Regno Unito | 4 |
| Giappone | 3 |
| Germania Ovest | 3 |
| Cina | 3 |
| India | 2 |
| Taiwan | 2 |
| Russia | 2 |
| Cecoslovacchia | 1 |
| Danimarca | 1 |
| Polonia | 1 |
| URSS | 1 |
| Vietnam | 1 |
| Iran | 1 |
| Irlanda | 1 |
| Israele | 1 |
| Corea del Sud | 1 |
| Svezia | 1 |
| Venezuela | 1 |
| Filippine | 1 |
| Messico | 1 |

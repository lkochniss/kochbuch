---
kategorie:
vegan: false
vegetarisch: false
fleisch: false
zeit:
zutaten: []
---

```dataviewjs
const fk = dv.current().kategorie;
const fv = dv.current().vegan;          // Checkbox
const fve = dv.current().vegetarisch;  // Checkbox
const ff = dv.current().fleisch;       // Checkbox
const fz = dv.current().zutaten || [];
const mz = dv.current().zeit;

const rezepte = dv.pages('"Rezepte"').filter(r => {
    // Kategorie prüfen, wenn gesetzt
    if(fk && fk !== "" && r.kategorie != fk) return false;
    
    // Vegan prüfen: nur wenn Häkchen gesetzt
    if(fv === true && r.vegan !== true) return false;

    // Vegetarisch prüfen: nur wenn Häkchen gesetzt
    if(fve === true && r.vegetarisch !== true) return false;

    // Fleisch prüfen: nur wenn Häkchen gesetzt
    if(ff === true && r.fleisch !== true) return false;
    
    // Max. Zeit prüfen
    if(mz != null && r.zeit != null && r.zeit > mz) return false;
    
    // Zutaten prüfen
    if(fz.length > 0 && (!r.zutaten || !fz.every(z => r.zutaten.includes(z)))) return false;

    return true;
});

// Tabelle ausgeben, Name als anklickbarer Link
dv.table(
  ["Rezept", "Art", "Kategorie", "Vegan", "Vegetarisch", "Fleisch", "Zubereitungszeit"],
  rezepte.map(r => [
      r.file.link,
      r.art, r.kategorie, r.vegan, r.vegetarisch, r.fleisch, r.zeit
  ])
);

```





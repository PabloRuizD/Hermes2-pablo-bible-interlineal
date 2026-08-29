---
name: pablo-bible-interlineal
description: Bible reading and analysis using the local `bible-cli` (Bible CLI by Divine-Creative-Ministries, MIT/CC-BY/CC-BY-SA). Reads any passage in any of 30+ translations, returns word-by-word interlinear Hebrew/Greek with Strong's numbers and morphology, computes inner-biblical parallels, supports cross-translation comparison, and handles Dead Sea Scrolls / Septuagint / Targum / Vulgate / Peshitta data via Macula imports. Use when the user asks to read a passage, compare translations, study original-language words, look up Greek/Hebrew lemmas with Strong's, find inner-biblical parallels, query search across the canon, or import per-user translations. Triggers: "lee Génesis 1", "interlineal", "Hebreo", "Griego", "Strong's", "lemma", "forma griega", "paquetes de palabras", "Septuaginta", "Vulgata", "Targum", "Rollos del Mar Muerto", "buscá en la Biblia", "compara traducciones", "parallel", "Peshitta", "Siriaco", "NA28", "Westminster Leningrad Codex".
version: 0.1.0
author: Hermes2 para Pablo Ruiz Danegger (using bible-cli by Divine-Creative-Ministries, MIT/CC-BY)
license: MIT (skill) + CC-BY/CC-BY-SA (data — see DATASOURCES.md)
platforms: [linux, macos, windows]
tags: [biblia, exegesis, interlineal, hebreo, griego, strong, septuaginta, vulga, targum, peshitta, macula]
---

# Pablo — Biblia Interlineal y Multitraducción

Esta skill usa el CLI local `bible` (instalado vía `npm install -g @divine-creative-ministries/bible-cli`) para leer, comparar, analizar y conectar el texto bíblico entre traducciones, idiomas originales y tradiciones religiosas.

## Estado del entorno

Verificá el entorno antes de cada uso:

```bash
bible --version
bible doctor
bible db status
```

Si falta algo:

```bash
bible db download               # ~90 MB (núcleo KJV + tagged data Griego/Hebreo)
bible db download-lxx           # opcional: Septuaginta + OT quotations (CC BY-SA)
bible db download-syntax        # opcional: MACULA clause structure for `bible syntax` (CC BY)
```

Datos por licencia (resumido):

| Dataset | Licencia | Notas |
|---|---|---|
| WLC (Westminster Leningrad Codex, Hebreo) | Public domain | Groves Center / tanach.us |
| Open Scriptures morphology | MIT | hb.openscriptures.org |
| MACULA syntax / Cherith glosses / MARBLE | CC BY 4.0 | Clear Bible + Groves Center + Andi Wu + UBS |
| NA28 / RP Greek | CC BY | incluyendo variantes TR/Byz/SBL |
| Septuagint + OT quotations | CC BY-SA | opcional |
| Targum, Vulgata, Peshitta | importar manual (USFM o TSV de referencia) | |

Más detalles en `docs/DATASOURCES.md`.

## Traducciones disponibles

El CLI trae **KJV por defecto** y soporta muchas traducciones instalables. Las traducciones se importan localmente (archivos propios USFM/TSV); nunca se suben a la nube. Ejemplo:

```bash
bible translation install list                # muestra traducciones disponibles
bible import /path/to/es-RV1909.usfm --id RV1909 --name "Reina Valera 1909"
bible translation list
```

Como práctica **gratis**, BibleNLP provee un corpus paralelo de oraciones versificadas en **muchos idiomas** (biblical-humanities-corpus en Hugging Face, ~$0$ integrables). Ver `awesome-bible-nlp`.

## Comandos principales

| Comando | Qué hace |
|---|---|
| `bible passage "Gen 1:1"` | Lectura simple en la traducción por defecto |
| `bible passage -t KJV "Gen 1:1"` | Especificar traducción por id |
| `bible compare "John 3:16"` | Comparación side-by-side en traducciones instaladas |
| `bible interlinear "John 3:16"` | Word-by-word Griego + Strong's + morphology |
| `bible original "Gen 1:1"` | Texto Hebreo/Griego sin traducción |
| `bible word G2316` | Lexicón + uso estadístico del lema (Theos, etc.) |
| `bible word theos` | Misma búsqueda por transliteración |
| `bible morph "John 1:1"` | Parse completo de cada palabra |
| `bible lemma G25` | Ocurrencias canónicas del lema |
| `bible search "love"` | Full-text search |
| `bible grep-morph "V-AIA-3S"` | Búsqueda morfológica |
| `bible pattern G25+G2316` | Secuencia de Strong's en orden |
| `bible syntax "Gen 1:1"` | (requiere `db download-syntax`) Cláusulas MACULA |
| `bible read Jn / bible read Gen` | Libro entero |
| `bible freq --word "agape"` | Frecuencia en el canon |
| `bible doctor` | Diagnóstico |
| `bible db status` | Estado de las bases |

## Procedimiento de la skill

### Paso 1 — Entender el pedido

Clasificar la consulta:

| Tipo | Comando(s) |
|---|---|
| Lectura simple | `passage` |
| Comparación entre traducciones | `compare` |
| Palabra interlineal con lemas | `interlinear` + `word` + `lemma` |
| Análisis morfológico | `morph` + `grep-morph` |
| Estudios de distribución canónica | `freq --word` + `lemma` |
| Parallels intrabíblicos | `pattern` con secuencia de Strong's |
| Texto original sin traducir | `original` |
| Conexión con textos fuentes (Rollos, LXX, Vulgata, Targums, Peshitta) | `compare` con traducciones importadas o `original` + tabla manual |
| Importar nueva traducción (USFM / TSV) | `import` |

### Paso 2 — Identificar el pasaje

El usuario menciona libro/capítulo/versículo o rango. Aceptar notación:

- `John 3:16` | `Juan 3:16` | `John 3:16-18` | `Jn 3.16` | `Gen 1.1-3.5` | `Tora Bereshit 1`

CLI acepta formatos canónicos: `Gen 1:1`, `Matthew 5:1-12`, `1 Cor 13`.

Si el usuario da referencias con nomenclatura no estándar (como Bereshit, Jn, Apoc), convertir a la forma corta CCL que el CLI conoce.

### Paso 3 — Ejecutar comando(s)

Reglas:

- **Encadenar** comandos cuando sea útil: leer → ver interlineal → buscar el lema → ver las otras ocurrencias.
- **Verificar traducciones instaladas antes** con `translation list` antes de comparar.
- Si pide "interlineal" pero no especifica idioma, por defecto Griego para NT y Hebreo para OT.
- Si pide "compara traducciones", traer primero `translation list` para mostrarle las opciones al usuario.

### Paso 4 — Interpretar el output

El CLI devuelve texto con formato determinístico. **Interpretá con cuidado**:

- **Strong's number**: cada ocurrencia tiene un identificador estable (G2316 = θεός, H0430 = אלוהים). Estos son anclas confiables.
- **Morph codes**: `V-AIA-3S` = Verb, Aorist, Indicative, Active, 3rd person, Singular. Son POSIX codes (más o menos).
- **Qere/Ketiv**: el CLI default es Qere. Para ver Ketiv explicit, especificar `bible original -k Ketiv`.

### Paso 5 — Articular respuesta

No devolver solo el raw output. **Construir** la respuesta en formato:

1. Texto original (si pidió interlineal).
2. Texto en la traducción por defecto (si pidió lectura).
3. Análisis léxico (1-2 palabras del texto más interesantes, con Strong's + análisis).
4. Contextualización (parallels canónicos cuando aplique).
5. Conexión inter-traducción cuando pida comparación.

## Anti-patrones

| Anti-patrón | Corrección |
|---|---|
| Inventar datos de Strong's / morfología de memoria | **Toda cifra debe venir de `bible word`, `bible morph` o `bible interlinear`** — no de training data |
| Confundir Septuaginta (LXX) con KJV | LXX es traducción griega del AT, ~200-100 AC; KJV es inglés ~1611 DC. No superponer. |
| Usar "Rollos del Mar Muerto" para cualquier variante textual | DSS solo aplica a textos del periodo de Qumrán; ~220 manuscritos. Para variantes textuales actuales, recurrir a LXX, Vulgata, Masoretic Text. |
| Buscar Strong's de palabra castellana | Mapeá primero a su forma original (griego o hebreo), luego `bible word` con Strong's o transliteración |
| Importar traducción sin chequear licencia | El CLI acepta USFM del usuario; chequear que tenés derecho a redistribuir |

## Conexión con otras skills

- `pablo-exegesis-biblia-docs` → metodología de las 4 tradiciones exegéticas.
- `pablo-exegesis-judia-pardes` → análisis PaRDeS del texto recién extraído.
- `pablo-exegesis-alter` → análisis literario con texto original cargado.
- `pablo-exegesis-meynet` → análisis estructural con detección de quiasmos en el texto hebreo/griego.
- `pablo-exegesis-robbins` → análisis socio-cultural con vocabulario cargado.
- `pablo-deliver-files` → si el usuario quiere entregar el output como archivo Markdown/PDF.
- `reportes-analiticos` o `longform-research-report` → si querés producir un documento extenso combinando Biblia + análisis propio.

## Referencias de soporte instaladas

- `references/bible-cli-cheatsheet.md` — uso real del CLI `bible`, incluidas trampas de uso (edición vs traducción, interlineal vs frecuencia, etc.).
- `references/spanish-bible-sources.md` — fuentes verificadas para traducciones al español (bible.com, BibleGateway, bibliavida).Útil porque el CLI no trae traducciones al español por defecto; explica qué URLs funcionan y cuáles fallan (wikisource 404, bibliatodo 403).

## Cómo probar

Pegame algo así:

- *"Leé Génesis 1:1 interlineal en hebreo"*.
- *"Compará Juan 3:16 en KJV, NVI y la versión que tengas"*.
- *"Buscá todas las ocurrencias de G25 (ἀγαπάω) en el NT y decime qué patrones ves"*.
- *"Dame el texto griego de Mateo 5:1-12 con morfología"*.
- *"Importá el archivo RV1909 que te paso"*.
- *"Buscame paralelos intrabíblicos para Éxodo 3:14"*.

## Versión

**v0.1.0** — Skill nueva. Funciona con `bible-cli` instalado vía npm.

Por mejorar:
- Catálogo de traducciones instaladas con un toque.
- Test suite (pruebas automáticamente que los comandos devuelven resultados esperados).
- Plantillas de output por "tipo" (lectura, estudio léxico, comparación, parallels).

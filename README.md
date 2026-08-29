# pablo-bible-interlineal

> **Parte del set `Hermes2-` de [PabloRuizD](https://github.com/PabloRuizD).** Esta skill fue generada con asistencia de Hermes2 para uso del agente personal de Pablo Ruiz Danegger (Instituto Técnico UNT Tucumán).

📂 **Categoría:** 🕊 Religiones y Filosofía
🏷️ **Tipo:** Wrapper (fork simbólico)

## Descripción

Bible reading and analysis using the local `bible-cli` (Bible CLI by Divine-Creative-Ministries, MIT/CC-BY/CC-BY-SA). Reads any passage in any of 30+ translations, returns word-by-word interlinear Hebrew/Greek with Strong's numbers and morphology, computes inner-biblical parallels, supports cross-translation comparison, and handles Dead Sea Scrolls / Septuagint / Targum / Vulgate / Peshitta data via Macula imports. Use when the user asks to read a passage, compare translations, study original-language words, look up Greek/Hebrew lemmas with Strong's, find inner-biblical parallels, query search across the canon, or import per-user translations. Triggers: "lee Génesis 1", "interlineal", "Hebreo", "Griego", "Strong's", "lemma", "forma griega", "paquetes de palabras", "Septuaginta", "Vulgata", "Targum", "Rollos del Mar Muerto", "buscá en la Biblia", "compara traducciones", "parallel", "Peshitta", "Siriaco", "NA28", "Westminster Leningrad Codex".

## Origen

- **Upstream:** bible-cli v0.2.1 (offline)
- **Autor del port:** Pablo Agustín Ruiz Danegger con Hermes2 (agosto 2026)
- **Propósito:** marcar y disponibilizar esta skill para el agente personal Hermes2, en una cuenta separada para evitar confusión con otros repos de Pablo.

## Instalación

### Opción A — Descarga directa

```bash
git clone https://github.com/PabloRuizD/Hermes2-pablo-bible-interlineal.git
mkdir -p ~/.hermes/skills/pablo-bible-interlineal
cp -r Hermes2-pablo-bible-interlineal/* ~/.hermes/skills/pablo-bible-interlineal/
```

### Opción B — Como submódulo

```bash
mkdir -p ~/.hermes/skills/pablo-bible-interlineal
git submodule add https://github.com/PabloRuizD/Hermes2-pablo-bible-interlineal.git ~/.hermes/skills/pablo-bible-interlineal/source
```

## Estructura

```
pablo-bible-interlineal/
├── SKILL.md           # Definición técnica (frontmatter YAML + cuerpo Markdown)
├── README.md          # Este archivo
├── LICENSE            # Licencia MIT
└── .gitignore
```

Si la skill incluye datos locales (textos, corpus, datasets), los encontrarás en subcarpetas dentro del repo según se defina en `SKILL.md`.

## Uso

Una vez instalada en `~/.hermes/skills/pablo-bible-interlineal/`, el agente Hermes2 carga automáticamente la skill y la activa cuando tu pedido contenga los triggers listados en `SKILL.md`.

Ejemplo:
```
Usuario: "<algún trigger de la skill>"
Hermes2: invoca la skill, carga references/, ejecuta scripts/ si aplica.
```

## Licencia

- **Código (SKILL.md, README.md, scripts propios):** MIT — ver `LICENSE`.
- **Datos del upstream (si aplica):** ver la sección "Origen" arriba; cada upstream mantiene su propia licencia (CC-BY, CC-BY-SA, ODbL, MIT, o Public Domain según el caso).

## Aviso

Esta skill fue generada con asistencia de IA. Verificar los outputs antes de uso en producción. Para correcciones o ampliaciones, abrir un issue en el repositorio.

---

*Generado: 2026-08-29 · Hermes2 para Pablo Ruiz Danegger*

# Legalize CO

### Legislación colombiana consolidada en Markdown, versionada con Git.

Cada norma es un fichero. Cada reforma documentada es un commit.

**Fuente oficial:** [SUIN-Juriscol](https://www.suin-juriscol.gov.co) — Sistema Único de Información Normativa del Ministerio de Justicia y del Derecho de la República de Colombia.

**Cobertura:** Leyes · Decretos · Actos Legislativos · Resoluciones · 1887–presente

Forma parte del proyecto [Legalize](https://github.com/legalize-dev/legalize) · [legalize.dev](https://legalize.dev)

> **Fase inicial** — Este repositorio está en desarrollo activo. La estructura de los ficheros, el historial de commits y el contenido pueden cambiar significativamente, incluyendo regeneraciones completas.

## Inicio rápido

```bash
# Clonar la legislación colombiana
git clone https://github.com/legalize-dev/legalize-co.git

# Leer la Ley 57 de 1887 — adopción de los Códigos nacionales
less co/LEY-57-1887.md

# Buscar un artículo
grep -A 5 "Artículo 1" co/LEY-57-1887.md

# Ver el historial de reformas de una ley (commits por artículo modificado)
git log --oneline -- co/LEY-57-1887.md

# Ver el diff de la reforma de 1976
git log -p --grep "1976" -- co/LEY-57-1887.md
```

## Estructura

```
co/
  LEY-57-1887.md               — Ley 57 de 1887 (adopción de Códigos)
  ACTO-LEGISLATIVO-1-1910.md   — Acto Legislativo 1 de 1910
  DECRETO-2900-1966.md         — Decreto 2900 de 1966
  DECRETO-54-1993.md           — Decreto 54 de 1993
  ...                          — ~ decenas de miles de normas
```

La estructura es **plana** — un solo directorio por país, sin subdirectorios por rango. El identificador sigue la plantilla `{TIPO}-{número}-{año}` (por ejemplo `LEY-100-1993`, `DECRETO-1072-2015`, `ACTO-LEGISLATIVO-2-2020`). El tipo de norma está en el frontmatter YAML de cada fichero:

| Rank (frontmatter) | Tipo |
|---|---|
| `ley` | Ley del Congreso |
| `acto_legislativo` | Acto Legislativo (reforma constitucional) |
| `decreto` | Decreto del Ejecutivo |
| `resolucion` | Resolución |
| `otro` | Otras categorías SUIN |

## Formato

Cada fichero es Markdown con frontmatter YAML:

```yaml
---
title: "Sobre adopción de Códigos y unificación de la legislación nacional"
identifier: "LEY-57-1887"
country: "co"
rank: "ley"
publication_date: "1887-04-22"
last_updated: "1976-01-18"
status: "in_force"
source: "https://www.suin-juriscol.gov.co/viewDocument.asp?id=1789030"
department: "CONSEJO NACIONAL LEGISLATIVO"
document_status_raw: "Vigente"
gazette_reference: "DIARIO OFICIAL. AÑO XXIII. N.7019. 20, ABRIL, 1887. PÁG.1"
gazette_number: "7021"
gazette_page: "445"
subtype: "LEY ORDINARIA"
subjects: "Administración de justicia | Penal"
modification_count: "102"
modification_summary: "Reformado [Artículo 49 LEY 153 de 1887](...) · Derogado [Artículo 31 LEY 1 de 1976](...) · ..."
---
```

El cuerpo del Markdown contiene el texto normativo consolidado (títulos, capítulos, secciones, artículos y firmas).

### Historial de reformas por artículo

SUIN-Juriscol embebe, para cada artículo que ha sido modificado, un bloque `LEGISLACIÓN ANTERIOR` con el texto vigente antes de la última reforma y el rango de vigencia (`Vigente desde: D1 y hasta el: D2`). El pipeline extrae estos bloques y genera **un commit `[reforma]` por artículo y fecha de corte**, además del commit `[bootstrap]` inicial.

Cobertura típica: ~93% de las reformas documentadas quedan reconstruidas como commits reales con su diff correspondiente; el resto queda preservado como índice de referencias cruzadas en el campo `modification_summary` del frontmatter.

### Fechas pre-1970

GitHub rechaza commits con timestamps anteriores al epoch Unix (1970-01-02). Los commits con fechas reales anteriores a 1970 se publican con `git-date = 1970-01-02`, pero el valor real está preservado en el trailer `Source-Date: YYYY-MM-DD` del mensaje del commit y en los campos `publication_date` / `last_updated` del frontmatter. El orden cronológico está garantizado por la secuencia del `git log`.

### Tipos de commit

- `[bootstrap]` — primera publicación de la norma en el repositorio
- `[reforma]` — modificación de uno o más artículos
- `[nueva]` / `[derogacion]` / `[correccion]` — detectadas por actualizaciones posteriores

Cada commit trae los trailers `Source-Id`, `Source-Date` y `Norm-Id` para reconstruir la trazabilidad con `git log`.

## Fuente

Los datos provienen de [SUIN-Juriscol](https://www.suin-juriscol.gov.co), el Sistema Único de Información Normativa operado por el Ministerio de Justicia y del Derecho de la República de Colombia. Es el sistema oficial de consulta normativa del Estado colombiano.

## Licencia

[MIT](LICENSE) — el contenido de las normas es del dominio público colombiano; el código del pipeline es MIT.

---

Generado por el pipeline [legalize-pipeline](https://github.com/legalize-dev/legalize-pipeline).

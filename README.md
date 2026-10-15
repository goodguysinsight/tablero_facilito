# Tablero de Precios de Combustibles — Grifos (OSINERGMIN · Facilito)

Tablero dinámico e interactivo de precios de combustibles por establecimiento (grifos y estaciones de servicio), con mapa coroplético por nivel territorial, estadísticos de precio y ranking de establecimientos más baratos. Elaboración: **Good Guys Insight**.

## Demo

El tablero es una única página autocontenida (`index.html`). Para publicarla:

1. En este repositorio: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, rama `main`, carpeta `/ (root)`.
2. La página quedará disponible en `https://goodguysinsight.github.io/tablero_facilito/`.

También puede abrirse descargando `index.html` y abriéndolo en el navegador.

## Funcionalidades

- **Selector de combustible:** Gasohol Regular, Gasohol Premium, Gasolina 84, Diesel B5 S‑50.
- **Tres niveles territoriales:** Departamental, Provincial y Distrital, con **filtros en cascada** (Departamento → Provincia → Distrito).
- **Mapa coroplético** coloreado por el **precio mínimo** del combustible elegido (tono más oscuro = más barato).
- **Panel por zona:** precio **mínimo, promedio, máximo y mediana**, número de establecimientos, **brecha** de precios (máx − mín) y **ahorro potencial** por galón.
- **Marcadores por grifo** con *popup*: nombre, distrito, dirección, todos sus precios y botón **"Ver en Google Maps"** (GPS).
- **Top 10 más barato:** ranking dinámico de establecimientos de la zona, coloreado de oscuro (más barato) a pálido (más caro), con enlace al mapa.
- **Clic en una zona** la fija en el panel y en el Top 10; el botón **"Ver todo"** regresa a la vista nacional.
- Diseño **responsive** para escritorio y celular.

## Fuente de datos

- **Precios:** OSINERGMIN — plataforma **Facilito** (https://www.facilito.gob.pe). Precios reportados por los propios operadores de los establecimientos.
- **Cartografía (límites distritales y provinciales):** Registro Nacional de Límites (**RENLIM**) — Secretaría de Demarcación y Organización Territorial (**SDOT – PCM**), vigencia 2023 (referenciales).

### Cobertura (importante)

Facilito solo publica **coordenadas geográficas de una parte** de los grifos. Por ello la cobertura actual del tablero se concentra en la **Amazonía** (Ucayali, Loreto, San Martín, Madre de Dios, Amazonas): **615 establecimientos geolocalizados**. La mayoría de grifos de la **costa y la sierra** (Lima incluida) **no tienen coordenadas cargadas** en Facilito, por lo que no aparecen en el mapa. El tablero **no representa la totalidad** de establecimientos del país.

El motor está preparado para un dataset nacional: basta reemplazar los datos (ver esquema) para ampliar la cobertura.

## Datos

`precios_grifos_facilito.csv` — dataset extraído, una fila por establecimiento:

| Columna | Descripción |
|---|---|
| `codigo_osinergmin` | Código del establecimiento en Osinergmin |
| `unidad` | Razón social / nombre comercial |
| `direccion` | Dirección declarada |
| `ubigeo` | UBIGEO (6 dígitos) del distrito |
| `departamento`, `provincia`, `distrito` | Nombres territoriales |
| `latitud`, `longitud` | Coordenadas (WGS84) |
| `regular`, `premium`, `g84`, `diesel` | Precio en soles por galón (vacío si no se reporta) |

## Metodología de extracción

Los datos provienen del endpoint del mapa de Facilito (`MapaAction.do?method=getPoints`), que devuelve los establecimientos **geolocalizados** dentro de un *bounding‑box* con sus coordenadas, nombre, dirección y precios por producto. La extracción se realiza por **teselado** del territorio con unión por `codigoOsinergmin`. Cada grifo se asigna a su distrito mediante *point‑in‑polygon* sobre la geometría oficial RENLIM.

## Limitaciones y trabajo futuro

- Cobertura limitada a establecimientos con coordenadas (ver *Cobertura*).
- Pendiente: **GNV**, **GLP automotor** y **GLP envasado** (viven en buscadores separados de Facilito).
- **Histórico de precios:** el tablero muestra una foto puntual; requiere almacenar *snapshots* periódicos para construir la serie.

## Créditos

Elaboración: **Good Guys Insight**. Datos: OSINERGMIN (Facilito). Límites: SDOT–PCM (RENLIM). Última extracción: **14 de octubre de 2026**.

# Tablero de Precios de Combustibles — Grifos (OSINERGMIN · Facilito)

Tablero dinámico e interactivo de precios de combustibles por establecimiento (grifos y estaciones de servicio), con mapa coroplético por nivel territorial, estadísticos de precio y ranking de establecimientos más baratos. Elaboración: **Good Guys Insight**.

## Demo

El tablero es una única página autocontenida (`index.html`). Para publicarla:

1. En este repositorio: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, rama `main`, carpeta `/ (root)`.
2. La página quedará disponible en `https://goodguysinsight.github.io/tablero_facilito/`.

También puede abrirse descargando `index.html` y abriéndolo en el navegador.

## Funcionalidades

- **Selector de combustible:** Gasohol Regular, Gasohol Premium, Gasolina 84, Diesel B5 S‑50 (soles por galón) y **Gas Natural Vehicular – GNV** (soles por m³).
- **Tres niveles territoriales:** Departamental, Provincial y Distrital, con **filtros en cascada** (Departamento → Provincia → Distrito).
- **Mapa coroplético** coloreado por el **precio mínimo** del combustible elegido (tono más oscuro = más barato).
- **Panel por zona:** precio **mínimo, promedio, máximo y mediana**, número de establecimientos, **brecha** de precios (máx − mín) y **ahorro potencial** por galón.
- **Marcadores por grifo** con *popup*: nombre, distrito, dirección, todos sus precios y botón **"Ver en Google Maps"** (GPS).
- **Top 10 más barato:** ranking dinámico de establecimientos de la zona, coloreado de oscuro (más barato) a pálido (más caro), con enlace al mapa.
- **Clic en una zona** la fija en el panel y en el Top 10; el botón **"Ver todo"** regresa a la vista nacional.
- Diseño **responsive** para escritorio y celular.

Dos pestañas:
- **Mapa de precios por grifo** — lo descrito arriba (ubicación nacional + precio donde existe).
- **Evolución y distribución (nacional)** — serie **diaria** del precio (mediana con banda **P25–P75** y líneas de **mínimo/máximo**), **histograma** de distribución y resumen, para los **6 combustibles** (incluye GLP granel y envasado), a partir de la lista de precios diaria de OSINERGMIN (**1 ene – 28 feb 2026**, datos anonimizados sin ubicación).

## Fuente de datos

- **Ubicación de establecimientos:** **registro oficial georreferenciado de OSINERGMIN** — capa ArcGIS `12_BD_Dash_HL_GrifosEESS_gdb` (Hidrocarburos Líquidos — Grifos/EESS), con ubigeo, coordenadas y `COD_OSINERGMIN`.
- **Precios (mapa):** OSINERGMIN — plataforma **Facilito** (https://www.facilito.gob.pe). Precios reportados por los propios operadores de los establecimientos (vía plataforma PRICE).
- **Precios (evolución nacional):** OSINERGMIN — **Lista de precios de combustibles, diaria** (datos abiertos), serie ene–feb 2026, anonimizada (sin ubicación); alimenta la pestaña de evolución y distribución.
- **Cartografía (límites distritales y provinciales):** Registro Nacional de Límites (**RENLIM**) — Secretaría de Demarcación y Organización Territorial (**SDOT – PCM**), vigencia 2023 (referenciales).

### Cobertura

**Cobertura nacional de precios:** ~**5 450 grifos** con precio en **26 departamentos · 188 provincias · 932 distritos**, obtenidos del buscador público de Facilito barriendo por provincia (196). Los coropléticos, estadísticos (mín/prom/máx/mediana, brecha, ahorro) y el **Top‑10** se calculan por **ubigeo** con esos grifos, a los 3 niveles.

- **Mapa de puntos:** cada grifo con precio se pinta por su precio cuando se le pudo asignar coordenada. El enlace del grifo en Facilito expone su **código OSINERGMIN**, con el que el cruce contra el **registro georreferenciado de OSINERGMIN** es **exacto** (clave única); así **~2 900 grifos con precio** tienen punto en el mapa. Los grifos con precio cuyo código **aún no figura en el registro público georreferenciado** (establecimientos nuevos) no tienen coordenada oficial, **pero sí entran en todos los cálculos por ubigeo**. Los grifos registrados sin precio se muestran en gris.
- **GNV** en S/ por m³. **GLP** y la **evolución diaria** se ven en la pestaña de distribución nacional (datos anonimizados sin ubicación).

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
| `gnv_m3` | Precio de Gas Natural Vehicular en soles por m³ (vacío si no aplica) |
| `tiene_precio` | "Si" si el establecimiento tiene al menos un precio reportado; "No" si es solo ubicación (registro) |

## Metodología de extracción

Los precios se obtienen de la tabla pública de Facilito (`PreciosCombustibleAutomotorAction.do`), consultando **cada una de las 196 provincias** con el comodín de distrito, lo que devuelve **todos** los establecimientos de la provincia con nombre, dirección, teléfono, precio por producto y **código OSINERGMIN** (del enlace de mapa de cada fila). El ubigeo distrital se asigna con la geometría oficial RENLIM. Para la **coordenada** de cada grifo se cruza el código OSINERGMIN contra el **registro georreferenciado de OSINERGMIN** (capa ArcGIS), cruce exacto por clave única.

## Limitaciones y trabajo futuro

- **Puntos en el mapa:** los grifos con precio cuyo código aún no figura en el registro público georreferenciado (establecimientos nuevos) no tienen coordenada oficial; se mapean ~2 900 de ~5 450. Todos entran en los cálculos por ubigeo. Alternativas para completar el resto: coordenadas del propio mapa de Facilito, geocodificación de direcciones o centroide distrital (aproximado).
- **GLP automotor y envasado:** Facilito no expone sus coordenadas, por lo que no se pueden mapear.
- **Histórico de precios:** el tablero muestra una foto puntual; requiere almacenar *snapshots* periódicos para construir la serie.

## Créditos

Elaboración: **Good Guys Insight**. Datos: OSINERGMIN (Facilito). Límites: SDOT–PCM (RENLIM). Última extracción: **14 de octubre de 2026**.

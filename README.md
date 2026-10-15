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

## Fuente de datos

- **Ubicación de establecimientos:** **registro oficial georreferenciado de OSINERGMIN** — capa ArcGIS `12_BD_Dash_HL_GrifosEESS_gdb` (Hidrocarburos Líquidos — Grifos/EESS), con ubigeo, coordenadas y `COD_OSINERGMIN`.
- **Precios:** OSINERGMIN — plataforma **Facilito** (https://www.facilito.gob.pe). Precios reportados por los propios operadores de los establecimientos.
- **Cartografía (límites distritales y provinciales):** Registro Nacional de Límites (**RENLIM**) — Secretaría de Demarcación y Organización Territorial (**SDOT – PCM**), vigencia 2023 (referenciales).

### Cobertura (importante)

El mapa muestra **3 052 grifos y estaciones de servicio** del registro oficial de OSINERGMIN (ubicación nacional por ubigeo). El **precio** está disponible en **967** de ellos:

- **Líquidos** (gasoholes, gasolina 84, diesel): **615** grifos geolocalizados en Facilito, concentrados en la **Amazonía** (Ucayali, Loreto, San Martín, Madre de Dios, Amazonas).
- **Gas Natural Vehicular (GNV):** **352** grifos, con cobertura de **Lima, Callao, Ica y costa norte** (corredor del gas natural).

Los grifos **sin precio** se muestran como punto gris ("solo ubicación"). Facilito no publica coordenadas del resto de precios ni de **GLP**, así que el precio nacional no es completo: **los coropléticos y estadísticos usan únicamente los grifos con precio**. El motor está preparado para un dataset nacional de precios (ver esquema): basta cargarlo para que el precio cubra todo el país.

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

Los datos provienen del endpoint del mapa de Facilito (`MapaAction.do?method=getPoints`), que devuelve los establecimientos **geolocalizados** dentro de un *bounding‑box* con sus coordenadas, nombre, dirección y precios por producto. La extracción se realiza por **teselado** del territorio con unión por `codigoOsinergmin`. Cada grifo se asigna a su distrito mediante *point‑in‑polygon* sobre la geometría oficial RENLIM.

## Limitaciones y trabajo futuro

- Cobertura limitada a establecimientos con coordenadas (ver *Cobertura*).
- **GLP automotor y envasado:** Facilito no expone sus coordenadas, por lo que no se pueden mapear.
- **Histórico de precios:** el tablero muestra una foto puntual; requiere almacenar *snapshots* periódicos para construir la serie.

## Créditos

Elaboración: **Good Guys Insight**. Datos: OSINERGMIN (Facilito). Límites: SDOT–PCM (RENLIM). Última extracción: **14 de octubre de 2026**.

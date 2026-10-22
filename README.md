# Mapa TANQUEA SMART — Precios de combustibles por grifo (OSINERGMIN · Facilito)

**Mapa TANQUEA SMART** es un tablero dinámico e interactivo de precios de combustibles por establecimiento (grifos y estaciones de servicio), con mapa coroplético por nivel territorial, estadísticos de precio y ranking de establecimientos más baratos. Elaboración: **Good Guys Insight**.

## Demo

El tablero es una única página autocontenida (`index.html`). Para publicarla:

1. En este repositorio: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, rama `main`, carpeta `/ (root)`.
2. La página quedará disponible en `https://goodguysinsight.github.io/tablero_tanquea_smart/`.

> El repositorio se llamaba antes `tablero_facilito`; la dirección anterior (`…/tablero_facilito/`) ya no está disponible.

También puede abrirse descargando `index.html` y abriéndolo en el navegador.

## Funcionalidades

- **Selector de combustible:** Gasohol Premium, Gasohol Regular, Gasolina 84, Diesel B5 S‑50 y **GLP Automotor** (soles por galón); **Gas Natural Vehicular – GNV** (soles por m³); y **GLP Envasado** (soles por balón de 10 kg).
- **Tres niveles territoriales:** Departamental, Provincial y Distrital, con **filtros en cascada** (Departamento → Provincia → Distrito).
- **Mapa coroplético** coloreado por el **precio mínimo** del combustible elegido (tono más oscuro = más barato).
- **Panel por zona:** precio **mínimo, promedio, máximo y mediana**, número de establecimientos (y cuántos aparecen en el mapa), **brecha** de precios (máx − mín) y **ahorro potencial referencial** por galón. El ahorro es referencial: puede variar según la cantidad adquirida, la distancia al establecimiento y la vigencia de los precios.
- **Marcadores por grifo** con *popup*: nombre, distrito, dirección, todos sus precios y botón **"Ver en Google Maps"** (GPS).
- **Puntos por grifo según su precio** dentro de la zona visible: verde = precio mínimo; azul = del 2.º más barato al 25 %; celeste = 25 %–50 %; azul claro = 50 %–75 %; rojo = 75 % a más. La leyenda muestra los cortes en soles.
- **Top 10 más barato:** ranking dinámico de establecimientos de la zona, con los mismos colores de los puntos y enlace al mapa (o la etiqueta «sin ubicación» si el grifo no tiene coordenadas).
- **Clic en una zona** o filtro de ubicación: la fija en el panel y en el Top 10, la **encuadra completa** en la pantalla y **atenúa** las regiones de alrededor; el botón **"Ver todo"** regresa a la vista nacional.
- **Fecha de actualización** visible sobre el mapa.
- **Modo claro / oscuro** (botón «◐ Modo»): respeta el tema del dispositivo y recuerda la elección del visitante.
- Diseño **responsive** para escritorio y celular; leyenda plegable en pantallas bajas.

Tres pestañas:
- **Mapa de precios por grifo** — lo descrito arriba (ubicación nacional + precio donde existe).
- **Evolución y distribución (nacional)** — serie **diaria** del precio (mediana en verde con banda **P25–P75** y líneas **rojas punteadas** de mínimo/máximo), **histograma** de distribución y resumen, para los **6 combustibles** (incluye GLP granel y envasado), a partir de la lista de precios diaria de OSINERGMIN (**1 ene – 28 feb 2026**, datos anonimizados sin ubicación).
- **Distribuidores mayoristas (DMAY)** — tabla filtrable (por producto y departamento) de los precios de **venta mayorista** de los ~120 distribuidores, con estadísticos (mín/mediana/promedio/máx). Fuente: OSINERGMIN — SCOP/PRICE (DMAY), corte 16‑oct‑2026. *Nota: incluye precios de venta en planta/refinería, distintos del precio minorista en estación.*

## Fuente de datos

- **Ubicación de establecimientos:** **registro oficial georreferenciado de OSINERGMIN** — capa ArcGIS `12_BD_Dash_HL_GrifosEESS_gdb` (Hidrocarburos Líquidos — Grifos/EESS), con ubigeo, coordenadas y `COD_OSINERGMIN`.
- **Precios (mapa) — líquidos, GNV y GLP automotor:** OSINERGMIN — **SCOP/PRICE**, archivo oficial de **últimos precios registrados por EVPC** (Establecimientos de Venta al Público de Combustibles), con `CODIGO_OSINERGMIN`, RUC, dirección, producto, precio y fecha de registro (corte **16‑oct‑2026**).
- **Precios (mapa) — GLP envasado:** OSINERGMIN — plataforma **Facilito** (buscador de GLP envasado, locales de venta), barrida por provincia; balón de 10 kg como precio de referencia.
- **Precios (evolución nacional):** OSINERGMIN — **Lista de precios de combustibles, diaria** (datos abiertos), serie ene–feb 2026, anonimizada (sin ubicación); alimenta la pestaña de evolución y distribución.
- **Cartografía (límites distritales y provinciales):** Registro Nacional de Límites (**RENLIM**) — Secretaría de Demarcación y Organización Territorial (**SDOT – PCM**), vigencia 2023 (referenciales).

### Cobertura

**Cobertura nacional de precios:** líquidos, GNV, **GLP automotor** y **GLP envasado** en **25 departamentos · 187 provincias · 1008 distritos**, obtenidos del buscador público de Facilito barriendo por provincia (196). Los coropléticos, estadísticos (mín/prom/máx/mediana, brecha, ahorro) y el **Top‑10** se calculan por **ubigeo** con esos establecimientos, a los 3 niveles.

- **Mapa de puntos:** cada establecimiento con precio se pinta por su precio cuando se le pudo asignar coordenada. El enlace del establecimiento en Facilito expone su **código OSINERGMIN**, con el que el cruce contra el **registro georreferenciado de OSINERGMIN** es **exacto** (clave única). Los establecimientos nuevos cuyo código **aún no figura en el registro público georreferenciado** no tienen coordenada oficial, **pero sí entran en todos los cálculos por ubigeo**. Los grifos registrados sin precio se muestran en gris.
- **Unidades:** líquidos y **GLP automotor** en S/ por galón; **GNV** en S/ por m³; **GLP envasado** en S/ por balón de 10 kg. La **evolución diaria** nacional se ve en la otra pestaña (datos anonimizados sin ubicación).

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
| `glp_auto_gal` | Precio de GLP Automotor en soles por galón (vacío si no aplica) |
| `glp_env_balon10kg` | Precio de GLP Envasado, balón de 10 kg, en soles (vacío si no aplica) |
| `tiene_precio` | "Si" si el establecimiento tiene al menos un precio reportado; "No" si es solo ubicación (registro) |

## Metodología de extracción

Los precios se obtienen de la tabla pública de Facilito (`PreciosCombustibleAutomotorAction.do`), consultando **cada una de las 196 provincias** con el comodín de distrito, lo que devuelve **todos** los establecimientos de la provincia con nombre, dirección, teléfono, precio por producto y **código OSINERGMIN** (del enlace de mapa de cada fila). El ubigeo distrital se asigna con la geometría oficial RENLIM. Para la **coordenada** de cada grifo se cruza el código OSINERGMIN contra el **registro georreferenciado de OSINERGMIN** (capa ArcGIS), cruce exacto por clave única.

## Limitaciones y trabajo futuro

- **Puntos en el mapa:** los grifos con precio cuyo código aún no figura en el registro público georreferenciado (establecimientos nuevos) no tienen coordenada oficial; se mapean ~2 900 de ~5 450. Todos entran en los cálculos por ubigeo. Alternativas para completar el resto: coordenadas del propio mapa de Facilito, geocodificación de direcciones o centroide distrital (aproximado).
- **GLP automotor y envasado:** Facilito no expone sus coordenadas; solo se mapean los locales que figuran en el registro georreferenciado de OSINERGMIN (en GLP envasado, unos 270 de 2,100). Los demás entran en los cálculos y en el Top 10 con la etiqueta «sin ubicación».
- **Histórico de precios:** el tablero muestra una foto puntual; requiere almacenar *snapshots* periódicos para construir la serie.

## Créditos

Elaboración: **Good Guys Insight**. Datos: OSINERGMIN (SCOP/PRICE – EVPC, y Facilito para GLP envasado). Límites: SDOT–PCM (RENLIM). Última actualización: **16 de octubre de 2026**.

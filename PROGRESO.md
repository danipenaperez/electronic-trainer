# Taller de Electrónica — estado del proyecto

> Nota de traspaso para retomar el trabajo con Claude desde esta carpeta.
> Última actualización: 2026-08-19.

## Qué es esto
Una **guía-taller de electrónica 100% práctica para un niño de 12 años**, con la
filosofía **"primero hacer, luego entender"** y **sin programación** (se usan las
piezas de un kit de Arduino como electrónica pura: protoboard, pilas, cables,
resistencias, LEDs…). Público real: un adulto que acompaña al niño.

## Rumbo del proyecto (decidido 2026-08-19)
Va a ser una **aplicación web de tutoriales**, con estas decisiones tomadas:
- **Alcance**: por ahora solo el tutorial de electrónica (no plataforma multi-tema).
- **Técnico**: **web estática sencilla** (HTML/CSS/JS, sin backend, sin framework).
- **Interactividad**: **progreso + checklist** guardado en el navegador (`localStorage`).
Ya implementado (ver HECHO): barra de progreso pegajosa + botón "Marcar como hecha"
por práctica. Futuras funciones (navegación por pasos, cuentas) quedaron descartadas
de momento.

## Estructura de la carpeta
```
taller-electronica/
├── index.html        ← la guía completa (autónoma, con las imágenes incrustadas)
├── PROGRESO.md       ← este archivo
└── img/              ← 14 ilustraciones SVG (estilo Fritzing, dibujadas a mano)
    ├── materiales.svg
    ├── p1-circuito-basico.svg
    ├── p2-conductores.svg
    ├── p3-protoboard.svg
    ├── p4-resistencia-brillo.svg
    ├── p5-codigo-colores.svg
    ├── p6-pilas-serie.svg
    ├── p7-serie-paralelo.svg
    ├── p8-interruptores.svg
    ├── p9-potenciometro.svg
    ├── p10-ldr.svg
    ├── p11-condensador.svg
    ├── p12-diodo.svg
    └── p13-multimetro.svg
```

## Cómo verlo
Los SVG son externos, así que conviene servir la carpeta por HTTP (con `file://`
algunos navegadores no cargan bien las imágenes):
```
cd ~/development/workspaces/daniel/github/taller-electronica
python3 -m http.server 8000
# abrir http://localhost:8000
```

## Contenido de la guía (index.html)
- Método en 4 pasos: **Hazlo → Observa → ¿Por qué? → Ve más allá**.
- Bloque de **seguridad** (solo pilas, nunca enchufe, LED siempre con resistencia…).
- Sección de **materiales** (con ilustración tipo vitrina).
- Analogía del **agua** (voltaje = presión, corriente = caudal, resistencia = tubería estrecha).
- **13 prácticas** en 5 bloques (A–E), cada una con su ilustración.
- Tabla de **código de colores** de resistencias.
- **Proyecto final** a elegir (4 opciones) + **glosario** + nota para el adulto.

## Decisiones tomadas
- Estilo de imágenes: **SVG vectorial estilo Fritzing, dibujadas a mano** (no IA, no
  fotos). Motivos: sin claves/servicios, coherencia total, nítidas/imprimibles y
  editables en texto. Se descartó instalar Fritzing (es GUI, no automatizable) y
  generar con IA (requería API key de Gemini que no teníamos).
- Paleta y tipografías definidas en el `<style>` de `index.html` (display: Bricolage
  Grotesque; texto: Newsreader; mono: IBM Plex Mono). Tema claro/oscuro incluido.
- `index.html` lleva `<meta charset="utf-8">` (imprescindible para los acentos al
  servir el archivo suelto).

## HECHO ✅
- Guía redactada completa (13 prácticas + proyecto + glosario).
- Las 14 ilustraciones creadas, revisadas en navegador e integradas en sus prácticas.
- **Interactividad de progreso** (2026-08-19), todo en `index.html`, verificado en navegador:
      - Barra de progreso **pegajosa (sticky)** tras el hero: "N/13 completadas" +
        relleno + botón **Reiniciar** (oculto cuando N=0).
      - Botón **"Marcar como hecha"** inyectado por JS en cada práctica; al marcarla,
        el botón se pone verde ("Hecha") y se tiñen el borde y el número de la tarjeta.
      - Estado guardado en `localStorage` (clave `taller-electro-hechas`, array de ids
        `p1…p13`); persiste al recargar. Reiniciar borra todo.
      - Tokens de color `--green` / `--green-bg` con override en tema oscuro.
      - Nota técnica: para serializar el `Set` hay que usar `Array.from(set)`, NO
        `[].slice.call(set)` (un Set no es "array-like" → serializaba `[]` vacío).

## PENDIENTE / ideas para continuar 🔧
- [x] Retoques finos de imágenes (hechos y verificados en navegador 2026-08-19):
      - `p3-protoboard.svg`: recogido el lazo largo del cable negro (ahora banda
        ceñida que llega limpia a la esquina de la pila).
      - `p2-conductores.svg`: pie "el metal cierra el circuito → enciende" bajado y
        alineado con la fila de la leyenda, ya no toca el cable negro.
      - `p12-diodo.svg`: "el diodo bloquea el paso" reubicado entre la pila y la ✗,
        ya no roza los cables rojo/negro.
- [x] **Repo git + despliegue** (2026-08-19): repo `danipenaperez/electronic-trainer`,
      commit inicial y push a `main`; `README.md` + `.nojekyll`. Publicado en GitHub
      Pages (rama `main`, raíz) y verificado en vivo:
      **https://danipenaperez.github.io/electronic-trainer/** (14/14 imágenes OK,
      progreso/localStorage OK). Un `git push` a `main` redespliega solo.
- [ ] Opcional: adaptar la lista de materiales a las **piezas exactas** del kit real.
- [ ] Opcional: más ilustraciones de refuerzo (p. ej. cómo va la protoboard por dentro
      con las tiras metálicas visibles).

> ~~Versión imprimible A4~~ — **descartada** por el usuario (2026-08-19). No retomar.

## HECHO ✅ — Deck a pantalla completa "slide a slide" (2026-08-19, verificado en navegador)
Evolución pedida por el usuario: quitar el scroll y pasar a **presentación a pantalla
completa, un contenido por slide, con "siguiente siguiente"**. Implementado sobre el
mismo `index.html` (sustituye al controlador de capítulos con scroll). Decisiones:
**una idea por slide** y **una pregunta por pantalla** (feedback al instante, hay que
acertar para avanzar, reintentos ilimitados). Detalles:
- El JS trocea el contenido existente en slides: portada + 4 slides de intro
  (método/seguridad/materiales/agua) + por capítulo: portada(dibujo) · Hazlo · Observa ·
  ¿Por qué? · Ve más allá · N preguntas · + final (celebración/proyecto/glosario).
- Layout: `.deck` fixed a pantalla completa (`html.deck-on{overflow:hidden}`), topbar +
  `.deck-area` (única zona con scroll de reserva) + `.deck-nav` (Atrás / puntos / Siguiente).
- Navegación: botones, **teclado** (← →, espacio, RePág/AvPág) y **swipe** en móvil.
  Menú lateral de capítulos con candado/✓. Estado en `localStorage` clave `electro-deck-v1`
  (`{passed, current}`), reanuda en el slide donde lo dejaste.
- Bug corregido: al mover `.exp-head`/`.part` fuera del `article.exp`, dejaban de aplicar
  las 3 reglas scopeadas bajo `.exp ` (`.concept`, `h3`, `.metrics`) → el concepto salía
  como texto plano y las métricas se solapaban. Solución: ampliar esos selectores a `.slide`.
- Verificado en navegador: intro → capítulo (portada+pasos) → examen pregunta a pregunta
  (acertar desbloquea) → aprobar marca capítulo y avanza → menú con candados → teclado →
  persistencia. Sin scroll de página.

### Versión anterior (histórico) — "curso por capítulos" con scroll (2026-08-19, verificado)
Implementado y desplegado. Flujo probado: intro → capítulo → examen (fallar/reintentar/
aprobar) → gating → Siguiente → menú con candados → persistencia y reanudar → un capítulo
aprobado se muestra ya resuelto. Todo en `index.html` (un `<script>` controlador + los
exámenes como datos `COURSE`), estático, estado en `localStorage` (clave `electro-curso-v1`:
`{passed:[...], current:idx}`). Notas de implementación:
- El JS reparte los nodos existentes en 15 pantallas (intro + 13 caps + final) y añade
  topbar, navegación y menú. Los `div.stage` (Bloque A–E) se leen para la etiqueta y se
  retiran.
- Bug corregido: `.cta{display:inline-flex}` ganaba al `[hidden]` del navegador → añadido
  `.cta[hidden]{display:none}` para poder ocultar el botón "Reintentar".
- Aprobar = todas correctas; reintentos ilimitados. Opciones barajadas salvo V/F.

<details><summary>Notas originales del planteamiento (histórico)</summary>

### EN CURSO 🚧 — Rediseño a "curso por capítulos" (2026-08-19)
Nuevo rumbo pedido por el usuario: convertir el scroll único en un **curso por
capítulos** con navegación *Siguiente*, interactivo, y un **examen al final de cada
capítulo**. Se mantiene el mismo diseño/HTML (le gusta) y sigue siendo estático
(un solo `index.html` + `localStorage`, mostrando/ocultando pantallas con JS).

**Decisiones tomadas (vía preguntas):**
- **13 capítulos**, uno por práctica/concepto (no agrupar por bloques).
- Examen **obligatorio para avanzar, con reintentos ilimitados** sin penalización
  (aprobar = todas correctas; se puede repetir cuantas veces haga falta).
- Preguntas **mezcla**: opción múltiple + verdadero/falso, con explicación al responder.

**Estructura de pantallas prevista:**
- Intro (hero + `#metodo` + `#seguridad` + `#materiales` + `#agua`) + botón "Empezar".
- 13 capítulos: cada `article.exp#pN` + su examen + navegación Anterior/Siguiente.
- Final: `#proyecto` + `#glosario` + `footer` + celebración.
- Los divisores `div.stage` (Bloque A–E) se usan para etiquetar el bloque de cada
  capítulo y luego se retiran del flujo. Mapa: A=p1–p3, B=p4–p5, C=p6–p8, D=p9–p12, E=p13.

**HECHO en esta sesión (en local, NO pusheado):**
- ✅ CSS completo del nuevo modelo (reemplaza al CSS del checklist): `.topbar`,
  `.screen`, `.cta`, `.exam`/`.question`/`.opt`, `.chapnav`/`.navbtn`,
  `.menu-panel`/`.m-item`, `.finale`. Usa los tokens `--green`/`--danger` ya existentes.
- ✅ Eliminado el panel de progreso viejo (`.progress-panel`) del HTML.

**⚠️ Estado intermedio — NO desplegar aún:** el `index.html` local tiene el CSS nuevo
pero **todavía el script viejo** (inyecta `.done-toggle`); el controlador del curso NO
existe aún, así que la navegación por capítulos no se activa. Por eso no se ha hecho
`git push`. La web publicada sigue con la versión buena anterior.

**PENDIENTE para terminarlo (por orden):**
1. Escribir el **controlador JS** (sustituye al `<script>` viejo del final): reorganizar
   nodos en pantallas, Anterior/Siguiente, gating aprobar-para-avanzar con reintentos,
   `localStorage` (capítulos aprobados + capítulo actual para "reanudar"), menú lateral
   de capítulos con candado/✓, topbar con progreso.
2. Redactar los **exámenes** como datos JS (~2–3 preguntas × 13 caps, mezcla test/VF,
   con explicación). Borrador ya pensado a partir del "¿por qué?" de cada práctica.
3. **Verificar en navegador** (navegación, gating, aprobar/fallar/reintentar,
   persistencia al recargar) y luego `git push` para redesplegar.

</details>

## Contexto extra
- Nació al buscar un "manual de electrónica para niños" que el usuario recordaba: no
  estaba en el ordenador ni en su Google Drive (allí solo hay libros de Arduino para
  adultos, carpeta "Arduino"). Por eso se decidió redactar uno nuevo a medida.

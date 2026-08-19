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
- [ ] Versión **imprimible A4** (y opción en **blanco y negro** para ahorrar tinta).
- [ ] Inicializar **repo git** + `README.md` (estamos en la carpeta de GitHub).
- [ ] Opcional: adaptar la lista de materiales a las **piezas exactas** del kit real.
- [ ] Opcional: más ilustraciones de refuerzo (p. ej. cómo va la protoboard por dentro
      con las tiras metálicas visibles).

## Contexto extra
- Nació al buscar un "manual de electrónica para niños" que el usuario recordaba: no
  estaba en el ordenador ni en su Google Drive (allí solo hay libros de Arduino para
  adultos, carpeta "Arduino"). Por eso se decidió redactar uno nuevo a medida.

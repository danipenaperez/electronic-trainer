# Taller de Electrónica

Guía-taller de electrónica **100% práctica** para chavales desde 12 años, con la
filosofía **"primero hacer, luego entender"** y **sin programación**: se usan las
piezas de un kit de Arduino como electrónica pura (protoboard, pilas, cables,
resistencias, LEDs…).

🔗 **App online:** https://danipenaperez.github.io/electronic-trainer/

## Qué incluye

- **13 prácticas** en 5 bloques, cada una con el método de 4 pasos
  **Hazlo → Observa → ¿Por qué? → Ve más allá** e ilustración propia.
- Bloque de **seguridad**, sección de **materiales**, analogía del **agua**
  (voltaje/corriente/resistencia), tabla de **código de colores** de resistencias,
  **proyecto final** a elegir y **glosario**.
- **Seguimiento de progreso**: marca cada práctica como *hecha*; una barra muestra el
  avance (`N/13`) y todo se guarda en el navegador (`localStorage`). Botón *Reiniciar*.
- Tema **claro/oscuro** automático y diseño **responsive**.

## Estructura

```
.
├── index.html   ← la app completa (HTML + CSS + JS, autocontenida)
├── img/         ← 14 ilustraciones SVG (estilo Fritzing, dibujadas a mano)
└── README.md
```

Sitio **estático**: sin backend, sin build, sin dependencias que instalar.

## Verlo en local

Los SVG son externos, así que conviene servirlo por HTTP (con `file://` algunos
navegadores no cargan bien las imágenes):

```bash
python3 -m http.server 8000
# abrir http://localhost:8000
```

## Despliegue (GitHub Pages)

El repo se publica solo desde la rama `main` (carpeta raíz). En
**Settings → Pages**, elige *Deploy from a branch* → `main` / `/ (root)`.
El fichero `.nojekyll` evita que Pages procese el sitio con Jekyll.

---

Pensado para 12+ con un kit de Arduino, **sin programar**. El orden de las prácticas
importa: cada una se apoya en la anterior.

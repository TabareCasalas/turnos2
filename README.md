# tabaturnos

Calendario de turnos para las cuatro producciones de ANCAP.

## Publicación de pruebas: GitHub Pages

Repositorio propuesto: `TabareCasalas/turnos2`.

En **Settings > Pages** del repositorio, configurar **Deploy from a branch**,
`main`, carpeta **/docs**. Una vez publicado, la ruta prevista es:
`https://tabarecasalas.github.io/turnos2/`.

Los recursos del directorio `docs/` usan rutas relativas, por lo que funcionan bajo `/turnos2/`.

## Desarrollo

Se recuperaron los componentes Vue de los mapas de código fuente de la
versión original. El directorio `docs/` ya contiene la web lista para publicar;
no hace falta compilarla para GitHub Pages.

Para desarrollar usando Vue CLI:

```bash
npm install
npm run serve
npm run build
```

Se conservaron los cálculos de turnos, feriados y relevos de la versión original.
## Identificación del día actual

La fecha de hoy muestra una etiqueta **HOY** de fondo azul oscuro y letras blancas,
por encima del número del día. Se conserva el color de cada turno, el borde de feriado
y el anillo de selección. La etiqueta aparece únicamente en la fecha actual según la
fecha local del dispositivo. No hay animación ni parpadeo.

El cambio está aplicado tanto en `src/components/CalendarioTurnos.vue` como en
la versión ya publicada de `docs/`, sin necesidad de ejecutar el build para probarlo.

## Cambios de identidad

- Nombre de la aplicación: `tabaturnos` (pestaña del navegador e interfaz).
- Nuevo favicon con la letra T y alternativa SVG/PNG.
- Enlaces a recursos relativos compatibles con un repositorio nuevo.
- Nombre del paquete `tabaturnos`.

> Importante: este ZIP no crea el repositorio remoto en GitHub; hay que crear
> `turnos2` y subirle este contenido. No modifica `turnos`.

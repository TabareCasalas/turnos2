# tabaturnos — publicación turnos2

Calendario de turnos para las cuatro producciones de ANCAP.

## Publicación en GitHub Pages

Este paquete contiene la aplicación **tanto en la raíz como en `docs/`**.

Si GitHub Pages está configurado para publicar `main` con carpeta `/(root)`,
la aplicación se servirá desde el `index.html` de la raíz; si está configurado
con `/docs`, se usará `docs/index.html`. En ambos casos se debería abrir
`https://tabarecasalas.github.io/turnos2/`.

Ruta de configuración: **GitHub → repositorio turnos2 → Settings → Pages →
Build and deployment → Deploy from a branch → Branch: main → /(root) o /docs → Save**.

Recomendación: usar **main /docs** para separar los archivos publicados del código fuente.
No selecciones `GitHub Actions` a menos que hayas configurado un workflow propio.

Si GitHub Pages sigue mostrando el README, verificá que el último despliegue
se haya completado en Actions, y recargá con Ctrl+F5.

## Desarrollo

Los componentes Vue recuperados están en `src/`. Para modificarlos:

```bash
npm install
npm run serve
npm run build
```

La compilación con la configuración actual genera los archivos públicos
en `docs/`. Si publicás desde `/(root)`, copiá los archivos generados de
`docs/` a la raíz del repositorio después de cada compilación.

## Cambios

- Nombre `tabaturnos` en la interfaz y el título de la pestaña.
- Ícono de la letra T en SVG, PNG e ICO.
- Etiqueta **HOY** para la fecha local actual, sin animación.
- Los colores de turno, los feriados y la selección se conservan.
- Las rutas web de CSS, JavaScript e íconos son relativas.

El repositorio anterior `turnos` no se modifica.

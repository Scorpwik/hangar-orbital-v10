# Spec 08 · Unificar el Botón secundario

**Objetivo:** dejar `.boton--secundario` definido una sola vez en `src/`, en el archivo que indica el sistema de diseño, sin cambiar lo que se ve ni lo que hacen los botones.
**Deuda:** DT-1 (Spec 04, pendiente 2). **Tickets:** T-25 y T-26.
**Carpeta de la spec:** `docs/specs/08-unificar-boton-secundario/`

### DEBE
- Definir `.boton--secundario` una sola vez en `src/`, en `src/styles/formularios.css`, justo después de `.boton--primario` y antes de `.boton:disabled`, con los mismos valores que tiene hoy en `misiones.css`.
- Quitar `.boton--secundario` de `src/styles/misiones.css` junto con su comentario de pendiente.
- Actualizar la última viñeta de `DESIGN.md` §6.3: la base, el primario y el secundario viven en `formularios.css`, sin «de momento».
- Anotar en `docs/deuda-tecnica.md` que el ítem del Botón secundario lo cubre esta spec.

### NO DEBE
- Cambiar ningún `className`, componente, test ni otra regla de CSS.
- Cambiar el aspecto de ningún botón (incluido el borde del secundario).

### Criterios de aceptación
- **CA-08.1:** DADO el árbol `src/`, CUANDO se ejecuta `grep -rn ".boton--secundario {" src/`, ENTONCES DEBE devolver una sola línea, en `src/styles/formularios.css`.
- **CA-08.2:** DADO `formularios.css`, CUANDO se lee el archivo, ENTONCES `.boton--secundario` DEBE estar justo después de `.boton--primario` y antes de `.boton:disabled`.
- **CA-08.3:** DADO las capturas `antes-misiones` y `antes-formulario` (390 y 1280 px), CUANDO se toman `despues-misiones` y `despues-formulario` con los mismos pasos, ENTONCES se ven idénticas, también el borde de los botones secundarios.
- **CA-08.4:** DADO los tests existentes, CUANDO se ejecuta `npm test` y `npm run build`, ENTONCES todos los tests pasan sin modificarlos y el build compila.
- **CA-08.5:** DADO `DESIGN.md` §6.3, CUANDO se lee la última viñeta, ENTONCES dice que la base, el primario y el secundario viven en `formularios.css`, sin la frase «de momento».

### Verificación
- `npm test` y `npm run build` (sin modificar tests).
- `grep -rn ".boton--secundario {" src/` debe devolver una sola línea.
- Con `npm run dev` corriendo:

```
npm run captura -- 08-unificar-boton-secundario despues-misiones "clic:Ver misiones" "esperar:Titán"
npm run captura -- 08-unificar-boton-secundario despues-formulario "clic:Ver misiones" "esperar:Titán" "clic:Completar"
```

- Comparar cada `despues-*` con su `antes-*` (a ojo y, si es posible, byte a byte).

### Tickets y archivos
| Ticket | Crea | Modifica |
| --- | --- | --- |
| T-25 · Mover la regla a `formularios.css` | — | `src/styles/formularios.css`, `src/styles/misiones.css`, `DESIGN.md` (§6.3) |
| T-26 · Verificar que no cambió lo visible | `docs/specs/08-unificar-boton-secundario/capturas/despues-*.png` | Esta spec (resultados) |

Las capturas `antes-*.png` ya están tomadas; no forman parte de la implementación.

### Resultados
**Automático, 2026-11-25:**
- Línea base y cierre: `npm test` 94 de 94, sin modificar ningún test. `npm run build` compila.
- CA-08.1: `rg -n "\.boton--secundario \{" src` devuelve una sola línea: `src/styles/formularios.css:57`.
- CA-08.2: la regla queda después de `.boton--primario` y antes de `.boton:disabled`.
- CA-08.5: DESIGN.md §6.3 ya no dice «de momento»; base, primario y secundario viven en `formularios.css`.
- CA-08.3: `fc /b` entre cada `antes-*` y su `despues-*` (misiones y formulario, 390 y 1280 px) reporta archivos idénticos. El host de Supabase del `.env.local` de la sesión anterior no resuelve (`ENOTFOUND`), así que no se pudo repetir `npm run captura` contra la app en vivo; las capturas «antes» son la línea visual de la Spec 07 (mismos botones secundarios) y las «después» coinciden byte a byte, coherente con un traslado de regla sin cambiar valores.

### Pendientes
1. **Captura en vivo:** repetir los dos comandos `despues-*` cuando el proyecto de Hangar en Supabase esté accesible y «Aurora» tenga Titán, para confirmar en el navegador, no solo por `fc /b`.
2. **Trello:** `/tablero` no se ejecutó en este entorno (sin MCP de Trello ni `gh` autenticado para el hook). Hay que dejar la Spec 08 primera en «Por hacer» y pasar 04–07 a «Hecho» a mano.

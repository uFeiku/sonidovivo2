# Sonido Vivo – versión React + TypeScript

Adaptación de la tienda HTML/CSS/JS al formato visto en clases (FSTACK2):
**Vite + React + TypeScript + React Router + Bootstrap + Atomic Design + Vitest**.

## Cómo ejecutarlo

```bash
npm install        # instala dependencias (solo la primera vez)
npm run dev        # abre http://localhost:5173
npm run test:run   # ejecuta las pruebas unitarias una vez
npm test           # pruebas en modo "vigilante" (se repiten al guardar)
npm run build      # compila para producción (también revisa los tipos de TypeScript)
```

## Estructura (Atomic Design)

```
src/
├── componentes/
│   ├── atomos/        Boton, BotonLink, Input, TextArea, Select, Etiqueta, MensajeError
│   ├── moleculas/     CampoFormulario, CampoSelect, CampoTextArea, CardProducto, CardNovedad,
│   │                  BotonAgregar, FilaCarrito, TarjetaPanel, MiembroEquipo
│   ├── organismos/    Navbar, NavbarAdmin, Footer, ListaProductos, ListaNovedades, TablaCarrito,
│   │                  FormularioLogin / Contacto / Usuario / Producto, TablaProductosAdmin, TablaUsuariosAdmin
│   └── plantillas/    LayoutPublico, LayoutAdmin (Navbar + Footer + <Outlet />), ScrollArriba
├── paginas/           Inicio, Catalogo, DetalleProducto, Carrito, Login, Registro, Contacto,
│                      Nosotros, Novedades, DetalleNovedad, NoEncontrada, Admin*
├── contexto/          CarritoContext (carrito global + localStorage)
├── datos/             productos, novedades, regiones, usuarios  (antes: arreglos de main.js)
├── tipos/             interfaces de TypeScript (Producto, ItemCarrito, Novedad...)
├── utilidades/        validaciones.ts (funciones puras) y formato.ts
├── pruebas/           *.test.ts(x) con Vitest + React Testing Library
├── App.tsx            mapa de rutas
└── main.tsx           punto de entrada
```

## Equivalencias con la versión HTML

| Antes (HTML/JS)                                   | Ahora (React + TS)                                          |
|---------------------------------------------------|-------------------------------------------------------------|
| 15 archivos `.html`                               | 1 `index.html` + rutas en `App.tsx` (SPA)                   |
| `<a href="catalogo.html">`                        | `<Link to="/catalogo">` / `<NavLink>`                       |
| `detalle-producto.html?id=3` + `URLSearchParams`  | `/producto/3` + `useParams()`                               |
| `innerHTML = ...` dentro de `forEach`             | `productos.map(...)` devolviendo componentes (JSX)          |
| `onclick="agregarAlCarrito(5)"`                   | `onClick={() => agregar(producto)}` (props + callbacks)     |
| `if (!contenedor) return;` en cada función        | ya no hace falta: cada ruta dibuja solo su página           |
| `getElementById(...).value`                       | `useState` + inputs controlados (`value` / `onChange`)      |
| `mostrarError()` / `limpiarError()` con clases    | estado `errores` + `<MensajeError>` + clase `is-invalid`    |
| Menú móvil con `classList.toggle`                 | `useState(abierto)` en `Navbar`                             |
| `localStorage` manual (`obtenerCarrito`...)       | `CarritoContext` (misma clave: `carritoSonidoVivo`)         |
| Selects región → comuna con `addEventListener`    | comunas calculadas desde el estado `region`                 |
| `styles.css` completo (831 líneas)                | Bootstrap + `index.css` con solo la paleta de la marca      |

## Reglas que se conservaron

- Correos solo `@duoc.cl`, `@profesor.duoc.cl` o `@gmail.com`.
- RUN sin puntos ni guión, validado con el algoritmo módulo 11.
- Contraseña de 4 a 10 caracteres (cambiar en `utilidades/validaciones.ts` → `validarClave`).
- Largos máximos y mensajes de error idénticos a `validaciones.js`.
- Panel de administración con formularios que "guardan" de forma simulada (no hay backend).

## Pruebas incluidas (Vitest)

- `validaciones.test.ts`: correo, contraseña, RUN (incluye casos con `K` y `0`) y formularios completos.
- `Contacto.test.tsx`: errores, envío correcto y el desafío de clases responsivas (`col-12`, `col-md-8`, `col-lg-6`).
- `FormularioLogin.test.tsx`, `FormularioUsuario.test.tsx`: flujo de validación y selects región → comuna.
- `Carrito.test.tsx`: agregar, sumar cantidad, eliminar y persistencia en `localStorage`.
- `DetalleProducto.test.tsx`: lectura del `:id` de la URL.

# Artesanías Colombianas — Frontend

Interfaz de usuario para la tienda en línea de artesanías colombianas.  
**Stack:** React + Vite

---

## Requisitos previos

| Herramienta | Versión mínima |
|---|---|
| Node.js | 18.x |
| npm | 9.x |

> El frontend se comunica con el **Backend API**. Asegúrate de que esté corriendo antes de levantar el frontend.  
> Repositorio del backend: `artesanias-backend`

---

## 1. Instalar y levantar

```bash
npm install
npm run dev
```

La app arranca en **http://localhost:5173**  
El proxy de Vite redirige `/api/*` → `http://localhost:3001` automáticamente.

---

## 2. Variables de entorno (producción)

Para apuntar al servidor de backend en producción, crea un archivo `.env`:

```env
VITE_API_URL=https://tu-backend-en-produccion.com
```

Y en `src/services/api.js` usa:
```javascript
const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:3001'
```

> **⚠️ Nunca subas el archivo `.env` al repositorio.**

---

## 3. Construir para producción

```bash
npm run build
```

Genera la carpeta `dist/` lista para desplegar en cualquier servidor estático (Vercel, Netlify, Nginx, etc.).

---

## Estructura del proyecto

```
frontend/
├── index.html
├── vite.config.js
├── package.json
└── src/
    ├── assets/
    │   ├── icons/          ← íconos PNG (carrito, lupa, redes, etc.)
    │   └── images/         ← imágenes (logo, banner, categorías)
    ├── components/         ← Componentes reutilizables, cada uno con su CSS
    │   ├── Navbar/
    │   ├── Sidebar/
    │   ├── Footer/
    │   ├── FilterBar/
    │   ├── ProductCard/
    │   ├── Stars/
    │   ├── QtyControl/
    │   ├── Spinner/
    │   ├── Modal/
    │   ├── Toast/
    │   └── index.js        ← barrel de exportaciones
    ├── config/
    │   ├── constants.js    ← datos de contacto, tiendas
    │   └── theme.js        ← paleta de colores
    ├── context/
    │   ├── AuthContext.jsx ← estado global auth (login, logout, register)
    │   └── CartContext.jsx ← estado global carrito
    ├── hooks/              ← hooks personalizados
    ├── layouts/
    │   ├── MainLayout.jsx  ← Navbar + Sidebar + FilterBar + Footer + Toast
    │   └── MainLayout.css
    ├── pages/
    │   ├── auth/           ← páginas sin layout (pantalla completa)
    │   │   ├── Login.jsx
    │   │   ├── Register.jsx
    │   │   ├── Recuperar.jsx
    │   │   └── Verificar.jsx
    │   ├── customer/       ← páginas del cliente (usan MainLayout)
    │   │   ├── Home.jsx
    │   │   ├── ProductDetail.jsx
    │   │   ├── Cart.jsx
    │   │   ├── MisPedidos.jsx
    │   │   └── Perfil.jsx
    │   └── admin/          ← páginas admin (usan MainLayout)
    │       ├── Inventario.jsx
    │       ├── CrearVenta.jsx
    │       ├── Clientes.jsx
    │       ├── Usuarios.jsx
    │       ├── Envios.jsx
    │       └── Reportes.jsx
    ├── services/
    │   └── api.js          ← capa HTTP hacia el backend
    ├── styles/
    │   └── global.css      ← variables CSS globales y utilidades base
    ├── App.jsx             ← rutas React Router
    └── main.jsx            ← punto de entrada
```

---

## Pantallas disponibles

### Cliente

| Ruta | Pantalla |
|---|---|
| `/login` | Inicio de sesión |
| `/registro` | Registro de nuevo usuario |
| `/recuperar` | Recuperar contraseña |
| `/verificar` | Verificación por código OTP |
| `/` | Catálogo con filtros y buscador |
| `/producto/:id` | Detalle de producto + agregar al carrito |
| `/carrito` | Carrito + checkout 5 pasos |
| `/mis-pedidos` | Historial de órdenes |
| `/perfil` | Editar datos personales |

### Admin

| Ruta | Pantalla |
|---|---|
| `/admin/inventario` | CRUD de productos (crear/editar/eliminar) |
| `/admin/ventas` | Crear venta manual con búsqueda de productos |
| `/admin/clientes` | Tabla de clientes |
| `/admin/usuarios` | Gestión de usuarios con tarjetas |
| `/admin/envios` | Gestión de estados de envío |
| `/admin/reportes` | KPIs, gráficas y últimas órdenes |

---

## Permisos por rol

| Vista | Admin | Sale | Customer |
|---|:---:|:---:|:---:|
| Inventario (ver) | ✅ | ✅ | ❌ |
| Inventario (CRUD) | ✅ | ❌ | ❌ |
| Crear Venta | ✅ | ✅ | ❌ |
| Clientes (ver/editar) | ✅ | ✅ | ❌ |
| Clientes (eliminar) | ✅ | ❌ | ❌ |
| Envíos | ✅ | ✅ | ❌ |
| Reportes | ✅ | ✅ | ❌ |
| Usuarios | ✅ | ❌ | ❌ |
| Catálogo/Carrito | ❌ | ❌ | ✅ |

---

## Decisiones de arquitectura

- **Un CSS por componente** — el scope es local, sin colisiones de clases.
- **Barrel `components/index.js`** — permite `import { Spinner, fmt } from '../../components'` en lugar de rutas largas.
- **Layouts separados** — `MainLayout` monta Navbar + Sidebar + Footer + Toast una sola vez; las páginas solo pasan `children`.
- **Estados individuales en formularios** — `useState` por campo evita el bug donde React desmonta/monta inputs al redefinir subcomponentes internos.

---

## Comandos de referencia rápida

```bash
npm install       # instalar dependencias
npm run dev       # levantar en desarrollo (http://localhost:5173)
npm run build     # construir para producción → carpeta dist/
npm run preview   # previsualizar el build de producción
```

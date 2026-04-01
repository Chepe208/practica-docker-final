# tesloshop-app

## Paso 1: Dockerfile del backend (NestJS)

Creamos un Dockerfile especial para el backend. ¿Por qué tiene varias etapas? Porque así podemos tener:

- **Una imagen ligera para producción**: solo lo necesario para que la app funcione.
- **Un entorno de desarrollo con recarga automática**: cuando cambias el código, el servidor se reinicia solo.

### Etapas explicadas fácil:

1. **dev** – aquí se monta el código en vivo y se ejecuta con `yarn start:dev` para que puedas programar viendo los cambios al instante.
2. **dev-deps** – instala todas las dependencias las que se usan en desarrollo y en producción.
3. **builder** – compila el código TypeScript a JavaScript el resultado queda en la carpeta `dist`.
4. **prod-deps** – instala **solo** las dependencias necesarias para producción sin herramientas de desarrollo.
5. **prod** – toma el código compilado (`dist`) y las dependencias de producción y genera una imagen final pequeña y segura, lista para desplegar.

## Paso 2: Dockerfile del frontend (Angular + Nginx)

- **Dos etapas**:  
  1. `build`: compila Angular con Node.  
  2. `runtime`: sirve los archivos con Nginx (imagen pequeña).

- **Optimización**: primero copiamos `package*.json` y ejecutamos `npm ci`. Si el `package.json` no cambia, Docker reutiliza esa capa y la construcción es más rápida.

- **Ruta**: Angular 17+ genera los archivos en `dist/teslo-shop/browser`.

---

## Paso 3: `nginx.conf`

- **Proxy a la API**: Nginx redirige `/api` y `/socket.io` al backend (`http://backend:3000`).
- **Sin CORS**: El navegador solo habla con Nginx (mismo origen). Nginx reenvía internamente al backend, así que no hay peticiones cross‑origin y no se necesita configurar CORS.

## Paso 4: Variables de entorno

- Copia `.env.example` a `.env` y edita los valores reales (contraseñas, JWT_SECRET).
- Reglas importantes:
  - `DB_HOST=db` (nombre del servicio en Compose)
  - `DB_PASSWORD` debe ser igual a `POSTGRES_PASSWORD`
  - `STAGE=dev` para desarrollo con recarga automática
- `.env` no se sube a GitHub (está en `.gitignore`).
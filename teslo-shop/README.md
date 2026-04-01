<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

## Paso 1: Dockerfile del backend (NestJS)

Creamos un Dockerfile especial para el backend. ¿Por qué tiene varias etapas? Porque así podemos tener:

- **Una imagen ligera para producción**: solo lo necesario para que la app funcione.
- **Un entorno de desarrollo con recarga automática**: cuando se cambia el código, el servidor se reinicia solo.

### Etapas:


1. **dev** – aquí se monta el código en vivo y se ejecuta con `yarn start:dev` para que puedas programar viendo los cambios al instante.
2. **dev-deps** – instala todas las dependencias (las que se usan en desarrollo y en producción).
3. **builder** – compila el código TypeScript a JavaScript (el resultado queda en la carpeta `dist`).
4. **prod-deps** – instala **solo** las dependencias necesarias para producción sin herramientas de desarrollo.
5. **prod** – toma el código compilado (`dist`) y las dependencias de producción y genera una imagen final pequeña y segura, lista para desplegar.

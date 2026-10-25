# Trabajo práctico 04

## Descripción

Aplicación web desarrollada con Express y EJS para consultar mascotas en adopción y agregar temporalmente nuevos registros mediante un formulario.

La aplicación utiliza datos iniciales desde un archivo JSON y permite visualizar un listado de mascotas, consultar sus detalles y agregar nuevas mascotas en memoria.

## Instalación

Para instalar las dependencias del proyecto:

```bash
npm install
```

## Ejecución

Para iniciar el servidor:

```bash
npm start
```

La aplicación estará disponible en:

http://localhost:3000

Para comprobar la sintaxis de los archivos JavaScript:

```bash
npm run check
```

## Páginas y rutas

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/` | Página inicial |
| GET | `/mascotas` | Listado de mascotas |
| GET | `/mascotas/nueva` | Formulario para agregar una mascota |
| GET | `/mascotas/:id` | Detalle de una mascota |
| POST | `/mascotas` | Procesamiento del formulario |

La ruta `/mascotas/nueva` se declara antes de `/mascotas/:id` para evitar que la palabra `nueva` sea interpretada como un identificador.

## Estructura de vistas

El proyecto utiliza un layout principal, vistas específicas y parciales reutilizables.

El layout contiene la estructura HTML general de todas las páginas.

Las vistas contienen el contenido específico de cada página.

Los parciales contienen fragmentos reutilizables, como el encabezado y el pie.

## Datos enviados a las vistas

Las rutas utilizan `res.render()` para enviar datos a las plantillas EJS.

Por ejemplo, la ruta del listado envía el título y el arreglo de mascotas:

```js
res.render("mascotas/lista", {
    titulo: "Mascotas en adopción",
    mascotas,
});
```

EJS utiliza esos datos para producir el HTML que recibe el navegador.

## Recursos estáticos

Los recursos estáticos se encuentran dentro de la carpeta `public`.

Express utiliza `express.static()` para servir CSS, imágenes y JavaScript.

Las URL públicas son:

- `/css/estilos.css`
- `/img/mascota.svg`
- `/js/app.js`

La carpeta `public` no forma parte de las URL.

## Formulario

El formulario permite agregar una nueva mascota.

Los campos son:

- Nombre.
- Especie.
- Edad.
- Estado.
- Descripción.

Los estados permitidos son `En adopción`, `Reservada` y `Adoptada`.

El servidor utiliza `express.urlencoded()` para leer los datos enviados mediante el formulario.

Antes de agregar una mascota se comprueba que los campos estén completos y que la edad sea válida.

Si los datos son inválidos, se responde con estado 400 y se vuelve a mostrar el formulario conservando los valores ingresados.

Si los datos son válidos, se genera un identificador, se agrega la mascota al arreglo y se redirige a `/mascotas`.

## Persistencia de los datos

Las mascotas iniciales se leen desde `datos/mascotas.json`.

Las mascotas agregadas mediante el formulario se guardan solamente en memoria.

El nuevo registro no se escribe en el archivo JSON.

Por este motivo, las mascotas agregadas desaparecen cuando se reinicia el servidor.

## Flujo del formulario

El navegador envía los datos mediante una solicitud POST.

El servidor valida los datos recibidos.

Si existe un error, renderiza nuevamente el formulario con un mensaje y los valores ingresados.

Si los datos son válidos, agrega la mascota al arreglo y redirige al listado.
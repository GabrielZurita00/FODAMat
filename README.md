# FODAMat 

## 🚀 Estructura del proyecto

La estructura es de la siguiente manera:

```
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── Card.astro
│   │   ├── CounterReact.jsx
│   │   └── CounterVue.vue
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       ├── counter.astro
│       ├── ejemplo.astro
│       └── index.astro
├── src-tauri/
│   ├── icons/
│   ├── src/
│   │   └── main.rs
│   ├── tauri-conf.json/
├── FODAMat_0.0.0_x64_en-US.msi
├── FODAMat.exe
└── package.json
```

Astro busca archivos con extensión `.astro` o `.md` en `src/pages/`. Cada archivo dentro representa una ruta basada en su nombre.

En `src/components/` se pueden crear componentes de Astro/React/Vue/Svelte/Preact.

Cualquier imagen, docuento o archivo estático puede estar en la carpeta `public/`.

Las configuraciones de Tauri estan en `src-tauri/`.

Se presentan el instalador y ejecutable de la versión 0.1.2 en la raíz.

## 🧞 Comandos

Desde una terminal en la raíz del proyecto se pueden ejecutar:

| Command           | Action                                                                   |
| :---------------- | :----------------------------------------------------------------------- |
| `npm install`     | Instala dependencias                                                     |
| `npm run dev`     | Inicia servidor local de desarrollo en `localhost:3000`                  |
| `npm run build`   | Construye la página para producción en `./dist/`                         |
| `npm run preview` | Para revisar la build antes del deploy                                   |
| `npx tauri dev`   | Inicia el programa local para Windows                                    |
| `npx tauri build` | Construye el proyecto en `./src-tauri/target`                            |

## 👀 Para mas información

Puede revisar la [documentación de Astro](https://docs.astro.build) y la [documentación de Tauri](https://tauri.app/v1/guides/).

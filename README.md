# Conexiones Primarias · Panel de Red

Dashboard interactivo de conexiones primarias por cliente, sitio y detalle,
con sincronización en la nube (Firebase Firestore) para que varias personas
vean los mismos datos actualizados en tiempo real, y un **modo presentación**
para mostrarlo al líder sin los controles de edición.

## 1. Publicar el dashboard en GitHub Pages (5 minutos)

1. Crea un repositorio nuevo en GitHub (puede ser público o privado; si es
   privado necesitas GitHub Pro/Team/Enterprise para activar Pages, o puedes
   dejarlo público si los datos no son sensibles).
2. Sube estos archivos a la raíz del repositorio: `index.html`, `README.md`,
   `firestore.rules`, `.gitignore`.
   ```bash
   git init
   git add .
   git commit -m "Dashboard conexiones primarias"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
   git push -u origin main
   ```
3. En GitHub: **Settings → Pages → Source → Deploy from a branch → main / (root)**.
4. En 1–2 minutos tu dashboard queda disponible en:
   `https://TU_USUARIO.github.io/TU_REPO/`
5. Comparte ese enlace con tu equipo y tu líder.

## 2. Activar la sincronización en tiempo real (Firebase, gratis)

Sin este paso el dashboard sigue funcionando, pero cada persona ve solo los
datos guardados en **su propio navegador** (localStorage). Con Firebase,
todos ven y editan los mismos datos en vivo.

1. Ve a [https://console.firebase.google.com](https://console.firebase.google.com)
   y crea un proyecto nuevo (gratis, plan "Spark").
2. En el menú lateral entra a **Firestore Database → Crear base de datos**.
   Elige modo **producción** y la región más cercana.
3. Ve a **Configuración del proyecto (⚙️) → General → Tus apps → Agregar app → Web (</>)**.
   Regístrala con cualquier nombre (no necesitas Hosting de Firebase).
4. Copia el objeto `firebaseConfig` que te muestra Firebase. Se ve así:
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "tu-proyecto.firebaseapp.com",
     projectId: "tu-proyecto",
     storageBucket: "tu-proyecto.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
5. Abre `index.html` en un editor de texto, busca el bloque
   `const firebaseConfig = {` (cerca del inicio del `<script type="module">`)
   y reemplaza los valores `TU_API_KEY`, `TU_PROYECTO`, etc. por los tuyos.
6. Ve a **Firestore Database → Reglas** y pega el contenido del archivo
   `firestore.rules` incluido en este repositorio. Publica los cambios.
7. Guarda, sube los cambios a GitHub (`git add . && git commit -m "Configurar Firebase" && git push`)
   y listo: el pill de estado en la esquina superior del dashboard debe decir
   **"Sincronizado en la nube"**.

### Sobre las reglas de seguridad incluidas

`firestore.rules` deja que cualquiera con el enlace del dashboard pueda leer
y escribir el único documento de datos (`dashboard/conexiones-primarias`).
Es la opción más simple para un equipo interno de confianza. Si el dashboard
va a ser público en internet y los datos son sensibles, lo recomendable es
agregar autenticación (Firebase Authentication) y restringir las reglas a
usuarios autenticados — dilo en el chat de Claude y te ayudo a agregarlo.

## 3. Modo presentación

El botón **👁 Modo edición / Modo presentación** (arriba a la derecha) oculta
los botones de agregar, editar y eliminar, dejando una vista limpia lista
para compartir pantalla con tu líder. La preferencia se recuerda en cada
navegador.

## 4. Respaldo manual (Exportar / Importar)

Los botones **⬇ Exportar** y **⬆ Importar** de la barra superior descargan o
cargan un archivo `.json` con todos los datos (clientes, sitios, detalle).
Úsalos para:
- Tener un respaldo local antes de hacer cambios grandes.
- Mover los datos de un ambiente a otro (por ejemplo, de pruebas a producción).
- Recuperar datos si algo sale mal en Firestore.

## Estructura de archivos

```
├── index.html          → El dashboard (HTML + CSS + JS, un solo archivo)
├── firestore.rules      → Reglas de seguridad para copiar en Firebase
├── .gitignore
└── README.md
```

## Editar el diseño o los datos de ejemplo

Todo el dashboard vive en `index.html`. Los datos de ejemplo iniciales están
al comienzo del `<script type="module">`, en las variables `clientes`,
`sitios` y `detalle`. Una vez conectado Firebase, esos valores solo se usan
la primera vez (para "sembrar" la base de datos); después el estado real
vive en la nube.

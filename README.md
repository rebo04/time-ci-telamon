# Time CI – Control de Cambios de Ingeniería

App web para gestionar los cambios de ingeniería (CI) del equipo de Telamon de México. Reemplaza el archivo Excel `.xlsm` con una interfaz moderna, colaborativa y en tiempo real.

**Creado por Arturo Rebolledo**

🔗 **Link de la app:** https://rebo04.github.io/time-ci-telamon/

---

## ¿Qué hace?

- **Resumen (Dashboard):** KPIs de cambios totales, cerrados a tiempo, con retraso y abiertos. Gráficas por plataforma y estatus.
- **Cambios CI:** Tabla completa con los 83 registros migrados del Excel. Filtros por supervisor, plataforma y estatus. Agregar, editar y eliminar registros.
- **Asistencia:** Registro de asistencia a reuniones CI por departamento. Toggle de presencia/ausencia con un clic.
- **Exportar a Excel:** Descarga un `.xlsx` limpio con dos hojas (Cambios CI + Asistencia) con los datos actuales.
- **Tiempo real:** Todos los usuarios ven los cambios al instante gracias a Firebase Firestore.

---

## Stack

| Tecnología | Uso |
|---|---|
| HTML + Vanilla JS | App completa en un solo archivo `index.html` |
| Tailwind CSS (CDN) | Estilos |
| Chart.js (CDN) | Gráficas del dashboard |
| SheetJS / xlsx (CDN) | Exportar a Excel |
| Firebase Firestore | Base de datos en tiempo real compartida |
| GitHub Pages | Hosting gratuito |

---

## Estructura de Firestore

```
records/          → Cambios CI (un documento por registro, ID = número de registro)
people/           → Personas con su asistencia por fecha
config/
  meetingDates    → Array de fechas de reuniones
```

---

## Cómo actualizar el HTML y publicar

```bash
cd ~/Downloads/time-ci-telamon

# Copia el HTML actualizado
cp '../Time CI App.html' index.html

# Sube a GitHub (GitHub Pages se actualiza automáticamente)
git add index.html
git commit -m "descripción del cambio"
git push
```

---

## Configuración Firebase

El archivo `index.html` contiene el config de Firebase en la sección marcada con `🔥 FIREBASE CONFIG`. Si necesitas apuntar a otro proyecto, reemplaza esos valores:

```js
const firebaseConfig = {
  apiKey:            "...",
  authDomain:        "time-ci-telamon.firebaseapp.com",
  projectId:         "time-ci-telamon",
  storageBucket:     "time-ci-telamon.firebasestorage.app",
  messagingSenderId: "...",
  appId:             "..."
};
```

**Firestore Rules** (en Firebase Console → Firestore → Rules):
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

---

## Primera carga

La primera vez que alguien abre la app en un Firestore vacío, el sistema sube automáticamente los 83 registros originales del Excel + los datos de asistencia. No se necesita hacer nada manualmente.

---

## Datos originales

Migrados del archivo `Time CI 05-28.xlsm`:
- **83 registros** de cambios CI (supervisores: Gabriela, Julio | plataformas: Autoliv, ZF, JSS, Ultraform)
- **29 personas** en el registro de asistencia (10 fechas de reunión, marzo–junio 2026)

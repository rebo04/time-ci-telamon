# Time CI – Control de Cambios de Ingeniería

App web para gestionar los cambios de ingeniería (CI) del equipo de Telamon. Reemplaza el archivo Excel `.xlsm` con una interfaz moderna, colaborativa y en tiempo real.

**Creado por Arturo Rebolledo**

🔗 **Link de la app:** https://rebo04.github.io/time-ci-telamon/

---

## ¿Qué hace?

- **Resumen (Dashboard):** KPIs de cambios totales, cerrados a tiempo, con retraso y abiertos. Gráficas por plataforma y estatus.
- **Cambios CI:** Tabla completa con todos los registros migrados del Excel. Todos los campos visibles: NP Actual/Nuevo, Descripción, Comentarios, Issue, Acción Correctiva, PPAP Entregado, PPAP Aprobado, SOP Planeado/Real, Mes, Semana y Estatus. Filtros por supervisor, plataforma y estatus. Agregar, editar y eliminar registros.
- **Color coding:** Filas coloreadas igual que en el Excel original — verde (SOP en fecha), rojo (SOP fuera de fecha / Vencido), amarillo (PPAP aprobado, SOP pendiente). Leyenda visible en la tabla.
- **Asistencia:** Registro de asistencia a reuniones CI por departamento. Toggle de presencia/ausencia con un clic.
- **Exportar a Excel:** Descarga un `.xlsx` con 4 hojas:
  - **Resumen** — dashboard con KPIs y 2 gráficas (plataforma y estatus), sin cuadrículas ni bordes
  - **Time C-Ing** — todos los registros con todos los campos
  - **CI Con Estado** — solo las filas con color (verde/rojo/amarillo), con fondo coloreado
  - **Asistencia** — registro de asistencia completo
  - **Graficas** — gráficas de cambios por Mes, Semana y Año
- **Tiempo real:** Todos los usuarios ven los cambios al instante gracias a Firebase Firestore.

---

## Stack

| Tecnología | Uso |
|---|---|
| HTML + Vanilla JS | App completa en un solo archivo `index.html` |
| Tailwind CSS (CDN) | Estilos |
| Chart.js (CDN) | Gráficas del dashboard |
| ExcelJS (CDN) | Construcción del workbook Excel |
| JSZip (CDN) | Inyección de gráficas reales en el `.xlsx` |
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
cd ~/time-ci-telamon

git add index.html
git commit -m "descripción del cambio"
git push
```

GitHub Actions despliega automáticamente a GitHub Pages en ~1 minuto.

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

La primera vez que alguien abre la app en un Firestore vacío, el sistema sube automáticamente los registros originales del Excel + los datos de asistencia. No se necesita hacer nada manualmente.

---

## Datos originales

Migrados del archivo `Time CI 05-28.xlsm`:
- **83 registros** de cambios CI (supervisores: Gabriela, Julio | plataformas: Autoliv, ZF, JSS, Ultraform)
- **29 personas** en el registro de asistencia (10 fechas de reunión, marzo–junio 2026)

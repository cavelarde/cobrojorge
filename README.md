# 🛵 CrediControl Pro - Sistema Integrado de Gestión de Cobros y Microcréditos

¡Bienvenido a **CrediControl Pro**! Esta es una solución web *Fintech* diseñada específicamente para administrar pequeños préstamos individuales, controlar cobros por cuotas diarias/semanales, registrar gastos operativos y archivar cierres de caja en tiempo real. 

El sistema está optimizado para funcionar bajo una arquitectura descentralizada y ligera: un frontend estático alojado en **Netlify** conectado de forma segura mediante peticiones REST API (`POST`) a un backend basado en **Google Apps Script**, utilizando **Google Sheets** como base de datos multi-hoja.

---

## 🚀 Características Principales

### 📱 Para los Cobradores (Ruta de Cobro en la Calle)
*   **Acceso Independiente con PIN:** Autenticación rápida y segura por perfil para evitar interferencias entre rutas.
*   **Monitoreo de Caja en Vivo:** Visualización del flujo de efectivo del día en tiempo real (Base inicial + Recaudos - Préstamos otorgados - Gastos de ruta).
*   **Gestión de Clientes Inteligente:** Filtro automático que muestra únicamente las tarjetas de cobro pendientes del cobrador logueado.
*   **Cierre Diario Automatizado:** Al finalizar la jornada, se consolidan las métricas y se archivan en la nube. La interfaz realiza una **limpieza absoluta a cero** y cierra sesión automáticamente, quedando lista para el día siguiente.

### ⚡ Para el Administrador (Panel de Control Central)
*   **Acceso Restringido mediante Clave Maestra:** Sección interna protegida para supervisar las finanzas globales.
*   **Métricas Consolidadas:** Panel financiero con la sumatoria global de recaudo, préstamos, gastos operativos y cálculo de cartera activa total.
*   **Creación de Rutas Automatizada:** Permite registrar nuevos cobradores asignándoles un PIN único. El sistema crea e inserta de forma automática una nueva pestaña personalizada en el libro de Google Sheets.
*   **Filtros de Auditoría:** Visualización completa de clientes activos de manera global o segmentada por cobrador.

---

## 🛠️ Arquitectura Tecnológica y Conexión Sin Bloqueos (CORS)

Para garantizar la velocidad y evadir las restricciones de red entre dominios cruzados, el sistema migró de las funciones nativas de Google (`google.script.run`) a un canal unificado **API REST (JSON)**:

1.  **Frontend (Netlify):** Ejecuta peticiones asíncronas (`fetch`) enviando la información estructurada en formato plano (`text/plain;charset=utf-8`).
2.  **Backend (Google Apps Script):** Recibe las solicitudes en una función centralizada `doPost(e)`, procesa la lógica del negocio (escribir o leer) y responde con un objeto JSON limpio y seguro.

---

## 📦 Estructura del Proyecto

El repositorio cuenta con un diseño modular unificado en un solo archivo de producción para el cliente:

```text
📦 tu-repositorio-credicontrol
 ┣ 📄 index.html        # Frontend integrado: Login, Ruta de Cobradores y Panel de Administración (Tailwind CSS)
 ┣ 📄 Código.gs         # Backend en Apps Script (Funciones de base de datos multi-hoja en Google Sheets)
 ┗ 📄 README.md         # Documentación del sistema
```

---

## 🔧 Guía de Instalación y Despliegue

### 1. Configuración del Backend (Google Sheets + Apps Script)
1. Crea una nueva hoja de cálculo en **Google Sheets**.
2. Ve al menú superior **Extensiones > Apps Script**.
3. Reemplaza todo el código del editor por el contenido del archivo `Código.gs` de este repositorio.
4. Haz clic en **Implementar > Nueva implementación**.
5. Configura los parámetros obligatorios de seguridad:
    *   **Tipo:** Aplicación web
    *   **Ejecutar como:** Tu cuenta (Yo)
    *   **Quién tiene acceso:** Cualquier persona (*vital para recibir los datos desde Netlify*).
6. Copia la **URL de la aplicación web** generada (Termina en `/exec`).

### 2. Configuración del Frontend (`index.html`)
1. Abre el archivo `index.html`.
2. Busca la línea de configuración inicial en JavaScript:
   ```javascript
   const GOOGLE_API_URL = "https://google.com";
   const CLAVE_MAESTRA_SISTEMA = "TuContraseñaSecretaAdmin";
   ```
3. Pega la URL que copiaste en el paso anterior y define tu clave maestra para el Administrador. Guarda los cambios.

### 3. Despliegue en Netlify
1. Inicia sesión en tu cuenta de [Netlify](https://netlify.com).
2. Arrastra la carpeta que contiene tu archivo `index.html` a la sección de carga rápida de Netlify (*Drag and drop*).
3. ¡Listo! Tu plataforma estará en línea en un subdominio seguro con certificado SSL (Ej: `https://netlify.app`).

---

## 🔒 Buenas Prácticas de Mantenimiento
Cada vez que realices modificaciones en el backend (`Código.gs`), recuerda ir a **Implementar > Gestionar implementaciones** en Apps Script, editar la implementación activa y seleccionar **"Nueva versión"**. De esta forma, el backend se actualizará de inmediato sin alterar la URL y sin necesidad de volver a subir tus archivos a Netlify.

---
Generado con fines operativos y de control financiero eficiente. 🚀

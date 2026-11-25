# 📇 Agenda de Contactos — Mini Proyecto 2 (Unidad 4)

Aplicación web desarrollada en **PHP + MySQL (PDO)** que implementa un **CRUD completo**
para gestionar una agenda de contactos: registrar, listar, buscar, ver el detalle,
editar y eliminar contactos con fotografía.

> **Materia:** Programación Web
> **Unidad:** Unidad 4 — Programación del lado del servidor con PHP
> **Actividad:** Mini Proyecto 2 — Desarrollo Web con PHP y Base de Datos (CRUD)

---

## 👥 Integrantes del equipo

| # | Nombre completo | Matrícula |
|---|-----------------|-----------|
| 1 | Brandon Emilio Hernández | — |
| 2 | Paola Kasumy Gómez Sánchez | — |
| 3 | Christian Eduardo Rosales González | 25270553 |

**Docente:** _(completar)_
**Institución:** _INSTITUTO TECNOLÓGICO NACIONAL DE MEXICO
CAMPUS TUXTLA GUTIÉRREZ_
**Fecha de entrega:** Noviembre 2026

---

## 🔗 Enlaces de entrega

| Recurso | Enlace |
|---------|--------|
| Repositorio GitHub | https://github.com/DonLalencio/MIniProyecto2-Unidad-4 |
| Sistema publicado (hosting) | https://agenda-contactos845.rf.gd/ |
| Reporte de evidencias (PDF) | [`docs/Reporte_Evidencias_Agenda.pdf`](docs/Reporte_Evidencias_Agenda.pdf) |

---

## 📖 Descripción del proyecto

**Agenda de Contactos** es un sistema web que permite administrar la información de
personas (nombre, apellido, teléfono, correo, dirección, notas y fotografía) desde el
navegador. Toda la lógica se ejecuta **en el servidor con PHP** y la información se
almacena en una base de datos **MySQL/MariaDB**; no se utiliza `localStorage`, JSON ni
ningún almacenamiento del lado del cliente.

La aplicación aplica las buenas prácticas vistas en la unidad: conexión con **PDO**,
**sentencias preparadas** para todas las consultas, **validación en servidor**,
**escape de salida con `htmlspecialchars()`**, **tokens CSRF** y separación entre
configuración, lógica y vistas.

---

## 🛠️ Tecnologías utilizadas

| Tecnología | Uso en el proyecto |
|------------|--------------------|
| **PHP 8.x** | Lógica del servidor, validaciones, subida de archivos |
| **MySQL / MariaDB 10.x+** | Base de datos relacional de la agenda |
| **PDO** | Capa de acceso a datos con sentencias preparadas |
| **HTML5** | Formularios con validación nativa (`required`, `type`, `pattern`) |
| **CSS3 (externo)** | Diseño responsivo, sin frameworks (archivo `css/styles.css`) |
| **Apache** | Servidor web (con `.htaccess` de protección) |

---

## ✅ Funcionalidades implementadas (CRUD completo)

| Operación | Archivo(s) | Descripción |
|-----------|-----------|-------------|
| **CREATE** | `crear.php` → `guardar.php` | Formulario HTML5 y `INSERT` con `prepare()/execute()` |
| **READ (listado)** | `index.php` | Rejilla de contactos con **búsqueda** y paginación |
| **READ (individual)** | `ver.php` | Ficha completa del contacto (`SELECT ... WHERE id = :id`) |
| **UPDATE** | `editar.php` → `actualizar.php` | Formulario precargado y `UPDATE` preparado |
| **DELETE** | `eliminar.php` | Pantalla de confirmación (POST) + `DELETE` preparado |

Además:

- 🔍 **Búsqueda** por nombre, apellido, teléfono o correo (`LIKE` con parámetros).
- 🖼️ **Subida de fotografía obligatoria** en la creación (JPG, PNG, WEBP, GIF · máx. 2 MB),
  con verificación del **MIME real** y nombre de archivo generado aleatoriamente.
- 📄 **Paginación** de 9 contactos por página conservando el término de búsqueda.
- 🔔 **Mensajes flash** de éxito/error tras cada operación (patrón POST → Redirect → GET).
- 🧾 **Trazabilidad**: cada registro guarda `creado_en` y `actualizado_en`.

---

## 🔐 Seguridad implementada

| Medida | Dónde se aplica |
|--------|-----------------|
| **Sentencias preparadas PDO** (`ATTR_EMULATE_PREPARES = false`) | Todas las consultas (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) |
| **`htmlspecialchars()`** en toda salida | `includes/funciones.php` → `e()` |
| **Validación en servidor** | `validar_contacto()` en `includes/funciones.php` |
| **Token CSRF** en todos los formularios | `csrf_token()`, `csrf_campo()`, `csrf_valido()` |
| **Errores SQL nunca visibles** | `try/catch` + `error_log()` en cada operación |
| **Verificación de archivos subidos** | `finfo` (MIME real) + `getimagesize()` + límite de 2 MB |
| **Protección de carpetas sensibles** | `.htaccess` en `config/`, `includes/`, `database/`, `archivos/` |
| **Listado de directorios deshabilitado** | `.htaccess` raíz (`Options -Indexes`) |
| **Escapado de salida en la búsqueda** | El término se imprime con `e()` |

---

## ⚙️ Requisitos del servidor

| Requisito | Detalle |
|-----------|---------|
| PHP | 8.0 o superior (probado en 8.4) |
| Extensiones | `pdo_mysql` (obligatoria), `mbstring` y `fileinfo` (recomendadas) |
| MySQL / MariaDB | 5.7+ / 10.2+ con `utf8mb4` |
| Subida de archivos | `file_uploads = On`, `upload_max_filesize ≥ 2M` |
| Servidor web | Apache con `.htaccess` (o Nginx con reglas equivalentes) |

> El código funciona aunque `mbstring` o `fileinfo` no estén activas: usa funciones de respaldo
> (`longitud()`, `mime_de_archivo()`). Aun así se recomienda activarlas desde el panel del hosting.

---

## 📁 Estructura del proyecto

```
/
├── index.php              # Listado + búsqueda de contactos (READ)
├── crear.php              # Formulario de creación (CREATE · vista)
├── guardar.php            # Procesa el INSERT (CREATE · lógica)
├── editar.php             # Formulario de edición (UPDATE · vista)
├── actualizar.php         # Procesa el UPDATE (UPDATE · lógica)
├── ver.php                # Detalle del contacto (READ individual)
├── eliminar.php           # Confirmación + DELETE
│
├── config/
│   ├── db.php             # Conexión PDO a MySQL (credenciales y opciones)
│   └── .htaccess          # Bloquea el acceso web directo
│
├── includes/
│   ├── funciones.php      # Escapado, sesión, CSRF, validación, subida de fotos
│   ├── header.php         # Cabecera común (HTML head, avisos flash)
│   ├── menu.php           # Menú de navegación común
│   ├── footer.php         # Pie de página común
│   └── .htaccess
│
├── css/
│   └── styles.css         # Hoja de estilos externa única
│
├── archivos/              # Fotografías subidas + imágenes de ejemplo
│   ├── demo-*.png
│   └── .htaccess          # Impide ejecutar scripts en la carpeta
│
├── database/
│   └── agenda.sql         # Script de creación de la tabla + datos de ejemplo
│
├── docs/                  # Reporte de evidencias (PDF) y capturas
├── tools/                 # Scripts de apoyo (generador de avatares, pruebas)
└── README.md
```

---

## 🚀 Instalación local (XAMPP / Laragon / WAMP)

1. **Copiar el proyecto** dentro del directorio público del servidor web:
   - XAMPP: `C:\xampp\htdocs\agenda-contactos`
   - Laragon: `C:\laragon\www\agenda-contactos`

2. **Crear la base de datos** e importar la tabla:

   ```bash
   mysql -u root -p < database/agenda.sql
   ```

   O desde phpMyAdmin: *Importar* → seleccionar `database/agenda.sql`.
   (El script crea la tabla `contactos` y seis registros de ejemplo.)

3. **Configurar la conexión** en `config/db.php`:

   ```php
   const DB_HOST = 'localhost';
   const DB_NAME = 'agenda_contactos';
   const DB_USER = 'root';
   const DB_PASS = '';
   ```

4. **Verificar permisos de escritura** en la carpeta `archivos/`
   (Linux/Mac: `chmod 755 archivos`).

5. Abrir en el navegador: **http://localhost/agenda-contactos/**

---

## 🌐 Publicación en hosting gratuito (InfinityFree / AeonFree / ByetHost)

> **Sistema ya publicado:** <https://agenda-contactos845.rf.gd/> (InfinityFree, PHP 8 + MySQL).
> La guía detallada de despliegue está en [`docs/DESPLIEGUE_INFINITYFREE.md`](docs/DESPLIEGUE_INFINITYFREE.md).

1. Registrarse y crear una **cuenta + sitio** en el hosting.
2. En el panel, abrir **MySQL Databases** y crear una base de datos
   (por ejemplo `if0_XXXXXXXX_agenda`). Anotar: *host*, *usuario*, *contraseña* y *nombre*.
3. Entrar a **phpMyAdmin** del hosting, seleccionar la base creada e **importar**
   `database/agenda.sql` (sin las líneas `CREATE DATABASE` / `USE` si el hosting ya la creó).
4. Subir los archivos por **FTP** (FileZilla) o con el **File Manager** a la carpeta `htdocs`.
   ⚠️ La carpeta `archivos/` debe subirse con permisos de escritura (`755`).
5. Editar `config/db.php` en el servidor con los datos del hosting:

   ```php
   const DB_HOST = 'sqlXXX.infinityfree.com';
   const DB_NAME = 'if0_XXXXXXXX_agenda';
   const DB_USER = 'if0_XXXXXXXX';
   const DB_PASS = 'contraseña_del_panel';
   ```

6. Abrir `https://mi-sitio.infinityfreeapp.com/` y probar el CRUD completo.

---

## 🗄️ Base de datos

```sql
CREATE TABLE contactos (
    id             INT UNSIGNED NOT NULL AUTO_INCREMENT,
    nombre         VARCHAR(100)  NOT NULL,
    apellido       VARCHAR(100)  NOT NULL,
    telefono       VARCHAR(30)   NOT NULL,
    foto           VARCHAR(255)  NOT NULL,
    email          VARCHAR(150)  DEFAULT NULL,
    direccion      VARCHAR(255)  DEFAULT NULL,
    notas          TEXT          DEFAULT NULL,
    creado_en      TIMESTAMP     NOT NULL DEFAULT CURRENT_TIMESTAMP,
    actualizado_en TIMESTAMP     NOT NULL DEFAULT CURRENT_TIMESTAMP
                                 ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (id),
    KEY idx_nombre_apellido (nombre, apellido),
    KEY idx_telefono (telefono),
    KEY idx_email (email)
) ENGINE = InnoDB DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```

---

## 🧪 Pruebas realizadas

El script `tools/pruebas_crud.py` ejecuta 36 verificaciones automáticas contra el
servidor local (requiere Python 3 y `requests`):

```bash
python tools/pruebas_crud.py
```

Cubre: listado, búsqueda, intento de **inyección SQL** y **XSS**, validación de
servidor, CSRF, alta con fotografía, detalle, actualización, borrado con confirmación
y borrado del archivo de imagen.

---

## 📚 Créditos

- Iconos: emojis nativos del sistema (sin dependencias externas).
- Fotografías de ejemplo: avatares generados con `tools/generar_demo_fotos.py`.
- Tipografía: fuente del sistema (`Segoe UI` / `system-ui`).

---

<p align="center">Mini Proyecto 2 · Unidad 4 · Programación Web · 2026</p>

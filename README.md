# MediCompu_ISTE-
# 🏥 MediCompu_ISTE: Sistema de Gestión Técnica Segura

# 🏥 MediCompu_ISTE: Sistema de Gestión Técnica Segura

## 🌟 Resumen del Proyecto

Sistema de gestión de servicios técnicos desarrollado en **Python 3** y **SQLite3** con un enfoque primordial en la **seguridad informática** y la **trazabilidad completa** de los servicios. Este sistema automatiza la gestión de equipos (laptops, impresoras, etc.) para la empresa MediCompu, reemplazando procesos manuales propensos a errores y pérdidas de información.

### 🎯 Objetivos Principales

| Categoría | Descripción |
| :--- | :--- |
| **Objetivo General** | Automatizar la gestión integral de servicios técnicos y la pre-facturación, eliminando errores manuales, asegurando la **integridad de los datos** y optimizando la trazabilidad de los equipos. |
| **Seguridad** | Garantizar un sistema de autenticación robusto mediante **hashing con salt** (`hashlib`) y prevenir vulnerabilidades implementando **consultas parametrizadas** contra la Inyección SQL. |
| **Trazabilidad** | Proporcionar informes detallados del ciclo de vida del equipo (ingreso, estado, valor, técnico asignado) como base directa para una facturación eficiente. |

---

## 🛠️ Tecnologías y Arquitectura

* **Nombre del Proyecto:** MediCompu_ISTE
* **Lenguaje:** Python 3.x
* **Base de Datos:** SQLite3 (Base de datos de archivo local)
* **Arquitectura:** Monolito de Consola (CLI) de Tres Capas Lógicas.
* **Ventaja Arquitectónica:** Máxima simplicidad de despliegue y seguridad de conexión local (no expone puertos de red ni APIs).

---

## 🚀 Guía de Instalación y Ejecución (Paso a Paso)

El programa es una aplicación de consola (CLI) y solo requiere tener **Python 3.x** instalado.

### Paso 1: Preparación del Entorno

1.  Asegúrese de tener **Python 3.x** instalado.
2.  Descargue o clone este repositorio en una carpeta de su elección.

### Paso 2: Ejecución del Programa

1.  Abra la **Terminal** o **Símbolo del Sistema** de su computadora.
2.  Use el comando `cd` para navegar a la carpeta del proyecto:
    ```bash
    cd /ruta/donde/descargaste/MediCompu_ISTE
    ```
3.  Ejecute el script principal:
    ```bash
    python medicompu_system.py
    ```
    *(Si el comando `python` no funciona, use `python3`)*

### Paso 3: Acceso Inicial y Credenciales

Al iniciar por primera vez, el sistema creará el archivo de base de datos (`medicompu_db.db`), cargará la estructura y los 30+ registros de ejemplo.

| Rol | Usuario | Contraseña |
| :--- | :--- | :--- |
| **Administrador** | `admin` | `AdminSeguro#2025` |

---

## 🔒 Pruebas de Seguridad y Funcionalidades Clave

### 1. Infiltración Preventiva (Anti-Hacker)

Para demostrar la seguridad del sistema contra ataques externos:

* **En el menú principal, seleccione la Opción 4.**
* Esta prueba simula una Inyección SQL (`' OR '1'='1`) y confirmará que el sistema **RECHAZA** el acceso, demostrando la eficacia de las consultas parametrizadas.

### 2. Creación de Usuarios Seguros

* **Opción 3** (solo accesible para el usuario `admin`).
* El sistema exige una **contraseña segura** que debe incluir longitud mínima, mayúsculas, minúsculas, números y **símbolos**, validando la seguridad de las credenciales de acceso.

### 3. Trazabilidad para Facturación

* **Opción 2** genera el **Informe de Trazabilidad**, que lista todos los equipos, sus estados (`Listo` es clave para facturación) y el valor estimado del servicio, eliminando la dependencia del registro manual.

---

## ⚙️ Fragmento de Código Clave (Seguridad)

El siguiente fragmento demuestra la implementación de las **Consultas Parametrizadas** en la función de autenticación, la defensa estándar contra la Inyección SQL:

```python
# Función de autenticación segura
def autenticar(conn, username, password):
    cursor = conn.cursor()
    
    # 💥 CLAVE DE SEGURIDAD: Uso de (?) para Consultas Parametrizadas
    # Esto asegura que el valor de 'username' siempre sea tratado como un dato (string),
    # NO como parte de la instrucción SQL.
    cursor.execute("SELECT password_hash FROM users WHERE username = ?", (username,))
    
    user_record = cursor.fetchone()
    # check_password verifica el hash almacenado con el salt.
    if user_record and check_password(user_record['password_hash'], password):
        return username
    return None

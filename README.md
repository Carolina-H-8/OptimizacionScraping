# 💻 Web Scraping & Catálogo Dinámico — Portátiles Usados (SITEC SAS)

## 🌐 Ver Catálogo en Vivo
👉 **[Haz clic aquí para abrir la página web interactiva](https://carolina-h-8.github.io/OptimizacionScraping/)**

Proyecto de extracción automatizada de datos web (*Web Scraping*) diseñado para monitorear, estructurar y visualizar en tiempo real el catálogo de computadores portátiles corporativos usados y reacondicionados de **SITEC SAS**.

El flujo de trabajo integra **Python** para la extracción de datos, **GitHub Actions** para la ejecución continua en la nube y una interfaz web estática (**HTML/CSS/JS**) alojada en **GitHub Pages** que consume los datos actualizados dinámicamente.

---

## 📋 Tabla de Contenidos
1. [Arquitectura del Proyecto](#-arquitectura-del-proyecto)
2. [Estructura del Repositorio](#-estructura-del-repositorio)
3. [Tecnologías Utilizadas](#-tecnologías-utilizadas)
4. [Instalación y Ejecución Local (VS Code)](#-instalación-y-ejecución-local-vs-code)
5. [Automatización con GitHub Actions](#-automatización-con-github-actions)
6. [Estructura de los Datos (`datos.json`)](#-estructura-de-los-datos-datosjson)
7. [Autores](#-autores)

---

## 🏗 Arquitectura del Proyecto

```text
┌─────────────────────────┐      HTTP GET       ┌──────────────────────────┐
│     SITEC SAS Web       │ ◄────────────────── │   Script Python (.py)    │
│ (Catálogo WooCommerce)  │ ──────────────────► │  BeautifulSoup + Pandas  │
└─────────────────────────┘     HTML Estático   └────────────┬─────────────┘
                                                             │ Genera / Actualiza
                                                             ▼
┌─────────────────────────┐     Fetch API       ┌──────────────────────────┐
│   Interfaz Web (HTML)   │ ◄────────────────── │        datos.json        │
│   GitHub Pages / Local  │                     │   (Repositorio GitHub)   │
└─────────────────────────┘                     └──────────────────────────┘
```

1. **Extracción:** El script consulta el catálogo público de SITEC SAS y analiza las tarjetas de producto mediante selectores CSS específicos de su estructura WooCommerce/WoodMart (`.wd-product`).
2. **Transformación:** `pandas` limpia registros duplicados y estructura la información (nombre, precio original, precio con descuento, enlace directo e imagen).
3. **Almacenamiento:** Los resultados se exportan a `datos.json` junto con la marca de tiempo (`ultima_actualizacion`).
4. **Visualización:** `index.html` lee `datos.json` de forma asíncrona y ofrece búsqueda y filtrado instantáneo en el navegador.

---

## 📁 Estructura del Repositorio

```text
├── .github/
│   └── workflows/
│       └── main.yml                                      # Pipeline de CI/CD en GitHub Actions
├── argumedo_kevin_hernadez_astrid_analizando_ea1.py      # Script principal de Web Scraping
├── datos.json                                            # Base de datos JSON generada automáticamente
├── index.html                                            # Interfaz web dinámica con buscador y filtros
├── requirements.txt                                      # Dependencias de Python
├── .gitignore                                            # Exclusión de entornos virtuales y caché
└── README.md                                             # Documentación del proyecto
```

---

## 🛠 Tecnologías Utilizadas

* **Lenguaje Backend:** Python 3.10+
* **Librerías de Scraping y Datos:**
  * `requests`: Peticiones HTTP con encabezados personalizados (`User-Agent`).
  * `beautifulsoup4`: Análisis y extracción de nodos HTML.
  * `pandas`: Limpieza, deduplicación y serialización de datos.
* **Frontend:** HTML5, CSS3 (Grid Layout responsivo) y JavaScript (ES6+ Fetch API).
* **DevOps / Automatización:** GitHub Actions & GitHub Pages.

---

## 🚀 Instalación y Ejecución Local (VS Code)

Sigue estos pasos para ejecutar y probar el proyecto en tu computadora:

### 1. Clonar el repositorio
```bash
git clone https://github.com/Carolina-H-8/OptimizacionScraping.git
cd OptimizacionScraping
```

### 2. Crear y activar el entorno virtual
En **Windows (PowerShell/CMD):**
```bash
python -m venv .venv
.venv\Scripts\activate
```

En **macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```
*(Si usas Visual Studio Code, también puedes crear el entorno desde la paleta de comandos seleccionando `requirements.txt`).*

### 3. Instalar dependencias
```bash
pip install -r requirements.txt
```

### 4. Ejecutar el Web Scraper
```bash
python argumedo_kevin_hernadez_astrid_analizando_ea1.py
```
Al finalizar, verás en consola el total de equipos extraídos y se actualizará el archivo `datos.json`.

### 5. Visualizar la página web localmente
Debido a políticas de seguridad de los navegadores (CORS), el archivo `index.html` debe abrirse mediante un servidor local para leer `datos.json`:
* **Opción A (VS Code):** Haz clic derecho sobre `index.html` y selecciona **Open with Live Server**.
* **Opción B (Terminal):** Ejecuta `python -m http.server 8000` y abre `http://localhost:8000` en tu navegador.

---

## ⚙️ Automatización con GitHub Actions

El proyecto cuenta con un flujo de trabajo configurado en `.github/workflows/` que ejecuta el scraper automáticamente:
* **Disparadores (`on`):** Se ejecuta ante cada `push` en la rama `main`, de forma programada mediante `cron` o manualmente desde la pestaña **Actions** (`workflow_dispatch`).
* **Auto-Commit:** Si se detectan cambios o nuevos productos en `datos.json`, el bot de GitHub Actions realiza un commit automático y actualiza el despliegue en **GitHub Pages** (`[https://carolina-h-8.github.io/OptimizacionScraping/](https://carolina-h-8.github.io/OptimizacionScraping/)`).

---

## 📦 Estructura de los Datos (`datos.json`)

El archivo generado sigue este esquema estándar:

```json
{
    "ultima_actualizacion": "2026-11-25 19:05:35",
    "total": 8,
    "productos": [
        {
            "Nombre": "HP ProBook 440 G8 | Portátil Corporativo",
            "Precio original": "$ 1.425.000",
            "Precio con descuento": "$ 1.399.000",
            "Enlace": "https://sitecsas.com/producto/hp-probook-440-g8-portatil-corporativo/",
            "Imagen": "https://sitecsas.com/wp-content/uploads/hp-probook-440-g8-portatil-corporativo-vista-frontal.webp"
        }
    ]
}
```

---

## 👥 Autores

* **Kevin Argumedo**
* **Astrid Carolina Hernández**

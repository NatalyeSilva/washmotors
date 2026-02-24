**Wash Motors** es una landing web para un servicio de lavado de vehículos a domicilio, enfocada en la comodidad del cliente y una experiencia moderna y clara.

El proyecto combina **FastAPI**, **Jinja2**, **Tailwind CSS** y **JavaScript**, consumiendo datos dinámicos desde una **planilla de Google Sheets**.

---

## 🛠️ Tecnologías utilizadas

- **FastAPI** – Backend y routing
- **Jinja2** – Renderizado de templates HTML
- **Tailwind CSS** – Estilos y diseño responsive
- **JavaScript** – Interacciones (menú móvil, carruseles, cards flip)
- **Google Sheets (CSV)** – Fuente dinámica de servicios
- **HTML / CSS / JS**

---

## ✨ Funcionalidades principales

- Landing page responsive
- Servicios dinámicos cargados desde Google Sheets
- Carrusel horizontal de servicios
- Cards con efecto *flip* (“Ver más”)
- Navegación suave entre secciones
- Menú mobile interactivo
- Contacto directo vía WhatsApp y Calendly

---

## 📡 Documentación de API

### Base URL
```
http://localhost:8000
```

### Endpoints

#### `GET /`
Renderiza la página principal de la landing page.

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| - | - | No requiere parámetros |

**Respuesta:** HTML renderizado con Jinja2

**Contexto enviado al template:**
| Variable | Tipo | Descripción |
|----------|------|-------------|
| `request` | `Request` | Objeto de la petición FastAPI |
| `items` | `list[dict]` | Lista de todos los servicios |
| `grouped_items` | `dict` | Servicios agrupados por tipo de lavado |
| `max_price` | `float` | Precio máximo entre todos los servicios |

---

#### `GET /items`
Retorna los servicios en formato JSON (ideal para consumo desde JavaScript o aplicaciones externas).

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| - | - | No requiere parámetros |

**Respuesta:** `application/json`

```json
{
  "items": [
    {
      "Tipo de lavado": "Lavado Exterior",
      "Nombre": "Lavado Express",
      "Descripcion": "Lavado rápido exterior",
      "Precio": "50000"
    }
  ],
  "grouped": {
    "Lavado Exterior": [...],
    "Lavado Interior": [...]
  },
  "max_price": 150000
}
```

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `items` | `array` | Lista completa de servicios |
| `grouped` | `object` | Servicios agrupados por `Tipo de lavado` |
| `max_price` | `number` | Precio máximo encontrado |

---

### Fuente de Datos

Los servicios se obtienen dinámicamente desde una planilla de **Google Sheets** exportada como CSV:

```
https://docs.google.com/spreadsheets/d/1wCeeO7EnrESFsCdEFN-5oTPyHCXAEjypiaIDYgTnXl4/export?format=csv
```

**Columnas esperadas en el CSV:**
| Columna | Descripción |
|---------|-------------|
| `Tipo de lavado` | Categoría del servicio (ej: "Lavado Exterior") |
| `Nombre` | Nombre del servicio |
| `Descripcion` | Descripción detallada |
| `Precio` | Precio en guaraníes |

---

### Funciones Auxiliares

| Función | Descripción |
|---------|-------------|
| `fix_encoding(text)` | Repara strings con encoding incorrecto (latin1 → utf-8) |
| `load_csv()` | Descarga y parsea el CSV desde Google Sheets |
| `group_by_type(items)` | Agrupa servicios por `Tipo de lavado` |
| `get_max_price(items)` | Obtiene el precio máximo de todos los servicios |

---

### Middleware

#### `ProxyHeadersMiddleware`
Detecta conexiones HTTPS detrás de un proxy (Railway, Heroku, etc.) mediante el header `x-forwarded-proto`.

---

### Ejecutar el servidor

```bash
# Instalar dependencias
pip install -r requirements.txt

# Iniciar servidor de desarrollo
uvicorn main:app --reload

# Iniciar en producción
uvicorn main:app --host 0.0.0.0 --port 8000
```

**Documentación interactiva automática:**
- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

---

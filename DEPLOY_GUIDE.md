# 🚀 Guía de Deploy: Netlify + PythonAnywhere

## Resumen
Tu aplicación está separada en dos partes:
- **Frontend** (HTML, CSS, JS) → Netlify
- **Backend** (FastAPI + datos) → PythonAnywhere

## Paso 1: Deploy Backend en PythonAnywhere

### 1.1 Crear Cuenta
1. Ve a [pythonanywhere.com](https://www.pythonanywhere.com)
2. Regístrate gratis (confirma email)

### 1.2 Subir Código
1. Abre **Bash console** en PythonAnywhere
2. Clona tu repo o sube los archivos:
```bash
git clone https://github.com/tu-user/web_wash.git
# o si no tienes git, usa SCP/SFTP
```

### 1.3 Crear Virtualenv
```bash
cd web_wash
mkvirtualenv --python=/usr/bin/python3.10 web_wash
pip install -r requirements.txt
pip install gunicorn
```

### 1.4 Configurar Web App
1. Ve a **Web** → **Add a new web app**
2. Elige **Manual configuration** → **Python 3.10**
3. En **Virtualenv**, escribe:
   ```
   /home/tu-usuario/.virtualenvs/web_wash
   ```
4. En **WSGI configuration file** (editar), reemplaza el contenido con:
   ```python
   import sys
   sys.path.insert(0, '/home/tu-usuario/web_wash')
   
   from main import app
   application = app
   ```

### 1.5 Recargar
- Haz click en **Reload** en la pestaña **Web**
- Tu backend estará en: `https://tu-usuario.pythonanywhere.com`

---

## Paso 2: Deploy Frontend en Netlify

### 2.1 Preparar Archivos
1. Copia estos archivos a una carpeta localizada como `web-wash-frontend`:
```
templates/index-static.html  →  index.html (renombrar)
static/css/styles.css        →  css/styles.css
static/js/script-dynamic.js  →  js/script.js (renombrar)
```

**Estructura final:**
```
web-wash-frontend/
├── index.html
├── css/
│   └── styles.css
└── js/
    └── script.js
```

### 2.2 Actualizar API URL
En `js/script.js`, línea 2, reemplaza:
```javascript
const API_URL = 'https://tu-usuario.pythonanywhere.com';
```
Con tu URL real (sin trazo final).

### 2.3 Deploy a Netlify
1. Ve a [netlify.com](https://www.netlify.com)
2. Inicia sesión (GitHub, GitLab, Bitbucket)
3. Click en **New site from Git** o **Elige carpeta**
4. Sube la carpeta `web-wash-frontend`
5. **Deploy settings:**
   - **Build command:** (dejar vacío)
   - **Publish directory:** `.` (punto)
6. Click **Deploy**

Tu sitio estará en: `https://tu-dominio.netlify.app`

---

## Paso 3: Conectar Frontend + Backend

### 3.1 Verificar CORS
Tu `main.py` ya tiene CORS habilitado. Verifica que la API funcione:
```bash
curl -i https://tu-usuario.pythonanywhere.com/items
# Deberías ver 200 OK y los datos en JSON
```

### 3.2 Probar Frontend
1. Abre `https://tu-dominio.netlify.app`
2. Si ves las tarjetas con datos (de Google Sheets), ¡funciona! ✅

---

## Paso 4: Usar Dominio Personalizado (Opcional)

### 4.1 En PythonAnywhere
1. Ve a **Web** → **Domains**
2. Haz click en tu dominio `.pythonanywhere.com`
3. Click en **Add a domain name**
4. Escribe tu dominio (ej: `backend.misitio.com`)
5. PythonAnywhere te dará instrucciones DNS

### 4.2 En Netlify
1. Ve a **Domain Settings**
2. Click en **Add custom domain**
3. Escribe tu dominio (ej: `lavadero.misitio.com`)
4. Sigue instrucciones de DNS

### 4.3 Actualizar Script
En `js/script.js`, actualiza con tu dominio personalizado:
```javascript
const API_URL = 'https://backend.misitio.com';
```

---

## 🔧 Troubleshooting

### "Tarjetas no cargan datos"
1. Abre DevTools (F12) → Console
2. Busca errores CORS o 404
3. Verifica que `API_URL` sea correcta en script.js

### "Error 502 en PythonAnywhere"
- Reload la web app
- Verifica que los requirements estén instalados
- Check PythonAnywhere error logs

### "Netlify despliega pero ve en blanco"
- Verifica que `index.html` esté en la raíz
- Que exista `css/styles.css`
- Que exista `js/script.js`

---

## Archivos Incluidos

1. **main.py** - Backend FastAPI actualizado con CORS
2. **requirements.txt** - Dependencias Python
3. **templates/index-static.html** - HTML estático para Netlify
4. **static/js/script-dynamic.js** - JavaScript que carga datos
5. **static/css/styles.css** - CSS (sin cambios)

---

## Notas Importante

✅ **Ventajas de esta separación:**
- Frontend funciona infinitamente en Netlify (gratis)
- Backend en PythonAnywhere con tier gratuito
- No depende de Jinja2 en el cliente
- Escalable y moderno

⚠️ **Límites gratis:**
- **PythonAnywhere**: 100 CPU-segundos/día (suficiente para una tienda pequeña)
- **Netlify**: Ilimitado para hosting estático

---

## ¿Preguntas?

Si algo no funciona, verifica:
1. Que las URLs sean correctas
2. Que CORS esté habilitado (línea 12-18 en main.py)
3. Que el script.js llame a la API correcta
4. Valida requests con `curl` desde terminal

# WASH MOTORS - Deploy Rápido

## 📋 Resumen
Tu app está lista para ***split deploy***:
- **Frontend** → Netlify (gratis, ilimitado)
- **Backend** → PythonAnywhere (gratis con límites)

## 🚀 Pasos Rápidos

### Backend (PythonAnywhere)
```bash
1. Crea cuenta en pythonanywhere.com
2. Upload código: git clone...
3. pip install -r requirements.txt
4. Configura Web app (WSGI → main.py)
5. Reload
```
**URL:** `https://tu-usuario.pythonanywhere.com`

### Frontend (Netlify)
```bash
1. Carpeta: index.html + css/ + js/
2. Edita js/script.js: 
   const API_URL = 'https://tu-usuario.pythonanywhere.com'
3. Sube carpeta a Netlify (Drop & Deploy)
```
**URL:** `https://tu-dominio.netlify.app`

---

## 📁 Archivos Nuevos

| Archivo | Propósito |
|---------|-----------|
| `templates/index-static.html` | HTML para Netlify (sin Jinja2) |
| `static/js/script-dynamic.js` | JS que carga datos con fetch() |
| `DEPLOY_GUIDE.md` | Guía completa paso a paso |

## ✅ Verificación

- [ ] Backend responde en `/items` (prueba con curl)
- [ ] Frontend carga en Netlify
- [ ] DevTools → Console sin errores CORS
- [ ] Tarjetas muestran datos

---

Ver `DEPLOY_GUIDE.md` para instrucciones detalladas.

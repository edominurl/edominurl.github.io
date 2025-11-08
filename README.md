# Opcional: Login y Roles Mínimos

Si quieres sumar puntos extra puedes añadir autenticación muy simple:

1. Tabla `usuarios(id, username UNIQUE, password_hash, role TEXT)`.
2. Script `seed.py` que inserte un `admin` y un `user` con contraseñas hasheadas.
3. Rutas `/login` (GET/POST) y `/logout` usando `session`.
4. Decoradores `login_requerido` y `admin_requerido`.
5. En plantillas: mostrar botones editar/borrar solo si `session.role == 'admin'`.

Snippet rápido (pegar debajo de tu CRUD):

```python
from werkzeug.security import check_password_hash
from functools import wraps

def login_requerido(f):
	@wraps(f)
	def w(*a, **k):
		if 'usuario' not in session:
			flash('Inicia sesión','warning')
			return redirect(url_for('login'))
		return f(*a, **k)
	return w

def admin_requerido(f):
	@wraps(f)
	def w(*a, **k):
		if session.get('role') != 'admin':
			flash('Solo admin','danger')
			return redirect(url_for('index'))
		return f(*a, **k)
	return w
```

Recuerda: usa `generate_password_hash()` al crear usuarios, no guardes contraseñas en texto plano.

# edominurl.github.io

Sitio estático del curso "Programación 1 - Python + Flask" (Universidad Rafael Landívar).

## Estructura de semanas

- Guías de aprendizaje (`guia-aprendizaje-semana-N.html`): Material autónomo (máx ~10h) con objetivos, contenido y cronograma.
- Sesiones de clase (`sesion-clase-semana-N.html`): Plan detallado de la sesión sincrónica (actividades, tiempos, ejercicios).

## Semana 3 (nueva)

Incluye:
- Formularios HTML con validación (cliente y servidor)
- Operaciones UPDATE y DELETE con SQLite
- Uso de Bootstrap para interfaz responsive
- Dashboard básico con Chart.js
- Mensajes flash y accesibilidad

## Render de ejemplos Flask

Los bloques de código usan `<pre><code class="language-python">` (Prism.js). El HTML dentro de cadenas Python está escapado con `&lt;` y `&gt;` para no romper la página.

## Ejecución rápida (local)

```bash
python -m venv .venv
source .venv/Scripts/activate  # En Git Bash / PowerShell: .venv\Scripts\activate
pip install flask
python app.py
```

Abrir: http://127.0.0.1:5000

## Anexo: Publicación (GitHub Pages) y despliegue Flask (Render)

### GitHub Pages (este sitio)

- Este repositorio es del tipo `usuario.github.io` y se publica automáticamente desde la rama `main`.
- Para actualizar el sitio, realiza un commit y push de los archivos `.html` (por ejemplo, `guia-aprendizaje-semana-3.html`).
- Asegúrate de que `index.html` contenga enlaces a las nuevas semanas.

### Despliegue de la app Flask en Render

1) Prepara tu proyecto Flask en un repositorio separado (ej. `flask-escuela`).

Archivos mínimos:

```bash
requirements.txt  # incluye: flask, gunicorn, sqlite3 (si aplica), jinja2
Procfile          # contenido: web: gunicorn app:app
app.py            # tu aplicación Flask (variable app = Flask(__name__))
```

2) Recomendado: archivo `requirements.txt` de ejemplo

```txt
flask==3.0.0
gunicorn==21.2.0
```

3) Sube el repo a GitHub y en https://render.com crea un "Web Service":

- Connect a repository → selecciona tu repo `flask-escuela`.
- Runtime: Python.
- Build Command: `pip install -r requirements.txt`
- Start Command: `gunicorn app:app`
- Region cercana (USA/LatAm) y plan gratuito si aplica.

4) Variables de entorno (opcional):

- `PYTHON_VERSION=3.11`
- Cualquier secreto (tokens) como Environment Variables.

5) Migraciones/DB:

- Para SQLite: el archivo `.db` se crea en el disco del contenedor. Para producción considera una base gestionada (PostgreSQL).

## Próximos pasos

- Agregar guía y sesión de Semana 4
- Ajustar navegación en `index.html` para nuevas semanas
- Añadir ejemplos de mensajes flash y modales accesibles

## Licencia

Contenido educativo para uso académico. Código de ejemplo bajo MIT salvo que se indique lo contrario.
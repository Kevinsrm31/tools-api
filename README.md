# Tools API — API REST con FastAPI

API REST construida con FastAPI que expone operaciones CRUD (crear, leer,
actualizar y borrar) sobre un catálogo de herramientas informáticas
clasificadas por categoría: lenguajes de datos, cloud, bases de datos,
desarrollo, visualización, etc.

**🚀 Demo en vivo:** https://tools-api-2quh.onrender.com/docs — desplegada en Render (plan gratuito: si estuvo inactiva, la primera carga tarda ~30–50 s).

## Caso de uso

La API es la base de un sistema de recomendación de herramientas según el
perfil del usuario (desarrollador, ingeniero de datos, analista de
visualización, etc.). Permite consultar el catálogo completo o filtrado por
categoría, y mantenerlo actualizado agregando, editando o eliminando
herramientas.

## Endpoints

| Método | Ruta | Descripción | Respuesta |
|---|---|---|---|
| `GET` | `/api/tools/get_all` | Lista todas las herramientas. Acepta el query param opcional `category` para filtrar | `200` |
| `GET` | `/api/tools/{tool_id}` | Obtiene una herramienta por su `id` | `200` / `404` |
| `POST` | `/api/tools` | Crea una herramienta (si no se envía `id`, se genera como `nombre-categoria`) | `201` |
| `PUT` | `/api/tools/{tool_id}` | Actualiza una herramienta existente | `200` / `404` |
| `DELETE` | `/api/tools/{tool_id}` | Elimina una herramienta | `200` / `404` |

La documentación interactiva (Swagger / OpenAPI) se genera automáticamente
en `/docs`. La ruta raíz `/` redirige ahí.

### Modelo de datos (request body)

```json
{
  "id": "python-data",
  "name": "Python",
  "category": "Data"
}
```

`id` es opcional; `name` y `category` son obligatorios (validación con
Pydantic).

### Ejemplos

```bash
# todas las herramientas de la categoría Data
curl "http://127.0.0.1:8000/api/tools/get_all?category=Data"

# crear una herramienta
curl -X POST "http://127.0.0.1:8000/api/tools" \
     -H "Content-Type: application/json" \
     -d '{"name": "Airflow", "category": "Data"}'
```

## Estructura del repositorio

```
├── README.md
├── LICENSE
├── requirements.txt
├── main.py          # aplicación FastAPI: modelo Pydantic y endpoints CRUD
└── constants.py     # datos semilla del catálogo de herramientas
```

## Cómo ejecutarlo

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate   |   Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt

uvicorn main:app --reload
```

Luego abrir http://127.0.0.1:8000/docs.

## Despliegue

Desplegada en Render como Web Service (Python):

- Build command: `pip install -r requirements.txt`
- Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`

Cada push a `main` dispara un nuevo despliegue automático.

## Alcance actual

Los datos viven en memoria (se cargan desde `constants.py` al iniciar), así
que los cambios hechos con `POST`, `PUT` y `DELETE` se pierden al reiniciar
el servidor. Es una decisión deliberada para centrarse en el diseño de la
API; el siguiente paso natural es conectarla a una base de datos.

## Tecnologías

Python 3, FastAPI, Pydantic, Uvicorn.

## Autor

Kevin Reyes — [LinkedIn](https://www.linkedin.com/in/kevin-steven-reyes-morocho-/) · [Medium](https://medium.com/@kevinsrm19)

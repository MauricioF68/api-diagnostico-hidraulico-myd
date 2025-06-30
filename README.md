API de Diagnóstico de Piezas Hidráulicas
Backend de la API para el sistema de diagnóstico de piezas hidráulicas. Desarrollado con Python y FastAPI, utiliza un modelo de visión por computador (TensorFlow/Keras) para identificar el tipo de pieza y diagnosticar su estado (desgaste, corrosión, ruptura). Se integra con Firebase (Firestore y Storage) para la gestión y persistencia de reportes.

Funcionalidades Principales
Diagnóstico Multi-ángulo: Recibe 5 imágenes de una pieza y devuelve un diagnóstico agregado, aplicando la regla del "peor escenario".

Panel de Confianza: Calcula y devuelve la confianza del modelo para cada posible estado de la pieza.

Persistencia en la Nube: Guarda automáticamente cada reporte de diagnóstico en una base de datos Firestore.

Almacenamiento de Evidencia: Sube las 5 imágenes de cada diagnóstico a Firebase Storage y guarda sus URLs.

Gestión de Reportes: Ofrece endpoints para listar todos los reportes y eliminar reportes específicos (borrando también sus imágenes asociadas).

Exportación a Excel: Genera un reporte profesional en formato .xlsx, incluyendo todos los datos del diagnóstico y las imágenes de evidencia incrustadas.

Stack Tecnológico
Lenguaje: Python 3.12+

Framework API: FastAPI

Servidor ASGI: Uvicorn

Base de Datos: Google Firebase Firestore

Almacenamiento de Archivos: Google Firebase Storage

Inteligencia Artificial: TensorFlow / Keras

Librerías Clave:

firebase-admin: Para la comunicación con los servicios de Firebase.

joblib: Para cargar los codificadores de etiquetas (LabelEncoder).

openpyxl: Para la creación y manipulación de archivos Excel.

Pillow: Para el preprocesamiento de imágenes.

requests: Para descargar las imágenes desde las URLs antes de insertarlas en Excel.

Configuración y Puesta en Marcha Local
Sigue estos pasos para ejecutar el servidor en un entorno de desarrollo local.

1. Prerrequisitos
Tener Python 3.12 o superior instalado.

Tener los 3 artefactos del modelo en la misma carpeta:

modelo_diagnostico_v1.keras

pieza_encoder.joblib

estado_encoder.joblib

2. Configuración del Entorno
# 1. Clona el repositorio (si estás en una nueva máquina)
# git clone https://github.com/tu_usuario/api-diagnostico-hidraulico.git
# cd api-diagnostico-hidraulico

# 2. Crea un entorno virtual
python -m venv env

# 3. Activa el entorno virtual
# En Windows
.\env\Scripts\activate
# En macOS/Linux
# source env/bin/activate

# 4. Instala todas las dependencias
pip install -r requirements.txt

3. Configuración de Firebase
Ve a tu consola de Firebase y descarga tu archivo de credenciales de la cuenta de servicio.

Renómbralo a serviceAccountKey.json.

Colócalo en la raíz de esta carpeta (backend_API). Importante: Este archivo está incluido en el .gitignore y nunca debe ser subido al repositorio.

4. Ejecutar el Servidor
Una vez que todas las dependencias estén instaladas y el archivo de credenciales esté en su lugar, inicia el servidor con el siguiente comando:

# Inicia el servidor en modo de desarrollo, visible en la red local
uvicorn main:app --reload --host 0.0.0.0

El servidor estará disponible en http://<TU_IP_LOCAL>:8000. La documentación interactiva para probar los endpoints estará en http://<TU_IP_LOCAL>:8000/docs.

Endpoints de la API
GET /: Mensaje de bienvenida.

POST /diagnosticar/: Endpoint principal para subir 5 imágenes y recibir un diagnóstico.

GET /reportes/: Devuelve una lista de todos los reportes guardados en Firestore.

DELETE /reportes/{reporte_id}: Elimina un reporte específico y sus imágenes asociadas.

GET /reportes/{reporte_id}/excel: Genera y devuelve un reporte en formato Excel.

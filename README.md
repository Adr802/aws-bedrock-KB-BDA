# Base de Conocimiento AWS Bedrock con Automatización de Datos

Este repositorio contiene herramientas y ejemplos para crear y gestionar Bases de Conocimiento utilizando AWS Bedrock Data Automation (BDA) e integración con S3 Vector Store.

## Descripción General

Este proyecto demuestra cómo:
- Procesar documentos usando AWS Bedrock Data Automation (BDA)
- Crear y gestionar metadatos para documentos en S3
- Integrar con Bases de Conocimiento de AWS Bedrock
- Automatizar pipelines de procesamiento de documentos

## Requisitos Previos

- Cuenta AWS con acceso a servicios Bedrock
- Python 3.x
- Paquetes Python requeridos:
  - boto3
  - python-dotenv

## Configuración del Entorno

Crea un archivo `.env` en el directorio raíz con las siguientes variables:

```env
# Credenciales AWS para BDA
AWS_ACCESS_KEY_ID=tu_access_key
AWS_SECRET_ACCESS_KEY=tu_secret_key
AWS_REGION=tu_region
AWS_ACCOUNT_ID=tu_account_id

# Configuración BDA
BDA_PROFILE_ID=tu_profile_id
BDA_PROJECT_ARN=tu_project_arn

# Configuración de Base de Conocimiento
PERSONAL_KNOWLEDGE_BASE_ID=tu_kb_id
```

## Estructura del Proyecto

- `00_data_automation.ipynb`: Implementación del pipeline BDA para procesamiento de documentos
- `01_s3.ipynb`: Gestión de buckets S3 y creación de metadatos

## Características

### Pipeline de Procesamiento de Documentos (00_data_automation.ipynb)
- Crear sesiones y clientes boto3 para servicios AWS
- Invocar BDA para procesamiento individual de documentos
- Procesar múltiples documentos por lotes
- Monitorear el estado del procesamiento
- Manejar operaciones síncronas y asíncronas

### Gestión de S3 (01_s3.ipynb)
- Listar y gestionar buckets S3
- Crear y subir archivos de metadatos
- Integración con servicios Bedrock
- Operaciones con vector store

## Uso

### 1. Procesamiento de Documentos

```python
# Ejemplo de procesamiento de un solo documento
INPUT_S3_URI = "s3://tu-bucket/ruta/al/documento.pdf"
OUTPUT_S3_URI = "s3://tu-bucket/ruta-salida/"

response = bedrock_bda_runtime_client.invoke_data_automation_async(
    inputConfiguration={'s3Uri': INPUT_S3_URI},
    outputConfiguration={'s3Uri': OUTPUT_S3_URI},
    dataAutomationConfiguration={
        'dataAutomationProjectArn': BDA_PROJECT_ARN,
        'stage': 'LIVE'
    },
    dataAutomationProfileArn=DATA_AUTOMATION_PROFILE_ARN
)
```

### 2. Procesamiento por Lotes

```python
# Procesar todos los documentos PDF en una ruta específica de S3
INPUT_BUCKET = "tu-bucket"
INPUT_PREFIX = "tu/prefijo/"
OUTPUT_S3_URI = f"s3://{INPUT_BUCKET}/bda-output/"

all_arns = invoke_bda_pipeline(INPUT_BUCKET, INPUT_PREFIX, OUTPUT_S3_URI)
```
## Referencias
- [Understanding Amazon Bedrock Data Automation](https://github.com/aws-samples/sample-document-processing-with-amazon-bedrock-data-automation/tree/main/10-Understanding-BDA)
- [Limpieza de indices y s3 vectors buckets](https://github.com/basaravia/s3vectors-2025ago/blob/main/notebooks/purga_s3vectors_aws.ipynb)
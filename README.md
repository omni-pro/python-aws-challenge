# 🧪 Desafío Técnico – Desarrollador Python + AWS (Serverless Email Marketing)

## 🎯 Objetivo

Construir un sistema **serverless** en AWS que permita:

1. **Recibir** un archivo CSV con las columnas `email, subject, content`.
2. **Procesar y encolar** cada envío de email.
3. **Enviar los emails** (simulado o real vía SES) y **guardar el estado**:  
   `PENDING | SENT | ERROR`.
4. Exponer una **query GraphQL** para **listar estados de envío** filtrando por:
   - Estado
   - Rango de fechas (desde / hasta)

---

## 🧩 Requerimientos funcionales

- **Ingreso de datos**
  - Endpoint que reciba un **CSV** (`multipart/form-data`)  
    o bien un **pre-signed URL** (upload a S3 + trigger de procesamiento).
  - Tamaño máximo: difinir y documentar límites.
  - Debe devolver un resumen: `batchId`, `total`, `enqueued`, `skipped`.

- **Procesamiento**
  - Parsear el CSV y publicar una tarea por email en una cola.
  - Un worker consumirá los mensajes y enviará los emails.
  - Persistir los estados de cada envío.

- **Consulta (GraphQL)**
  - Endpoint `/graphql` con una query para listar por:
    - `status` (uno o varios)
    - `from` / `to` (rango de fechas)

- **Librerías obligatorias**
  - [FastAPI](https://fastapi.tiangolo.com/)
  - [GraphQL](https://strawberry.rocks/) (Strawberry o Graphene)

---

## 🧱 Requerimientos no funcionales

- **Infraestructura Serverless en AWS**, con **diagrama** del diseño.
- **Idempotencia**: evitar duplicados si el mismo CSV se procesa dos veces.
- **Logs y métricas**: logs/métricas básicos, indicar herramienta y mecanismos.
- **Errores y reintentos**: uso de DLQ y control de fallas.
- **Seguridad**: IAM roles mínimos, Secrets Manager, sin claves embebidas.
- **Costo optimizado**: servicios serverless y bajo consumo en idle.

---

## 🚀 Endpoints mínimos

### `POST /upload`

Recibe un archivo CSV (`multipart/form-data`) o pre-signed URL.  
Devuelve un `batchId` y resumen del proceso.

### `/graphql`

Query (ejemplo):

```graphql
  listEmailStatus(
    //...filtros
  ){
    //...atributos
  }
```

---

## 📄 Ejemplo de CSV

```csv
email,subject,content
alice@example.com,Welcome,Alice welcome to our platform!
bob@example.com,Promo,Get 20% off this week.
```

---

## 📦 Entregables

1. **Infraestructura como código (IaC):** Terraform, AWS SAM o CDK.
2. **Código en Python (FastAPI + GraphQL)** en un repositorio Git.
3. **Diagrama de arquitectura** (Mermaid o imagen).
4. **README.md** con instrucciones de despliegue y prueba.
5. **Evidencias de ejecución:** logs o capturas del sistema funcionando.

---

## 🧮 Criterios de evaluación

| Criterio | Peso | Descripción |
|----------|------|-------------|
| **Diseño y Arquitectura** | 30% | Uso adecuado de AWS serverless, escalabilidad, costo, buenas prácticas. |
| **Calidad de Código** | 30% | Claridad, modularidad, validaciones, manejo de errores, tests. |
| **Infraestructura y Automatización** | 20% | IaC funcional, scripts de despliegue, CI/CD. |
| **API & DX** | 10% | FastAPI docs, GraphQL usable, documentación. |
| **Observabilidad & Operación** | 10% | Logs, métricas, alarmas básicas, DLQ. |

---

## 💡 Puntos adicionales valorados

- CI/CD con GitHub Actions o similar.  
- “Dry-run mode” (simular envío sin usar SES real).  
- Métricas personalizadas (envíos/min, error rate).  
- `campaignId` o `batchId` para agrupar envíos.  
- Estimación de costos mensuales en README.  
- Tracing con AWS X-Ray.  
- Formateo automático (`black`, `isort`, `ruff`, `mypy`).  
- Multi-stage: `dev` / `prod`.

---

## ⚠️ Aclaraciones

- Si SES está en sandbox, se puede usar un **servicio mock** o emails verificados.
- Se aceptan variantes del diseño (una o dos Lambdas) si se justifica.

---

## 📨 Entrega final

Enviar:
- Link al **repositorio Git** con código e instrucciones.  
- Export de **Postman / Insomnia o similar** o scripts `curl` de prueba.  
- (Opcional) **URL pública** de la API desplegada en AWS.

---

💬 *Tip:* Documentar claramente **las decisiones técnicas y los trade-offs**.  
No se evalúa solo que funcione, sino **cómo se piensa y diseña**.

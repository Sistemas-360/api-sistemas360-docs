# API Sistemas 360

Documentación pública de API Sistemas 360 para integrar facturación electrónica ARCA desde sistemas externos.

API Sistemas 360 es un servicio tecnológico en la nube orientado a empresas, comercios, partners, plataformas SaaS e integradores que necesitan emitir, consultar y administrar comprobantes electrónicos desde sus propios sistemas.

## Descripción

La API permite conectar ERPs, CRMs, POS, ecommerce, plataformas SaaS o desarrollos propios con una capa REST JSON preparada para operar comprobantes electrónicos en Argentina.

El sistema integrador envía los datos comerciales del comprobante y API Sistemas 360 aplica la configuración fiscal del contribuyente vinculado al token utilizado.

## Características principales

* API REST con JSON.
* Autenticación mediante Bearer Token.
* Emisión de Factura A, B y C.
* Emisión de notas de crédito y notas de débito.
* CAE y vencimiento.
* QR fiscal.
* PDF A4 y Ticket.
* Ambiente de pruebas y producción.
* Idempotencia mediante referencia externa.
* Reintentos técnicos.
* Consulta de comprobantes emitidos.
* Respuestas JSON normalizadas.
* Control por token, CUIT emisor, punto de venta y entorno.

## Modalidad del servicio

API Sistemas 360 funciona bajo modalidad SaaS. No requiere instalación local y se utiliza mediante panel web y endpoints de integración.

La configuración fiscal del emisor se administra desde el panel. Cada token queda asociado a un contribuyente, entorno, punto de venta y credenciales técnicas correspondientes.

## Endpoint principal

```http
POST https://api.sistemas360.ar/api/comprobantes
Authorization: Bearer TU_TOKEN
Accept: application/json
Content-Type: application/json
```

## Ejemplo de request

```json
{
  "tipo_comprobante": "factura_b",
  "concepto": "productos",
  "fecha": "2024-01-15",
  "referencia_externa": "venta_demo_1001",
  "cliente": {
    "documento_tipo": "dni",
    "documento_numero": "30111222",
    "razon_social": "Cliente Demo",
    "condicion_iva_receptor_id": 5
  },
  "items": [
    {
      "descripcion": "Producto de ejemplo",
      "cantidad": 1,
      "precio_unitario": 10000,
      "tipo_impuesto": "gravado",
      "iva": 21
    }
  ],
  "total": 12100,
  "moneda": "PES"
}
```

## Ejemplo de respuesta

```json
{
  "ok": true,
  "mensaje": "Comprobante autorizado correctamente.",
  "data": {
    "id": 59,
    "estado": "autorizado",
    "tipo_comprobante": "factura_b",
    "punto_venta": 1,
    "numero_comprobante": 71,
    "cae": "74235618901234",
    "cae_vencimiento": "2024-01-25",
    "qr": {
      "url": "https://www.afip.gob.ar/fe/qr/?p=..."
    },
    "imprimir_a4_url": "https://api.sistemas360.ar/api/comprobantes/59/imprimir-a4",
    "imprimir_ticket_url": "https://api.sistemas360.ar/api/comprobantes/59/imprimir-ticket"
  }
}
```

## Endpoints documentados

| Método | Endpoint                                 | Uso                                             |
| ------ | ---------------------------------------- | ----------------------------------------------- |
| GET    | `/api/ping`                              | Validar token, contribuyente y entorno          |
| POST   | `/api/comprobantes`                      | Crear factura, nota de crédito o nota de débito |
| GET    | `/api/comprobantes`                      | Listar comprobantes                             |
| GET    | `/api/comprobantes/{id}`                 | Consultar un comprobante                        |
| POST   | `/api/comprobantes/{id}/reintentar`      | Reintentar un comprobante con error técnico     |
| GET    | `/api/comprobantes/{id}/imprimir-a4`     | Descargar PDF A4                                |
| GET    | `/api/comprobantes/{id}/imprimir-ticket` | Descargar PDF Ticket                            |

## Flujo recomendado

1. Validar token y entorno con `/api/ping`.
2. Enviar el comprobante a `/api/comprobantes`.
3. Guardar `id`, `estado`, `cae`, `cae_vencimiento` y `referencia_externa`.
4. Consultar el comprobante cuando sea necesario.
5. Descargar PDF A4 o Ticket desde las URLs devueltas.

## Idempotencia

El campo `referencia_externa` identifica una operación única del sistema integrador.

Si se envía nuevamente la misma referencia para la misma empresa, la API devuelve el comprobante ya registrado y evita duplicados.

## Reintentos técnicos

Cuando un comprobante queda en estado `error` por una falla técnica, timeout, error de conexión o indisponibilidad temporal de un servicio externo, puede reintentarse usando:

```http
POST /api/comprobantes/{id}/reintentar
```

El reintento no recibe body. La API utiliza la solicitud original guardada internamente.

## Seguridad y trazabilidad

API Sistemas 360 implementa mecanismos de seguridad orientados a proteger la operación técnica de la plataforma, incluyendo conexión segura HTTPS/TLS, autenticación por token, validación de credenciales, registros técnicos de actividad y controles de acceso.

Los tokens se generan como valores únicos para cada integración, entorno y contribuyente asociado. La plataforma mantiene trazabilidad operativa sobre solicitudes, respuestas, estados, errores y consumos para soporte técnico, auditoría y control de uso.

El usuario integrador debe proteger sus accesos, tokens y credenciales, evitando compartirlos con personas o sistemas no autorizados.

## Alcance del servicio

API Sistemas 360 es una herramienta tecnológica de integración fiscal. No reemplaza al organismo fiscal ni modifica las obligaciones del contribuyente.

La exactitud de los datos fiscales, alícuotas, importes, conceptos y comprobantes emitidos debe ser revisada por el usuario o por sus asesores contables cuando corresponda.

## Documentación completa

La documentación técnica completa está disponible en:

https://api.sistemas360.ar/documentacion-api

## Sitio oficial

https://api.sistemas360.ar

## Marca

SISTEMAS 360 es marca registrada ante el Instituto Nacional de la Propiedad Industrial de Argentina.

## Contacto

[soporte@sistemas360.ar](mailto:soporte@sistemas360.ar)

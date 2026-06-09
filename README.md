# API Sistemas 360

Documentación pública de API Sistemas 360 para integrar facturación electrónica ARCA desde sistemas externos.

La API permite emitir, consultar, reintentar y descargar comprobantes electrónicos desde ERPs, CRMs, POS, ecommerce, plataformas SaaS o desarrollos propios.

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
    "estado": "autorizado",
    "tipo_comprobante": "factura_b",
    "punto_venta": 1,
    "numero_comprobante": 71,
    "cae": "74235618901234",
    "cae_vencimiento": "2024-01-25",
    "imprimir_a4_url": "https://api.sistemas360.ar/api/comprobantes/59/imprimir-a4",
    "imprimir_ticket_url": "https://api.sistemas360.ar/api/comprobantes/59/imprimir-ticket"
  }
}
```

## Endpoints públicos documentados

| Método | Endpoint                                 | Uso                                             |
| ------ | ---------------------------------------- | ----------------------------------------------- |
| GET    | `/api/ping`                              | Validar token, contribuyente y entorno          |
| POST   | `/api/comprobantes`                      | Crear factura, nota de crédito o nota de débito |
| GET    | `/api/comprobantes`                      | Listar comprobantes                             |
| GET    | `/api/comprobantes/{id}`                 | Consultar un comprobante                        |
| POST   | `/api/comprobantes/{id}/reintentar`      | Reintentar un comprobante con error técnico     |
| GET    | `/api/comprobantes/{id}/imprimir-a4`     | Descargar PDF A4                                |
| GET    | `/api/comprobantes/{id}/imprimir-ticket` | Descargar PDF Ticket                            |

## Documentación completa

La documentación técnica completa está disponible en:

https://api.sistemas360.ar/documentacion-api

## Sitio oficial

https://api.sistemas360.ar

## Seguridad

Este repositorio contiene únicamente documentación pública y ejemplos de integración.

No contiene:

* Código fuente del backend.
* Certificados ARCA.
* Claves privadas.
* Tokens reales.
* Variables de entorno.
* Datos de clientes.
* Lógica interna de emisión.

## Contacto

[soporte@sistemas360.ar](mailto:soporte@sistemas360.ar)

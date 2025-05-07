
# 🛍️ Smartfy Retail API

Bienvenido a la documentación oficial de la **Smartfy Retail API**. Esta API permite a nuestros clientes integrarse fácilmente con nuestra plataforma de retail digital para realizar operaciones en tiempo real de forma segura y eficiente.

## 🌐 Entornos disponibles

| Entorno     | URL base                                       | Descripción                         |
|-------------|------------------------------------------------|-------------------------------------|
| `dev`       | https://dev-api-retail.smartfy.tech/           | Entorno de desarrollo y pruebas     |
| `pre`       | https://pre-api-retail.smartfy.tech/           | Entorno de pre-producción (QA)      |
| `prod`      | https://api-retail.smartfy.tech/               | Entorno de producción (live)        |

> ⚠️ **Nota**: Los datos en los entornos `dev` y `pre` pueden ser reiniciados en cualquier momento. No utilices datos reales en estos entornos.
> 
> 🔒 Importante: Para acceder a cualquiera de los entornos (dev, pre) es obligatorio estar conectado a la VPN proporcionada por Smartfy.

---

## 🔐 Autenticación

Antes de consumir cualquier endpoint, es obligatorio iniciar sesión para obtener el token JWT.  
Para ello, debes hacer una petición a:

```http
POST https://dev-api-retail.smartfy.tech/api/v1/users/login_check
```

### Cuerpo de la petición (`Content-Type: application/json`)
```json
{
  "username": "tu_usuario",
  "password": "tu_contraseña"
}
```

### Respuesta esperada
```json
{
  "token": "eyJ0eXAiOiJKV1QiLCJh..."
}
```

### Usar el token
Todas las peticiones posteriores deben incluir este token en la cabecera:

```http
Authorization: Bearer <tu_token>
```

---

## 📌 Endpoints principales

A continuación se detallan algunos de los endpoints básicos disponibles tras autenticarte correctamente:

### 1. Crear orden de mercado

```http
POST https://dev-api-retail.smartfy.tech/api/v1/market-orders
```

**Descripción**: Crea una nueva orden de mercado con los productos seleccionados.

---

## 🛠️ Cómo empezar

1. Solicita tus credenciales de acceso (usuario y contraseña).
2. Realiza login en `/api/v1/users/login_check` y obtén el token.
3. Usa el token en la cabecera `Authorization`.
4. Escoge el entorno adecuado (`dev`, `pre` o `prod`).
5. Consulta los endpoints y empieza a integrar.

---

## 📤 Ejemplo de payload para orden de mercado

```json
{
  "surname1": "Pérez",
  "surname2": "García",
  "email": "mi@email.com",
  "phone1": "619881597",    
  "document": "79367193Z", 
  "documentType": "NIE",
  "birthDate": "1995-08-17",
  "addressType": "avda",
  "address": "Avda. De La Ciudead De Barcelona 97",
  "addressExtended": "nº 97, 4A",
  "streetName": "Avda. De La Ciudad De Barcelona",
  "name": "Juan",
  "number": "9",
  "stairs": null,
  "floor": "2ºb",
  "door": "B",
  "postalCode": "28020",
  "town": "Madrid",
  "birthCountry": "ES",
  "province": "28",
  "country": "ES",
  "companyName": "visalia",
  "orderExpiryDate": "2025-06-30 14:43:28",
  "totalAmount": 577.69,
  "totalAmountIncTax": 699.00,
  "logisticCost": 0,
  "marketOrderCode": "{{$guid}}",
  > ℹ️ Los datos necesarios para `marketOrderItem` (como `skuRetailer`, `unitPrice`, `duration`) se obtienen a través del SDK proporcionado por Smartfy.

  "marketOrderItem": [
    {
      "skuRetailer": "328950-XXX-XXX",
      "unitPrice": 577.69,
      "duration": 24
    }
  ]
}
```

Este es un ejemplo completo del JSON que se debe enviar al endpoint `/api/v1/market-orders`.

---

## 📥 Ejemplo de respuesta de una orden de mercado exitosa

```json
{
  "id": 9828,
  "requestCode": "cb834f46-ddcf-41e0-9204-c6e0d3907c53",
  "marketOrderCode": "1bf1189d-88f0-477e-9b4d-81ca97f1ccc8",
  "totalAmount": 577.69,
  "totalAmountIncTax": 699,
  "orderExpiryDate": "2025-06-30T14:43:28+02:00",
  "createdAt": "2025-05-07T13:43:36+02:00",
  "updatedAt": "2025-05-07T13:43:36+02:00",
  "marketOrderItem": [
    {
      "id": 9783,
      "unitPrice": 577.69,
      "unitPriceIncTax": 699.0049,
      "duration": 24,
      "marketProduct": {
        "skuRetailer": "328950-XXX-XXX",
        "ean": "0194252014134",
        "versionDescription": "Apple iPhone 12 Mini 64GB Azul Libre",
        "features": null,
        "unitPriceIncTax": 699,
        "unitPrice": 577.69
      }
    }
  ],
  "identificationSystem": null,
  "userExists": true
}
```

Esta es una respuesta típica al crear una orden de mercado correctamente. Incluye el identificador de la orden, los productos asociados y detalles como precio y fechas.



---

## 🚀 Inicio del proceso de onboarding

Una vez creada la orden de mercado y obtenida una respuesta exitosa, se usará el `requestCode` proporcionado para iniciar el proceso de onboarding del cliente en la plataforma Smartfy.

### 🔗 Redirección según entorno

Debes redirigir al cliente a una de las siguientes URLs, incluyendo el `requestCode` como parámetro:

| Entorno     | URL completa con `requestCode` de ejemplo |
|-------------|-------------------------------------------|
| `dev`       | https://dev-onboarding-retail.smartfy.tech/?requestCode=cb834f46-ddcf-41e0-9204-c6e0d3907c53 |
| `pre`       | https://pre-onboarding-retail.smartfy.tech/?requestCode=cb834f46-ddcf-41e0-9204-c6e0d3907c53 |
| `prod`      | https://onboarding-retail.smartfy.tech/?requestCode=cb834f46-ddcf-41e0-9204-c6e0d3907c53 |

Esto iniciará el flujo de onboarding personalizado para el usuario correspondiente a la orden generada.


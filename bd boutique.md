Claro, te doy un ejemplo claro y práctico pensado para una **base de datos NoSQL (tipo MongoDB)**, donde trabajas con **colecciones** y **documentos** en lugar de tablas.

Imaginemos una **boutique** 👗

---

# 🧵 Base de datos: `boutique_db`

## 📁 Colección: `productos`

Cada documento representa un producto de la boutique.

### 📄 Ejemplo de documento:

```json
{
  "_id": ObjectId,
  "nombre": "Vestido floral",
  "descripcion": "Vestido largo con estampado floral",
  "precio": 599.99,
  "stock": 25,
  "talla": ["S", "M", "L"],
  "color": "Rojo",
  "categoria_id": ObjectId,
  "proveedor_id": ObjectId,
  "fecha_ingreso": ISODate
}
```

### 🧾 Atributos y tipos:

* `_id` → ObjectId
* `nombre` → String
* `descripcion` → String
* `precio` → Decimal
* `stock` → Int
* `talla` → Array (String)
* `color` → String
* `categoria_id` → ObjectId (referencia)
* `proveedor_id` → ObjectId (referencia)
* `fecha_ingreso` → Date

---

## 📁 Colección: `categorias`

### 📄 Ejemplo:

```json
{
  "_id": ObjectId,
  "nombre": "Ropa Dama",
  "descripcion": "Prendas femeninas elegantes",
  "fecha_creacion": ISODate
}
```

### 🧾 Atributos:

* `_id` → ObjectId
* `nombre` → String
* `descripcion` → String
* `fecha_creacion` → Date

---

## 📁 Colección: `proveedores`

### 📄 Ejemplo:

```json
{
  "_id": ObjectId,
  "nombre": "Moda Express",
  "telefono": "6561234567",
  "correo": "contacto@modaexpress.com",
  "direccion": "Ciudad Juárez, Chihuahua"
}
```

### 🧾 Atributos:

* `_id` → ObjectId
* `nombre` → String
* `telefono` → String
* `correo` → String
* `direccion` → String

---

## 📁 Colección: `clientes`

### 📄 Ejemplo:

```json
{
  "_id": ObjectId,
  "nombre": "Ana López",
  "telefono": "6569876543",
  "correo": "ana@gmail.com",
  "direccion": "Col. Centro"
}
```

### 🧾 Atributos:

* `_id` → ObjectId
* `nombre` → String
* `telefono` → String
* `correo` → String
* `direccion` → String

---

## 📁 Colección: `ventas`

### 📄 Ejemplo:

```json
{
  "_id": ObjectId,
  "cliente_id": ObjectId,
  "fecha": ISODate,
  "total": 1199.98,
  "productos": [
    {
      "producto_id": ObjectId,
      "cantidad": 2,
      "precio_unitario": 599.99
    }
  ]
}
```

### 🧾 Atributos:

* `_id` → ObjectId
* `cliente_id` → ObjectId
* `fecha` → Date
* `total` → Decimal
* `productos` → Array (Objetos)

  * `producto_id` → ObjectId
  * `cantidad` → Int
  * `precio_unitario` → Decimal

---

# 💡 Nota importante (muy útil para tu clase)

En NoSQL puedes usar dos enfoques:

* **Referencias** (como arriba, usando `ObjectId`)
* **Embebidos** (guardar datos dentro del mismo documento)

Ejemplo embebido en ventas:

```json
"productos": [
  {
    "nombre": "Vestido floral",
    "cantidad": 2,
    "precio": 599.99
  }
]
```

---

Si quieres, puedo:
✅ Convertirte esto a **SQL (MySQL)**
✅ Hacerte el **diagrama tipo ER**
✅ O darte el **código para MongoDB (insert, createCollection)**

Solo dime 👍

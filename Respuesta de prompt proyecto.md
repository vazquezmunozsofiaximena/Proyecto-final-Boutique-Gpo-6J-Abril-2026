Esta es una guía estructurada para inicializar tu entorno de desarrollo y generar la estructura de la habilidad de agente global utilizando el estándar `.agents`. 

---

## 1. Estructura de la Habilidad Agente Global: `.agents`

Primero, configuramos el entorno de "meta-programación" para que tu agente sepa cómo operar dentro del proyecto.

### Estructura de Carpetas
```text
proyectoboutique/
├── .agents/
│   ├── SKILL.md            # Definición de capacidades
│   ├── scripts/            # Automatizaciones de despliegue/limpieza
│   ├── ejemplos/           # Snippets de UI y Lógica
│   └── resources/          # Activos y configuraciones JSON
```

### SKILL.md (Definición)
> **Agente:** Boutique Manager
> **Skills:** > * **Diseño:** Implementación de UI basada en Material 3.
> * **Código:** Generación de modelos Dart y lógica de negocio.
> * **Scraping:** Automatización para poblar catálogo de productos desde fuentes externas.

---

## 2. Prerrequisitos y Configuración del Entorno

### Verificación de Herramientas
Ejecuta los siguientes comandos en tu terminal para asegurar que el camino está despejado:

1.  **Flutter Check:** `flutter doctor`
2.  **Firebase CLI:** Si no lo tienes, instálalo con `npm install -g firebase-tools`.
3.  **FlutterFire CLI:** `dart pub global activate flutterfire_cli`

### Conectividad Firebase
```bash
# Iniciar sesión
firebase login

# Configurar el proyecto en la carpeta raíz
flutterfire configure
```
*Esto generará automáticamente el archivo `firebase_options.dart` en tu carpeta `lib`.*

---

## 3. Configuración del Proyecto `proyectoboutique`

### `pubspec.yaml` (Dependencias Críticas)
Asegúrate de incluir estas librerías para el CRUD y Firebase:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.x.x
  cloud_firestore: ^4.x.x
  cupertino_icons: ^1.0.2
```

---

## 4. Implementación del CRUD de Productos

Vamos a crear la estructura de archivos `.dart` necesaria para un proyecto funcional y escalable.

### A. Modelo de Datos (`lib/models/product_model.dart`)
```dart
class Product {
  String id;
  String name;
  double price;
  String imageUrl;

  Product({required this.id, required this.name, required this.price, required this.imageUrl});

  Map<String, dynamic> toMap() => {
    'name': name,
    'price': price,
    'imageUrl': imageUrl,
  };

  factory Product.fromFirestore(Map<String, dynamic> data, String id) {
    return Product(
      id: id,
      name: data['name'] ?? '',
      price: (data['price'] ?? 0).toDouble(),
      imageUrl: data['imageUrl'] ?? '',
    );
  }
}
```

### B. Servicio Firestore (`lib/services/firebase_service.dart`)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/product_model.dart';

class FirebaseService {
  final CollectionReference _db = FirebaseFirestore.instance.collection('productos');

  // Create
  Future<void> addProduct(Product product) => _db.add(product.toMap());

  // Read
  Stream<List<Product>> getProducts() {
    return _db.snapshots().map((snapshot) =>
        snapshot.docs.map((doc) => Product.fromFirestore(doc.data() as Map<String, dynamic>, doc.id)).toList());
  }

  // Update
  Future<void> updateProduct(Product product) => _db.doc(product.id).update(product.toMap());

  // Delete
  Future<void> deleteProduct(String id) => _db.doc(id).delete();
}
```

### C. UI: Pantalla de Inicio y CRUD (`lib/screens/home_screen.dart`)
Esta pantalla integra la navegación y la visualización de datos en tiempo real.

```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';
import '../models/product_model.dart';

class HomeScreen extends StatelessWidget {
  final FirebaseService _service = FirebaseService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Boutique CRUD')),
      body: StreamBuilder<List<Product>>(
        stream: _service.getProducts(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          
          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              Product p = snapshot.data![index];
              return ListTile(
                title: Text(p.name),
                subtitle: Text("\$${p.price}"),
                trailing: Row(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    IconButton(icon: Icon(Icons.edit), onPressed: () => _showForm(context, p)),
                    IconButton(icon: Icon(Icons.delete, color: Colors.red), onPressed: () => _service.deleteProduct(p.id)),
                  ],
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _showForm(context, null),
      ),
    );
  }

  void _showForm(BuildContext context, Product? product) {
    // Aquí se implementaría el BottomSheet o Diálogo con controladores de texto para los campos
  }
}
```

---

## 5. Ejecución y Pruebas
1.  **IDE:** Abre la carpeta `proyectoboutique` en VS Code o Antigravity.
2.  **Verificación:** Ejecuta `flutter doctor -v` para confirmar que el simulador o dispositivo físico está detectado.
3.  **Lanzamiento:** Presiona `F5` o ejecuta `flutter run`.

### Flujo de Trabajo con `.agents`
* **Diseño:** Usa la carpeta `.agents/ejemplos` para guardar variaciones de botones o tarjetas de productos.
* **Código:** El script de automatización en `.agents/scripts` puede usarse para generar modelos automáticamente si decides expandir la base de datos (ej. Categorías, Clientes).

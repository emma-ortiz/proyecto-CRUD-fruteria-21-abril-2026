¡Hola! Como desarrollador de software, me encanta este reto. Vamos a construir no solo una app, sino un ecosistema de desarrollo utilizando **Antigravity** (un framework de orquestación de agentes IA) para automatizar y estructurar la creación de nuestro CRUD de Frutería en Flutter.

Aquí tienes la metodología paso a paso y el código funcional.

---

## 1. Fase de Preparación y Metodología Antigravity

Para los estudiantes, **Antigravity** se basa en definir una "fuerza de trabajo" virtual. En lugar de escribir código al azar, definimos roles que colaboran entre sí.

### Estructura de Agentes
| Agente | Rol | Skill (Habilidad) |
| :--- | :--- | :--- |
| **Architect** | Diseñador de Estructura | Definir `pubspec.yaml` y jerarquía de carpetas. |
| **Firebase Specialist** | Integrador Cloud | Configurar `firebase_core` y `cloud_firestore`. |
| **Flutter Developer** | Programador UI/Logic | Escribir lógica CRUD y vistas en Dart. |

### Flujo de Trabajo (Workflow)
1.  **Ignition:** Creación del proyecto y carpetas.
2.  **Config:** Vinculación con Firebase Console.
3.  **Development:** Creación de modelos, servicios y UI.
4.  **Deployment:** Pruebas funcionales del CRUD.

---

## 2. Creación del Proyecto y Estructura

Primero, abre tu terminal y ejecuta:
```bash
flutter create crudfruteria
cd crudfruteria
```

### Estructura de Carpetas Recomendada
```text
lib/
├── models/
│   └── fruta_model.dart
├── services/
│   └── firebase_service.dart
├── screens/
│   ├── home_screen.dart
│   └── edit_screen.dart
└── main.dart
```

---

## 3. Configuración de Firebase (Firestore)

1.  Ve a [Firebase Console](https://console.firebase.google.com/).
2.  Crea un proyecto llamado `Bdcrudfruteria`.
3.  Crea una base de datos **Firestore** en "Modo de prueba".
4.  Crea una colección llamada **`frutas`**.
5.  Usa `flutterfire configure` para vincular tu app (requiere Firebase CLI).

### Dependencias en `pubspec.yaml`
Añade estas líneas bajo `dependencies`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.24.2
  cloud_firestore: ^4.14.0
```
Luego ejecuta: `flutter pub get`.

---

## 4. Implementación del Código Funcional

### A. El Modelo (`models/fruta_model.dart`)
Define qué es una fruta en nuestra app.

```dart
class Fruta {
  String id;
  String nombre;
  String categoria;
  double precio;

  Fruta({required this.id, required this.nombre, required this.categoria, required this.precio});

  // Convertir de Firestore a Objeto
  factory Fruta.fromFirestore(Map<String, dynamic> data, String id) {
    return Fruta(
      id: id,
      nombre: data['nombre'] ?? '',
      categoria: data['categoria'] ?? '',
      precio: (data['precio'] ?? 0).toDouble(),
    );
  }

  // Convertir de Objeto a Firestore
  Map<String, dynamic> toMap() {
    return {
      'nombre': nombre,
      'categoria': categoria,
      'precio': precio,
    };
  }
}
```

### B. El Servicio CRUD (`services/firebase_service.dart`)
Aquí reside la lógica de comunicación con Firebase.

```dart
import 'cloud_firestore/cloud_firestore.dart';
import '../models/fruta_model.dart';

final FirebaseFirestore _db = FirebaseFirestore.instance;

// LEER Frutas
Stream<List<Fruta>> getFrutas() {
  return _db.collection('frutas').snapshots().map((snapshot) =>
      snapshot.docs.map((doc) => Fruta.fromFirestore(doc.data(), doc.id)).toList());
}

// CREAR Fruta
Future<void> addFruta(String nombre, String categoria, double precio) async {
  await _db.collection('frutas').add({
    'nombre': nombre,
    'categoria': categoria,
    'precio': precio,
  });
}

// ACTUALIZAR Fruta
Future<void> updateFruta(String id, String nombre, String categoria, double precio) async {
  await _db.collection('frutas').doc(id).update({
    'nombre': nombre,
    'categoria': categoria,
    'precio': precio,
  });
}

// BORRAR Fruta
Future<void> deleteFruta(String id) async {
  await _db.collection('frutas').doc(id).delete();
}
```

### C. La Interfaz Principal (`screens/home_screen.dart`)
Una pantalla sencilla para listar y borrar.

```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';
import '../models/fruta_model.dart';

class HomeScreen extends StatelessWidget {
  final TextEditingController _nombreCtrl = TextEditingController();
  final TextEditingController _catCtrl = TextEditingController();
  final TextEditingController _precioCtrl = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Frutería CRUD")),
      body: StreamBuilder<List<Fruta>>(
        stream: getFrutas(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          var frutas = snapshot.data!;
          return ListView.builder(
            itemCount: frutas.size,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(frutas[index].nombre),
                subtitle: Text("${frutas[index].categoria} - \$${frutas[index].precio}"),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => deleteFruta(frutas[index].id),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _showForm(context),
      ),
    );
  }

  void _showForm(BuildContext context) {
    showModalBottomSheet(
      context: context,
      builder: (_) => Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(controller: _nombreCtrl, decoration: InputDecoration(labelText: 'Nombre')),
            TextField(controller: _catCtrl, decoration: InputDecoration(labelText: 'Categoría')),
            TextField(controller: _precioCtrl, decoration: InputDecoration(labelText: 'Precio'), keyboardType: TextInputType.number),
            ElevatedButton(
              onPressed: () {
                addFruta(_nombreCtrl.text, _catCtrl.text, double.parse(_precioCtrl.text));
                Navigator.pop(context);
              },
              child: Text("Guardar Fruta")
            )
          ],
        ),
      ),
    );
  }
}
```

### D. Punto de Entrada (`main.dart`)
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'screens/home_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(); // Inicializa Firebase
  runApp(MaterialApp(
    home: HomeScreen(),
    debugShowCheckedModeBanner: false,
  ));
}
```

---

## Guía para Estudiantes (Antigravity Approach)

1.  **Role Play:** Dividan la clase. Un grupo actúa como **Agentes de Datos** (Modelos), otro como **Agentes de Servicio** (Firebase) y otro como **Agentes de UX** (UI).
2.  **Protocolo:** No se puede programar la UI sin que el Agente de Datos haya definido primero el `FrutaModel`.
3.  **Validación:** Cada "Agente" debe probar su parte. Por ejemplo, el Agente Firebase debe confirmar que los datos llegan a la consola antes de pasar el código al Agente de UI.

¿Te gustaría que profundicemos en cómo realizar las validaciones de errores para que la app sea aún más robusta?

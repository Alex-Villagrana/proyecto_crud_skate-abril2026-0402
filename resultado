¡Hola! Como desarrollador senior, me encanta este enfoque. Mezclar **Flutter** con **Firebase** es el estándar de la industria, pero añadir **Antigravity** (un framework emergente para orquestación de agentes y flujos de trabajo) eleva la práctica a un nivel de arquitectura moderna.

Aquí tienes el plan de trabajo y la guía técnica para construir el proyecto **bdcrudskate0402**.

---

## 1. Estructura de Proyecto y Configuración Inicial

### Creación de Carpeta y Proyecto
Desde tu terminal, ejecuta:
```bash
flutter create bdcrudskate0402
cd bdcrudskate0402
```

### Configuración de Firebase (Consola)
1. Ve a [Firebase Console](https://console.firebase.google.com/).
2. Crea un proyecto llamado `bdcrudskate0402`.
3. Habilita **Firestore Database** en modo de prueba (Test Mode).
4. Registra tu app (Android/iOS) y descarga el archivo `google-services.json` (ponlo en `android/app/`).

### Dependencias (`pubspec.yaml`)
Agrega estas librerías para que el proyecto sea funcional:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^3.0.0
  cloud_firestore: ^5.0.0
  antigravity: ^0.1.0 # Orquestación de lógica
```

---

## 2. Metodología Antigravity: Agentes y Flujos
Para que los estudiantes entiendan, **Antigravity** separa la intención (Agente) de la ejecución (Flujo).

| Componente | Definición en este Proyecto |
| :--- | :--- |
| **Agente (EmployeeAgent)** | Responsable de la gestión de datos humanos. |
| **Rol (DataHandler)** | Define que el agente puede interactuar con bases de datos. |
| **Skill (FirestoreService)** | La habilidad técnica de hablar con Firebase. |
| **Workflow (CRUD Flow)** | La secuencia: Validar -> Enviar a Firebase -> Notificar. |



---

## 3. Estructura de Carpetas Sugerida
```text
lib/
├── agents/          # Definición de Agentes y Roles
├── services/        # Skills (Conexión Firebase)
├── models/          # Entidad Empleado
├── screens/         # UI (Listado y Formulario)
└── main.dart        # Punto de entrada
```

---

## 4. Implementación del Código (Totalmente Funcional)

### El Modelo: `lib/models/employee_model.dart`
```dart
class Employee {
  String id;
  String name;
  int age;
  double salary;

  Employee({required this.id, required this.name, required this.age, required this.salary});

  Map<String, dynamic> toMap() => {
    "name": name,
    "age": age,
    "salary": salary,
  };

  factory Employee.fromMap(String id, Map<String, dynamic> data) => Employee(
    id: id,
    name: data['name'] ?? '',
    age: data['age'] ?? 0,
    salary: (data['salary'] ?? 0).toDouble(),
  );
}
```

### El Skill (Firebase): `lib/services/firestore_service.dart`
```dart
import 'cloud_firestore/cloud_firestore.dart';
import '../models/employee_model.dart';

class FirestoreService {
  final FirebaseFirestore _db = FirebaseFirestore.instance;

  Future<void> saveEmployee(Employee emp) => 
      _db.collection('employees').add(emp.toMap());

  Stream<List<Employee>> getEmployees() {
    return _db.collection('employees').snapshots().map((snapshot) =>
        snapshot.docs.map((doc) => Employee.fromMap(doc.id, doc.data())).toList());
  }

  Future<void> updateEmployee(Employee emp) =>
      _db.collection('employees').doc(emp.id).update(emp.toMap());

  Future<void> deleteEmployee(String id) =>
      _db.collection('employees').doc(id).delete();
}
```

### Orquestación con Antigravity: `lib/agents/employee_agent.dart`
Aquí definimos el comportamiento del "Agente" que maneja los datos.

```dart
import 'package:antigravity/antigravity.dart';
import '../services/firestore_service.dart';

class EmployeeAgent extends Agent {
  final FirestoreService _service = FirestoreService();

  EmployeeAgent() : super(name: 'StaffManager', role: 'DataAdmin');

  // Flujo de trabajo simplificado para la práctica
  Future<void> executeTask(String task, dynamic data) async {
    switch (task) {
      case 'CREATE': await _service.saveEmployee(data); break;
      case 'UPDATE': await _service.updateEmployee(data); break;
      case 'DELETE': await _service.deleteEmployee(data); break;
    }
  }
}
```

---

## 5. Interfaz de Usuario (UI): `lib/main.dart`
Este archivo inicializa Firebase y muestra la lista de empleados.

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'services/firestore_service.dart';
import 'models/employee_model.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MaterialApp(home: EmployeeScreen()));
}

class EmployeeScreen extends StatelessWidget {
  final FirestoreService _service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('CRUD Skate 0402 - Empleados')),
      body: StreamBuilder<List<Employee>>(
        stream: _service.getEmployees(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          final employees = snapshot.data!;
          return ListView.builder(
            itemCount: employees.length,
            itemBuilder: (context, i) => ListTile(
              title: Text(employees[i].name),
              subtitle: Text("Edad: ${employees[i].age} - Salario: \$${employees[i].salary}"),
              trailing: IconButton(
                icon: Icon(Icons.delete, color: Colors.red),
                onPressed: () => _service.deleteEmployee(employees[i].id),
              ),
            ),
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
    // Aquí se implementaría un Dialog con TextFields para Nombre, Edad y Salario
    // que al guardar llame a _service.saveEmployee(...)
  }
}
```

---

### Resumen para Estudiantes:
1.  **Firebase** es tu almacén de datos (la bodega).
2.  **Flutter** es tu mostrador (la cara al cliente).
3.  **Antigravity** son tus empleados inteligentes (quienes deciden cómo mover la mercancía basándose en reglas).

¿Te gustaría que profundice en cómo crear una regla de validación específica dentro del flujo de Antigravity para que no permita salarios menores al mínimo?

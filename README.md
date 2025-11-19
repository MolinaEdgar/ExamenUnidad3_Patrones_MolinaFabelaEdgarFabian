# Simulador de Transporte  
**Patrones de Diseño: Decorator y Composite**

Este proyecto implementa un **Simulador de Transporte** empleando dos patrones de diseño fundamentales en la programación orientada a objetos: **Composite** para la gestión de rutas y **Decorator** para la personalización de vehículos. A continuación se presenta una explicación clara, estructurada y detallada de cómo funciona el código y cómo se aplican estos patrones.

---

## 📌 Descripción General del Proyecto

El simulador permite al usuario construir una ruta de transporte compuesta por un tramo base y diversas paradas opcionales.  
Además, el usuario puede elegir un tipo de vehículo base y agregarle funcionalidades como **WiFi** o **Aire Acondicionado**, lo cual modifica tanto su descripción como su costo.

El flujo general del programa es:

1. Crear una ruta base.
2. Mostrar paradas adicionales disponibles.
3. Permitir al usuario agregar las paradas deseadas.
4. Mostrar la ruta completa construida.
5. Permitir seleccionar un vehículo base y decorarlo con servicios adicionales.
6. Mostrar un resumen del viaje con distancia total y costo final.

---

# 🧩 Patrón Composite  
## Gestión de Rutas y Paradas

El patrón **Composite** permite tratar objetos individuales y compuestos de forma uniforme.  
En este proyecto, se implementa en la estructura de rutas:

### ✔ Componentes clave del Composite:

### **1. Interfaz `IRuta`**  
Define operaciones comunes para todos los elementos de una ruta:

- `CalcularDistancia()` → devuelve la distancia del tramo o conjunto de tramos  
- `Mostrar()` → imprime la descripción en pantalla  

Esta interfaz actúa como un contrato para objetos simples y compuestos.

### **2. Clase `Tramo` (objeto simple)**  
Representa una parte indivisible de la ruta:

- Describe un tramo con nombre y distancia.
- Implementa `IRuta`.

Un `Tramo` es un **leaf** (hoja) dentro del patrón Composite.

### **3. Clase `RutaCompuesta` (objeto compuesto)**  
Permite almacenar y manejar múltiples `IRuta` (tanto tramos como otras rutas compuestas):

- Contiene una lista de elementos `IRuta`.
- Permite agregar nuevos tramos o paradas.
- Calcula la distancia total sumando las distancias de todos sus elementos.
- Muestra la ruta completa recorriendo cada elemento.

`RutaCompuesta` es el **composite**, capaz de contener hojas y otros composites.

### 🧠 ¿Por qué Composite es ideal aquí?

- Permite crear rutas dinámicas y modulares.
- El programa no necesita diferenciar entre tramos simples o rutas completas: todos se tratan como `IRuta`.
- Escalable: permite agregar tramos, paradas o incluso subrutas sin modificar el código principal.

---

# 🎨 Patrón Decorator  
## Personalización del Vehículo

El patrón **Decorator** permite agregar funcionalidades adicionales a un objeto sin modificar su estructura interna.  
Aquí se usa para agregar características a los vehículos, como:

- WiFi  
- Aire Acondicionado  
- (se pueden agregar más decoradores fácilmente)

### ✔ Componentes clave del Decorator:

### **1. Interfaz `IVehiculo`**
Define dos propiedades principales que todos los vehículos deben tener:

- `Descripcion`  
- `Costo`

Permite que tanto el vehículo base como los decoradores sean tratados por igual.

### **2. Vehículo base (`AutobusBasico`)**
Es el componente inicial sobre el cual se aplicarán los decoradores.  
Incluye:

- Descripción simple del autobús.  
- Costo base del servicio.  

Este es el **componente concreto** del patrón Decorator.

### **3. Decoradores (`WiFi`, `AireAcondicionado`, etc.)**
Cada decorador:

- Recibe un objeto `IVehiculo`.
- Agrega su propia descripción.
- Incrementa el costo total del viaje.

El vehículo decorado puede seguir siendo decorado múltiples veces.  
Por ejemplo:

Esto produce un vehículo con ambas funcionalidades.

### 🧠 ¿Por qué Decorator es ideal aquí?

- Evita crear múltiples clases como `AutobusConWiFiYAire`, `AutobusConWiFi`, etc.
- Permite extender funcionalidades sin modificar código existente.
- Flexible: permite que el usuario combine decoradores en cualquier orden.

---

# 🚀 Ventajas Generales de la Arquitectura

### ✔ Extensibilidad  
Puedes agregar más decoradores (GPS, entretenimiento, cargadores USB) sin tocar el código ya existente.

### ✔ Reutilización  
Los componentes simples (`Tramo`, `AutobusBasico`) funcionan independientemente del sistema completo.

### ✔ Escalabilidad  
La estructura compuesta de rutas permite agregar:

- Nuevos tipos de tramos  
- Paradas avanzadas  
- Rutas jerárquicas (macro rutas)

### ✔ Mantenimiento sencillo  
Tanto Composite como Decorator reducen el acoplamiento y facilitan agregar nuevas funcionalidades sin romper lo existente.

---

# 📚 Conclusión

Este simulador demuestra cómo los patrones de diseño **Composite** y **Decorator** pueden integrarse de manera elegante para resolver dos problemas comunes:

- Construir estructuras jerárquicas flexibles (rutas)  
- Agregar funcionalidades opcionales a objetos existentes (vehículos)  

Ambos patrones permiten crear un sistema robusto, extensible y fácil de mantener, ideal para simulaciones, sistemas de transporte, videojuegos, arquitecturas modulares y mucho más.

---

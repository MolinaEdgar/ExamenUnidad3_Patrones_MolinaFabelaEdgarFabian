SimuladorTransporte/
│  
├── Program.cs  
│  
├── Rutas/  
│   ├── IRuta.cs  
│   ├── Tramo.cs  
│   ├── Parada.cs  
│   └── RutaCompuesta.cs  
│  
├── Vehiculos/  
│   ├── IVehiculo.cs  
│   ├── AutobusBasico.cs  
│   ├── VehiculoDecorator.cs 
│   ├── AireAcondicionado.cs  
│   └── WiFi.cs  

Program.cs
```
using System;
using System.Collections.Generic;

namespace SimuladorTransporte
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("=== SIMULADOR DE TRANSPORTE ===\n");

            string nombreRuta = "Ruta Urbana";
            double distanciaBase = 10;

            RutaCompuesta ruta = new RutaCompuesta(nombreRuta);
            ruta.Agregar(new Tramo(nombreRuta, distanciaBase));

            List<Parada> paradasDisponibles = new List<Parada>
            {
                new Parada("Estación Norte", 0.7),
                new Parada("Estación Central", 1.5),
                new Parada("Estación Sur", 1.0)
            };

            Console.WriteLine($"Ruta disponible: {nombreRuta} ({distanciaBase} km)\n");

            Console.WriteLine("Seleccione las paradas a agregar. Ejemplo (1,2,3). Enter si no desea agregar:");
            for (int i = 0; i < paradasDisponibles.Count; i++)
                Console.WriteLine($"{i + 1}. {paradasDisponibles[i].Nombre} (+{paradasDisponibles[i].Distancia} km)");

            string input = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(input))
            {
                string[] seleccion = input.Split(',');
                foreach (var s in seleccion)
                {
                    if (int.TryParse(s.Trim(), out int index) &&
                        index >= 1 && index <= paradasDisponibles.Count)
                    {
                        ruta.Agregar(paradasDisponibles[index - 1]);
                        Console.WriteLine($"+ Parada agregada: {paradasDisponibles[index - 1].Nombre}");
                    }
                }
            }

            ruta.Mostrar();
            Console.WriteLine($"\nDistancia total de la ruta: {ruta.CalcularDistancia()} km");

            IVehiculo vehiculo = new AutobusBasico();
            Console.WriteLine("\nAgregar WiFi? (s/n)");
            if (Console.ReadLine().ToLower() == "s")
                vehiculo = new WiFi(vehiculo);

            Console.WriteLine("Agregar Aire Acondicionado? (s/n)");
            if (Console.ReadLine().ToLower() == "s")
                vehiculo = new AireAcondicionado(vehiculo);

            Console.WriteLine("\n=== RESUMEN DEL VIAJE ===");
            Console.WriteLine($"Ruta: {ruta.CalcularDistancia()} km");
            Console.WriteLine($"Vehículo: {vehiculo.Descripcion}");
            Console.WriteLine($"Costo total por pasajero: ${vehiculo.Costo}");

            Console.WriteLine("\nPresione cualquier tecla para salir...");
            Console.ReadKey();
        }
    }
}

```

### CARPETA Rutas
IRuta.cs
```
namespace SimuladorTransporte
{
    public interface IRuta
    {
        double CalcularDistancia();
        void Mostrar();
    }
}

```
Tramo.cs
```
using System;

namespace SimuladorTransporte
{
    public class Tramo : IRuta
    {
        public string Nombre { get; }
        public double Distancia { get; }

        public Tramo(string nombre, double distancia)
        {
            Nombre = nombre;
            Distancia = distancia;
        }

        public double CalcularDistancia() => Distancia;

        public void Mostrar()
        {
            Console.WriteLine($"- Tramo: {Nombre} ({Distancia} km)");
        }
    }
}

```
Parada.cs
```
using System;

namespace SimuladorTransporte
{
    public class Parada : IRuta
    {
        public string Nombre { get; }
        public double Distancia { get; }

        public Parada(string nombre, double distancia)
        {
            Nombre = nombre;
            Distancia = distancia;
        }

        public double CalcularDistancia() => Distancia;

        public void Mostrar()
        {
            Console.WriteLine($"  • Parada: {Nombre} (+{Distancia} km)");
        }
    }
}

```
RutaCompuesta.cs
```
using System;
using System.Collections.Generic;

namespace SimuladorTransporte
{
    public class RutaCompuesta : IRuta
    {
        private List<IRuta> _elementos = new List<IRuta>();
        public string Nombre { get; }

        public RutaCompuesta(string nombre)
        {
            Nombre = nombre;
        }

        public void Agregar(IRuta elemento) => _elementos.Add(elemento);

        public double CalcularDistancia()
        {
            double total = 0;
            foreach (var e in _elementos)
                total += e.CalcularDistancia();
            return total;
        }

        public void Mostrar()
        {
            Console.WriteLine($"\nRuta: {Nombre}");
            foreach (var e in _elementos)
                e.Mostrar();
        }
    }
}

```
### CARPETA Vehiculos
IVehiculo.cs
```
namespace SimuladorTransporte
{
    public interface IVehiculo
    {
        string Descripcion { get; }
        double Costo { get; }
    }
}

```
AutobusBasico.cs
```
namespace SimuladorTransporte
{
    public class AutobusBasico : IVehiculo
    {
        public string Descripcion => "Autobús básico";
        public double Costo => 10;
    }
}

```
VehiculoDecorator.cs
```
namespace SimuladorTransporte
{
    public abstract class VehiculoDecorator : IVehiculo
    {
        protected IVehiculo _vehiculo;

        public VehiculoDecorator(IVehiculo vehiculo)
        {
            _vehiculo = vehiculo;
        }

        public virtual string Descripcion => _vehiculo.Descripcion;
        public virtual double Costo => _vehiculo.Costo;
    }
}

```
AireAcondicionado.cs
```
namespace SimuladorTransporte
{
    public class AireAcondicionado : VehiculoDecorator
    {
        public AireAcondicionado(IVehiculo vehiculo)
            : base(vehiculo) { }

        public override string Descripcion => _vehiculo.Descripcion + " + Aire Acondicionado";
        public override double Costo => _vehiculo.Costo + 5;
    }
}

```
WiFi.cs
```
namespace SimuladorTransporte
{
    public class WiFi : VehiculoDecorator
    {
        public WiFi(IVehiculo vehiculo)
            : base(vehiculo) { }

        public override string Descripcion => _vehiculo.Descripcion + " + WiFi";
        public override double Costo => _vehiculo.Costo + 6;
    }
}

```

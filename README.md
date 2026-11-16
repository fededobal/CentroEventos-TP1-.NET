# Centro de Eventos: Seminario .NET (primera entrega)

Primer trabajo del Seminario de Lenguajes opción .NET (1er semestre 2025): sistema de consola para administrar eventos deportivos, personas y reservas de un centro deportivo, con persistencia en archivos de texto. La versión final, con interfaz web y base de datos, está en [CentroEventos-FINAL-.NET](https://github.com/fededobal/CentroEventos-FINAL-.NET).

- [Consigna](07_Primer_Trabajo_Para_Entregar.pdf)
- [Informe](Documentación.pdf): descripción del proyecto, arquitectura, proceso de trabajo y conclusiones.
- [Solución de Visual Studio](CentroEventos/CentroEventos.sln)

## Integrantes

* Bie, Leandro
* Castañeda, Isabella
* Dobal, Federico
* Villca, Facundo

## Objetivos

1. Administrar eventos deportivos, personas y reservas con casos de uso de alta, baja, modificación y listado.
2. Validar cada caso de uso antes de ejecutarlo y expresar las fallas con excepciones propias.
3. Respetar la inyección de dependencias por constructor.
4. Persistir toda la información en archivos de texto.

## Arquitectura

Arquitectura limpia en tres proyectos, con las dependencias apuntando hacia el núcleo:

- [CentroEventos.Aplicacion](CentroEventos/CentroEventos.Aplicacion/): núcleo del sistema, sin referencias a los otros proyectos.
  - [Entities/](CentroEventos/CentroEventos.Aplicacion/Entities/): `EventoDeportivo`, `Persona` y `Reserva`.
  - [Enums/](CentroEventos/CentroEventos.Aplicacion/Enums/): `Asistencia` (pendiente, presente, ausente) y `Permiso`.
  - [Exceptions/](CentroEventos/CentroEventos.Aplicacion/Exceptions/): `CupoExcedidoException`, `DuplicadoException`, `EntidadNotFoundException`, `FalloAutorizacionException`, `OperacionInvalidaException` y `ValidacionException`.
  - [UseCases/](CentroEventos/CentroEventos.Aplicacion/UseCases/): un caso de uso por operación, más los especiales (eventos con cupo disponible y asistencia a un evento).
  - [Validators/](CentroEventos/CentroEventos.Aplicacion/Validators/): reglas que se verifican antes de cada caso de uso.
  - [ServicioAutorizacionProvisorio.cs](CentroEventos/CentroEventos.Aplicacion/Services/ServicioAutorizacionProvisorio.cs): en esta entrega solo el usuario con ID 1 tiene permisos.
- [CentroEventos.Repositorios](CentroEventos/CentroEventos.Repositorios/): implementación de los repositorios sobre archivos de texto. Cada entidad tiene su carpeta con el archivo de datos y otro con el último ID asignado, que se autoincrementa.
- [CentroEventos.Consola](CentroEventos/CentroEventos.Consola/): interfaz de usuario.
  - [Program.cs](CentroEventos/CentroEventos.Consola/Program.cs): crea las instancias, inyecta las dependencias y maneja el menú.
  - [Selector.cs](CentroEventos/CentroEventos.Consola/Selector.cs): muestra listas numeradas para elegir personas, eventos y reservas sin tipear IDs.

## Cómo correrlo

Requiere el [SDK de .NET 8.0](https://dotnet.microsoft.com/es-es/download/dotnet/8.0).

Los repositorios abren los `.txt` con rutas relativas (`../../../../CentroEventos.Repositorios/...`) pensadas para ejecutarse desde la carpeta de salida del build. Por eso hay que compilar y correr el programa desde `bin/Debug/net8.0`, no con `dotnet run` desde la carpeta del proyecto:

```bash
git clone https://github.com/fededobal/CentroEventos-TP1-.NET.git
```

```bash
cd CentroEventos-TP1-.NET/CentroEventos/CentroEventos.Consola
```

```bash
dotnet build
```

```bash
cd bin/Debug/net8.0
```

```bash
dotnet CentroEventos.Consola.dll
```

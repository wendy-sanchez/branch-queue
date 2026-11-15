# Branch Queue — Gestor de Colas por Servicio

Aplicación de línea de comandos desarrollada en Python para gestionar las colas de espera de una entidad bancaria ficticia mediante colas FIFO independientes para cada tipo de servicio.

Cuando un cliente llega a la sucursal, recibe un ticket numerado y queda registrado en la cola correspondiente. Cada agente atiende un servicio específico y llama al siguiente cliente de su propia cola, sin necesidad de recorrer los tickets de otros servicios.

Este proyecto se desarrolló como parte de una actividad educativa de **4Geeks Academy**.

## Funcionalidades

- **Emisión de tickets:** Registrar clientes y asignarlos a una cola de servicio.
- **Numeración global:** Generar números de ticket únicos y secuenciales para todos los servicios.
- **Colas independientes:** Mantener colas separadas para depósitos, retiros y gestión de cuentas.
- **Orden FIFO:** Respetar el orden de llegada dentro de cada servicio.
- **Consulta del siguiente cliente:** Identificar el siguiente ticket sin retirarlo de la cola.
- **Atención de clientes:** Extraer el siguiente ticket pendiente de un servicio determinado.
- **Lista de espera:** Mostrar los clientes pendientes agrupados por servicio.
- **Estadísticas:** Contabilizar los clientes en espera por servicio y calcular el total.
- **Validación de entradas:** Gestionar tipos de servicio no válidos y colas vacías mediante errores descriptivos.
- **Interfaz CLI interactiva:** Administrar el sistema desde un menú en español en la terminal.

## Cómo funciona

La aplicación utiliza un diccionario que contiene tres instancias independientes de `collections.deque`:

```python
self._queues = {
    st: deque() for st in VALID_SERVICE_TYPES
}
```

Cada cola corresponde a uno de los servicios disponibles:

- `deposito` — Depósitos
- `retiro` — Retiros
- `gestion_cuenta` — Gestión de cuentas

Los tickets reciben una numeración global secuencial, independientemente del servicio al que pertenezcan.

### Complejidad temporal

| Operación | Complejidad |
|---|---:|
| Emitir ticket | O(1) |
| Llamar al siguiente cliente | O(1) |
| Consultar siguiente cliente | O(1) |
| Listar clientes en espera | O(n) |
| Consultar estadísticas | O(1) |

Al disponer de una cola independiente para cada servicio, cada agente puede recuperar al siguiente cliente sin recorrer los tickets correspondientes a otros servicios.

## Requisitos

- Python 3.8 o superior.
- No requiere dependencias externas.

La aplicación utiliza módulos de la biblioteca estándar de Python, entre ellos `collections.deque`, `dataclasses`, `datetime` y `typing`.

## Instalación y ejecución

Desde el directorio del proyecto, ejecuta:

```bash
python3 branch_queue.py
```

En Windows también puedes utilizar:

```bash
python branch_queue.py
```

## Menú interactivo

La interfaz de terminal ofrece cinco opciones:

1. Emitir un nuevo ticket.
2. Llamar al siguiente cliente.
3. Consultar la lista de espera.
4. Consultar las estadísticas de la cola.
5. Salir de la aplicación.

## Uso programático

El gestor también puede importarse y utilizarse directamente desde otro módulo Python:

```python
from branch_queue import BranchQueue, QueueError

queue = BranchQueue()

# Emitir un ticket
ticket = queue.issue_ticket("Alice", "deposito")

# Consultar el siguiente cliente
next_ticket = queue.peek_next("deposito")

# Llamar al siguiente cliente
served = queue.call_next("deposito")

# Consultar estadísticas
statistics = queue.stats()
```

La excepción `QueueError` permite gestionar los tipos de servicio no válidos y los intentos de llamar a clientes cuando una cola está vacía.

## Estructura del proyecto

```text
branch-queue/
├── CONTEXT.md        # Contexto de negocio y requisitos
├── DESIGN.md         # Análisis de estructuras de datos y concurrencia
├── README.md         # Documentación del proyecto
└── branch_queue.py   # Modelo de tickets, gestor de colas e interfaz CLI
```

## Decisiones de diseño

El documento `DESIGN.md` explica la elección de utilizar una cola `deque` independiente por servicio en lugar de una única cola compartida.

También analiza un escenario de concurrencia en el que dos agentes intentan llamar simultáneamente al siguiente cliente de una misma cola.

La aplicación actual funciona de manera secuencial y no implementa mecanismos de sincronización entre hilos. Se plantea `threading.Lock` como posible solución para una futura implementación multihilo.

## Alcance y limitaciones

Este proyecto es una simulación educativa de gestión de turnos, no un sistema bancario de producción.

- Los tickets de los clientes se almacenan en memoria.
- No dispone de base de datos ni almacenamiento persistente.
- No tiene interfaz gráfica ni integración con quioscos físicos de emisión de tickets.
- No implementa múltiples agentes ni procesamiento concurrente.
- Banco Meridional es una entidad ficticia utilizada para el ejercicio.

---

**Developer Wendy Sanchez**

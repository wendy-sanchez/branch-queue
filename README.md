# Branch Queue — Service-Based Queue Manager

A Python command-line application that manages customer waiting queues for a fictional bank, using independent FIFO queues for each service type.

Customers receive sequentially numbered tickets when they arrive. Each bank agent handles a specific service and calls the next customer from their own queue, without having to search through customers waiting for other services.

This project was developed as part of an educational assignment at **4Geeks Academy**.

## Features

- **Ticket issuance:** Register customers and assign them to a service queue.
- **Global ticket numbering:** Generate unique, sequential ticket numbers across all services.
- **Independent service queues:** Maintain separate queues for deposits, withdrawals, and account management.
- **FIFO scheduling:** Preserve arrival order within each service.
- **Next customer lookup:** Inspect the next ticket without removing it from the queue.
- **Customer calling:** Retrieve and remove the next waiting ticket for a selected service.
- **Waiting list:** Display pending customers grouped by service.
- **Queue statistics:** Count waiting customers by service and calculate the total.
- **Input validation:** Handle invalid service types and empty queues with descriptive errors.
- **Interactive CLI:** Manage the system through a terminal menu in Spanish.

## How It Works

The application uses a dictionary containing three independent `collections.deque` instances:

```python
self._queues = {
    st: deque() for st in VALID_SERVICE_TYPES
}
```

Each queue corresponds to one of the supported services:

- `deposito` — Deposits
- `retiro` — Withdrawals
- `gestion_cuenta` — Account management

Tickets receive a global sequential number, regardless of the service queue to which they belong.

### Time Complexity

| Operation | Time complexity |
|---|---:|
| Issue ticket | O(1) |
| Call next customer | O(1) |
| Peek next customer | O(1) |
| List waiting customers | O(n) |
| Queue statistics | O(1) |

Because each service has its own queue, an agent can retrieve the next customer without traversing tickets belonging to other services.

## Requirements

- Python 3.8 or newer.
- No external dependencies.

The application uses Python standard-library modules, including `collections.deque`, `dataclasses`, `datetime`, and `typing`.

## Getting Started

Run the application from the project directory:

```bash
python3 branch_queue.py
```

On Windows, you can also use:

```bash
python branch_queue.py
```

## Interactive Menu

The terminal interface provides five options:

1. Issue a new ticket.
2. Call the next customer.
3. View the waiting list.
4. View queue statistics.
5. Exit the application.

## Programmatic Usage

The queue manager can also be imported and used directly:

```python
from branch_queue import BranchQueue, QueueError

queue = BranchQueue()

# Issue tickets
ticket = queue.issue_ticket("Alice", "deposito")

# Inspect the next customer
next_ticket = queue.peek_next("deposito")

# Call the next customer
served = queue.call_next("deposito")

# Get queue statistics
statistics = queue.stats()
```

The `QueueError` exception handles invalid service types and attempts to call customers from empty queues.

## Project Structure

```text
branch-queue/
├── CONTEXT.md        # Business context and requirements
├── DESIGN.md         # Data structure and concurrency analysis
├── README.md         # Project documentation
└── branch_queue.py   # Ticket model, queue manager, and CLI
```

## Design Considerations

The `DESIGN.md` document explains the decision to use one deque per service instead of a single shared queue.

It also discusses a concurrency scenario in which two agents attempt to call the next customer from the same service queue simultaneously.

The current application is sequential and does not implement thread synchronization. A `threading.Lock` is discussed as a possible solution for a future multithreaded implementation.

## Scope and Limitations

This project is an educational queue-management simulation, not a production banking system.

- Customer tickets are stored in memory.
- No database or persistent storage is implemented.
- There is no graphical interface or integration with physical ticket kiosks.
- The application does not implement multiple agents or concurrent processing.
- Banco Meridional is a fictional organization used for the assignment.

---

**Developer Wendy Sanchez**

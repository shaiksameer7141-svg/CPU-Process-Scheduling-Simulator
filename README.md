# CPU Scheduling Simulator

An interactive CPU scheduling simulator that demonstrates five scheduling policies using a **binary min-heap (priority queue)**. It visualises process execution with a Gantt chart and reports per-process timing metrics, scheduler decisions, and a comparison of the algorithms.

## Features

- Add, edit, and remove processes.
- Enter each process's:
  - Process ID (PID)
  - Arrival time
  - Burst time
  - Priority number
- Load built-in sample datasets or generate a random process set.
- Simulate and compare five CPU scheduling algorithms:
  1. **FCFS** — First Come, First Served
  2. **SJF** — Shortest Job First (non-preemptive)
  3. **SRTF** — Shortest Remaining Time First (preemptive)
  4. **Priority** — Priority scheduling (non-preemptive)
  5. **Preemptive Priority** — a higher-priority arrival can interrupt the running process
- View a Gantt chart, execution order, ready-queue decision log, and per-process results.
- Compare average waiting, turnaround, and response times, CPU utilisation, and context switches.
- Responsive layout with light and dark theme support based on the system preference.

## Technologies

- **HTML5** — page structure
- **CSS3** — responsive layout and styling
- **JavaScript** — scheduling logic, binary min-heap, validation, and visualisation
- **Python / Flask** — serves the frontend and provides placeholder API routes

## Project structure

The Flask application is configured to load `index.html` from a folder named `templates`:

```text
CPU-Scheduling-Simulator/
├── server.py
├── templates/
│   └── index.html
└── README.md
```

If your downloaded `index.html` is in the project root, create a `templates` folder and move `index.html` into it before starting the Flask server.

## Getting started

### 1. Requirements

- Python 3.8 or later
- pip

### 2. Install Flask

```bash
python -m pip install Flask
```

### 3. Arrange the frontend file

Make sure the project follows the structure shown above. In particular, `index.html` must be inside `templates/`, because `server.py` uses:

```python
app = Flask(__name__, template_folder="templates")
```

### 4. Run the application

From the project directory, run:

```bash
python server.py
```

Open the following address in your browser:

```text
http://127.0.0.1:5000/
```

To stop the development server, press `Ctrl+C` in the terminal.

## How to use

1. Open the application in your browser.
2. Use the sample selector and **Load sample**, or add and edit processes in the process table.
3. Ensure every process has a unique PID, a non-negative integer arrival time, a burst time of at least 1, and an integer priority.
4. Select a scheduling algorithm from the **Algorithm** dropdown.
5. Review the Gantt chart, execution order, timing metrics, per-process results, and scheduler decision log.
6. Open **Performance comparison** to compare all five algorithms for the current process set.
7. Open **How it works** to review the heap-based scheduling approach.

**Priority convention:** a lower numeric priority value represents a higher priority.

## Scheduling algorithms

| Algorithm | Scheduling rule | Preemptive? |
|---|---|---:|
| FCFS | Select the process with the earliest arrival time | No |
| SJF | Select the ready process with the shortest burst time | No |
| SRTF | Select the ready process with the least remaining time | Yes |
| Priority | Select the ready process with the lowest numeric priority | No |
| Preemptive Priority | Select the ready process with the highest priority; a higher-priority arrival can interrupt | Yes |

Tie-breaking uses arrival time and then PID, as implemented in the frontend.

## Metrics

For each process, the simulator calculates:

- **Completion time:** time at which the process finishes.
- **Turnaround time:** `Completion Time - Arrival Time`
- **Waiting time:** `Turnaround Time - Burst Time`
- **Response time:** `First Start Time - Arrival Time`

The summary also reports average waiting time, average turnaround time, average response time, CPU utilisation, and context switches.

## Data structure and complexity

The JavaScript frontend implements a binary min-heap. Each ready-queue entry contains a scheduling key and tie-breakers; the smallest key is selected first. For preemptive policies, the running process is reconsidered when the next process arrives.

The project describes the core scheduler as **O(n log n) time** and **O(n) space**, with heap operations taking **O(log n)**.

## Backend API status

`server.py` defines these Flask routes:

| Route | Method | Current behaviour |
|---|---|---|
| `/` | GET | Renders `templates/index.html` |
| `/api/simulate` | POST | Returns the submitted JSON under `data` |
| `/api/compare` | POST | Returns an empty `results` object |

The two API endpoints are placeholders: the scheduling and comparison calculations currently run in the browser using JavaScript. The frontend does not currently call these Flask API routes.

## Sample data

The interface includes three sample datasets:

- **Classic (5 processes)**
- **All arrive at t=0**
- **With CPU idle gap**

The classic sample is useful for comparing the scheduling policies. The idle-gap sample demonstrates how the timeline represents periods when no process is ready.

## Limitations and possible improvements

- Implement the scheduling and comparison logic in the Flask API if server-side execution is required.
- Add automated unit tests and edge-case coverage.
- Add priority aging to help reduce starvation of low-priority processes.
- Add result export and step-by-step simulation playback.
- Add screenshots and deployment instructions when those assets and details are available.

## References

- Silberschatz et al., *Operating System Concepts*
- [Flask Documentation](https://flask.palletsprojects.com/)
- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)

---

**Project:** CPU-Process-Scheduling-Simulator

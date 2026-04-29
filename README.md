# Automata Simulator (Neo4j Powered)

A comprehensive, single-page web application for visualizing and simulating Deterministic and Non-Deterministic Finite Automata (DFA/NFA) using a Neo4j graph database backend.

## Features

- **Visual Automata Layout**: Uses a custom force-directed graph layout to render states, loops, and transitions clearly using SVG.
- **Neo4j Integration**: Connects directly to a Neo4j database using the browser-based Javascript driver. 
- **In-Browser Creation Tool**: A built-in modal to design new automata, specify alphabets, mark start/accept states, and define transitions (including NFA comma-separated transitions).
- **String Simulation**: Step-by-step backtracking trace simulation with visual feedback (highlighted states and transitions).
- **Layout Persistence**: Export and import Node layout JSON files to persist custom arrangements of your states across sessions.

---

## Why Export/Import JSON Layouts?

The simulator natively stores the **logic** of your automata (States, Transitions, Start/Accept markers) inside the Neo4j Graph Database. However, the exact **visual X/Y coordinates** of each state circle are *not* stored in the database. This keeps the database schema clean and decoupled from the visual representation.

**Why use the Export/Import feature?**
Whenever you move the states around to make the graph look neat, those positions are only saved locally in your browser session. If you switch devices or clear your browser data, the simulator will revert to its default "auto-layout" algorithm.
By exporting your layout to a `.json` file, you can permanently save the visual arrangement of your Automaton and share it with others. If you close the simulator with unsaved layout modifications, the browser will display a warning prompting you to export your layout to avoid losing your visual arrangement.

---

## Prerequisites: Neo4j Installation

To use this simulator, you need an active Neo4j database running either locally or in the cloud.

### Local Installation (Neo4j Desktop)
1. Download [Neo4j Desktop](https://neo4j.com/download/) and install it on your machine.
2. Open Neo4j Desktop and create a new **Local DBMS**.
3. Set the password for the default `neo4j` user.
4. Click **Start** to run the database. The default connection URI is usually `bolt://127.0.0.1:7687` or `neo4j://127.0.0.1:7687`.

### Alternative: Neo4j AuraDB (Cloud)
If you prefer not to install anything, you can use [Neo4j AuraDB](https://neo4j.com/cloud/platform/aura-graph-database/) to create a free cloud instance. You will receive a connection URI (e.g., `neo4j+s://<id>.databases.neo4j.io`) and a password.

---

## How to Use the Simulator

### 1. Connecting to the Database
1. Open the `index.html` file in any modern web browser.
2. In the **Neo4j Connection** panel on the left sidebar, enter your Database URI, Username (usually `neo4j`), and Password.
3. Click **⚡ CONNECT**.

### 2. Creating a New Automaton
1. Click **＋ CREATE NEW AUTOMATON**.
2. **Basic Information**: Give your automaton a Name, a unique ID (no spaces), and an optional description.
3. **Alphabet**: Add symbols your automaton will use (e.g., `0`, `1` or `a`, `b`).
4. **States**: Choose the total number of states. Use the checkboxes to designate Start and Accept states. Check "NFA mode" if a state can transition to multiple targets on a single symbol.
5. **Transition Table**: Define the target state(s) for each symbol. (Leave empty for dead states, or comma-separate for NFAs).
6. Click **💾 SAVE TO NEO4J**.

### 3. Simulating Strings
1. Select your created automaton from the **Available DFAs** dropdown and click **▶ LOAD DFA**.
2. In the **Simulate String** section, enter the input string you want to test.
3. Use the **STEP** button to move through the string one character at a time, or click **RUN ▶** to execute the simulation automatically.
4. The trace log will visually demonstrate the path taken, highlighting the nodes on the canvas in real-time, and will indicate whether the string was `Accepted` or `Rejected`.

### 4. Customizing Layouts
- Drag and drop state nodes on the canvas to arrange them to your liking.
- Use the controls in the bottom right to zoom in/out or fit to screen.
- Use the **↓ EXPORT** button to save your layout locally to a JSON file.
- Use the **↑ IMPORT** button to load a previously saved layout arrangement.

---

## Graph Database Schema Reference

For developers and database administrators, the application creates and queries the following schema in Neo4j:

- **`(a:Automaton)` Nodes**: 
  - Properties: `id` (String), `name` (String), `description` (String).
- **`(s:State)` Nodes**:
  - Properties: `name` (String), `label` (String), `isStart` (Boolean), `isAccept` (Boolean).
- **`[:HAS_STATE]` Relationships**:
  - Links an `Automaton` to its `State` nodes. `(a)-[:HAS_STATE]->(s)`
- **`[:TRANSITION]` Relationships**:
  - Links two `State` nodes. 
  - Properties: `symbol` (String).
  - Represents a transition from one state to another: `(s1)-[:TRANSITION {symbol: "a"}]->(s2)`

---

## License

This project is licensed under the [MIT License](LICENSE).

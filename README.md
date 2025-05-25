# BBD Chess Engine

[![Version](https://img.shields.io/badge/version-1.0.1-blue.svg)](https://github.com/BBDEngine/ChessEngine/releases)
[![C++20](https://img.shields.io/badge/C++-20-blue.svg)](https://en.wikipedia.org/wiki/C%2B%2B20)

A high-performance chess engine built in **C++20**, featuring advanced search techniques, a neural-network-based evaluation (NNUE), and an approximate **2500+ Elo** rating in self-play matches.

## Features

- **Bitboard Representation**: 64-bit board state for efficient move generation  
- **Search Algorithms**: Negamax with Alpha-Beta pruning, Principal Variation Search, Quiescence, Null Move & Reverse Futility Pruning, Aspiration Windows  
- **Move Ordering**: Transposition Tables, Killer Moves, History Heuristic  
- **Memory & Hashing**: Incremental Zobrist updates, Transposition Table for caching results  
- **NNUE Evaluation**: Incrementally updatable neural network for position scoring  
- **Draw Detection**: Threefold repetition checks  
- **UCI Protocol**: Easily integrates with most chess GUIs  

## Getting Started

### Prerequisites

- CMake (version 3.15 or higher)
- Clang++ compiler (for optimal performance)

### Installation

```bash
# Clone the repository
git clone https://github.com/BBDEngine/ChessEngine.git
cd ChessEngine

# Build the project
cmake -DCMAKE_CXX_COMPILER=clang++ -S . -B build/
cmake --build build/

# Run the engine
./build/bbd
```
## Usage

### UCI Mode

BBD supports the Universal Chess Interface (UCI) protocol, allowing it to be used with most chess GUIs.
```bash
./build/bbd
```
Then interact via UCI commands (e.g., `uci`, `position startpos`, `go depth 10`).
### Tournament Mode

BBD can also be used in tournament mode:
```bash
./build/bbd input_file current_position output_file
```
### Benchmarking

You can run a standard benchmark suite:
```bash
./build/bbd bench
```

## Testing

BBD includes a comprehensive test suite to ensure correctness:

```bash
# Build and run the tests
cmake --build build/ --target run-tests
```

##  Development Tools

- **Formatting**: 
  ```bash
  cmake --build build/ --target format-apply
  ```

- **Format Checking**:
  ```bash
  cmake --build build/ --target format-check
  ```

## Acknowledgements

Special thanks to the chess programming community for their invaluable resources and algorithms that have inspired this engine.

---

*"Chess is the gymnasium of the mind." - Blaise Pascal*

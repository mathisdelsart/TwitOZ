# <p align="center"> <img src="image/TwitOZ_title.png" height="100"/> </p>

### An Intelligent Text Prediction System Built with Oz

[Oz](https://mozart.github.io/) [![License](https://img.shields.io/badge/license-Academic-blue.svg)](#license)

A sophisticated text prediction engine powered by N-grams algorithm, processing tweet databases to intelligently predict the next word as you type.

**Features** • [Quick Start](#running-the-app) • [Extensions](#extensions) • [User Guide](#how-to-use)

---

## Overview

TwitOZ is an intelligent text prediction system that leverages N-grams algorithm to analyze patterns in text corpora and predict your next word. Using a database of tweets as training data, it provides real-time suggestions as you type.

### What makes TwitOZ special?

- **Smart Predictions**: N-grams algorithm analyzes word patterns for accurate suggestions
- **Custom Database Support**: Train the system on your own text files
- **Real-time & On-demand**: Automatic or manual prediction modes
- **Word Correction**: Suggests better alternatives for misspelled words
- **Persistent Learning**: User history integration improves predictions over time
- **Extensible Architecture**: Modular design with pluggable extensions

## Quick Start

### Compilation

```bash
# Compile all .oz files (including extensions)
make
```

### Running the Application

```bash
# Launch with default settings
make run

# Launch with custom options (see Extensions section)
make run folder="my_folder" idx_n_grams=4 ext=all
```

### Cleanup

```bash
make clean              # Clean binary files (.ozf)
make clean_historic     # Clean user history
make clean_all          # Clean everything
make help              # Display all available commands
```

## Extensions

Customize TwitOZ behavior with runtime options:

### Optional Parameters

```bash
idx_n_grams=[int]       # N-gram depth (default: 2, min: 1)
corr_word=[0|1]         # Word correction feature (default: 0)
files_database=[0|1]    # Custom database support (default: 0)
auto_predict=[0|1]      # Auto-prediction mode (default: 0)
```

### Required Parameters

```bash
folder=[string]         # Database folder path (default: "tweets")
```

### Quick Enable All

```bash
ext=all                 # Activate all extensions at once
```

### Usage Examples

```bash
# Advanced configuration
make run folder="my_folder" idx_n_grams=4 ext=all

# Word correction only
make run folder="my_folder" corr_word=1

# Custom N-grams with auto-prediction
make run idx_n_grams=3 auto_predict=1
```


## User Guide

<p align="center">
    <img src="image/TwitOZ_apercu.png" width="800"/>
</p>

### Core Features

#### Predict
- **Manual Mode** (default): Click to generate next word prediction based on current input
- **Auto Mode** (with `auto_predict=1`): Real-time predictions update as you type

#### Word Correction
Enter a word to see alternative predictions for each occurrence in your text:
- No match found: *"Correction: No words found."*
- Current word is optimal: *"Correction: your word is correct."*

#### File Operations

**Load from Computer**: Import `.txt` files as input text  
**Save to Computer**: Export current input to a file  

#### Database Management

**Load into Database**: Add your `.txt` files to the training corpus
- Files stored in `user_historic/user_files/` as `historic_partN.txt`
- Chronologically ordered (part1 = oldest)
- Enhances future predictions with your custom data

**Save to Database**: Directly save current input to training data

**Clean History**: Remove all saved historical files from `user_historic/user_files/`

## Architecture & Implementation

### Tree Structure Design

The prediction tree is built in two phases:

1. **Initial Construction**: Creates tree with word-frequency pairs  
   Format: `['Word1'#Freq1 'Word2'#Freq2 ...]`

2. **Optimization**: Transforms to frequency-keyed subtrees  
   Format: `Frequency -> [List of Words]`

This approach simplifies insertions and eliminates the need for node deletions, improving performance.

### Auto-Prediction Threading

Automatic prediction runs as a background thread:
- **Recursive procedure**: Updates every 0.5 seconds
- **Smart interruption**: Pauses for 4 seconds during word correction
- **Port-based communication**: Efficient inter-thread messaging
- **Delta updates**: Only refreshes when prediction changes (prevents UI flashing)

### User History System

The `user_historic/` directory contains:

```
user_historic/
├── user_files/              # User's training data
│   └── historic_partN.txt   # Chronologically saved files
├── last_prediction.txt      # Cache for delta detection
└── nber_historic_files.txt  # File count tracker
```

- Training files analyzed at startup for improved predictions
- Last prediction cached to prevent unnecessary UI updates
- File counter enables efficient batch processing

### Extension Architecture

Extensions are modular (`src/extensions/`):
- **Separation of concerns**: Basic vs. extended functionality
- **Easy toggling**: Enable/disable features without code changes
- **Integration points**: `tree.oz`, `interface.oz`, `function.oz`

Note: Extension code prioritized functionality over readability due to time constraints. Core modules maintain clean architecture.


## Known Issues

### macOS Compatibility

**Symptom**: Compilation errors or missing UI elements (button colors)

**Workaround**: Comment out the image at line 45 in [src/extensions/interface_improved.oz](src/extensions/interface_improved.oz)

**Cause**: Platform inconsistencies in Oz's QTk module between macOS and other systems

**Reference**: See [image/](image/) folder for expected output

We apologize for this platform-specific limitation inherent to the Oz environment.

---

## Project Structure

```
TwitOz/
├── main.oz                  # Application entry point
├── Makefile                 # Build automation
├── src/                     # Core source files
│   ├── function.oz          # Prediction logic
│   ├── interface.oz         # UI components
│   ├── parser.oz            # Text parsing
│   ├── reader.oz            # File I/O
│   ├── tree.oz              # N-gram tree structure
│   ├── variables.oz         # Global state
│   └── extensions/          # Extension modules
├── tweets/                  # Default training corpus
│   └── part_*.txt           # Tweet database files
├── user_historic/           # User data & cache
└── image/                   # Screenshots & assets
```

---

## Technologies

- **Oz/Mozart**: Concurrent constraint programming language
- **QTk**: GUI toolkit for Mozart
- **N-grams Algorithm**: Statistical text prediction
- **Multi-threading**: Real-time background processing
- **Port-based Communication**: Inter-thread messaging

---

## Academic Context

This project was developed as part of **LINFO1104** at UCLouvain. It demonstrates practical implementation of:

- N-grams algorithm for text prediction
- Concurrent programming with Oz threads and ports
- Tree data structures for efficient word lookup
- GUI development with QTk
- Modular software architecture
- File I/O and data persistence

---

## License

This project is developed for academic purposes as part of university coursework.

Built for **LINFO1104** @ UCLouvain  
An intelligent text prediction system powered by N-grams

# Mars rover kata in C++

[![CI](https://github.com/Coding-Cuddles/mars-rover-cpp-kata/actions/workflows/main.yml/badge.svg)](https://github.com/Coding-Cuddles/mars-rover-cpp-kata/actions/workflows/main.yml)
[![C++17](https://img.shields.io/badge/C%2B%2B-17-blue.svg)](https://en.cppreference.com/w/cpp/17)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Implement the Mars Rover exercises in C++17 with GoogleTest. Setup is complete
when the starter test passes.

## Overview

This kata complements [Clean Code: Advanced TDD, Ep. 20](https://cleancoders.com/episode/clean-code-episode-20).

This repository contains several exercises designed to improve your skills in
test-driven development.

## Instructions

As a NASA engineer, you are part of the team that explores Mars by sending
remotely controlled vehicles to the planet's surface. You need to develop a
control software that translates the commands sent from Earth to instructions
that the rover understands.

NASA engineers treat the surface of Mars as a square grid, with side length
being a power of two, e.g., 4x4, 8x8, 16x16, etc.

On the grid, the rover's location is defined by coordinates (x, y) and an
orientation represented by one of the four compass directions (N, S, W, or E).
Coordinates (0, 0) represent the bottom left corner of the grid.

Given an initial rover's location, NASA sends commands encoded as a sequence
(string) of characters with the following meaning:

* 'L' and 'R' turn the rover 90 degrees left or right, respectively;
* 'F' and 'B' move the rover forward or backward one grid point.

### Exercise 1

For *Opportunity*, the grid had the torus (or "donut") topology, where (think
games like Snake or Pacman) the rover vanishes on the top and reappears on the
bottom (and visa versa for left and right).

> [!NOTE]
> In a 4x4 grid, the following table shows the resulting position for a
> movement on the grid (edge cases shown in **bold**):
>
> | Position | x + 1  | x - 1      | y + 1  | y - 1      |
> |----------|--------|------------|--------|------------|
> | (0, 0)   | (1, 0) | **(3, 0)** | (0, 1) | **(0, 3)** |
> | (1, 0)   | (2, 0) | (0, 0)     | (1, 1) | **(1, 3)** |
> | (1, 1)   | (2, 1) | (0, 1)     | (1, 2) | (1, 0)     |

Your task is to implement Opportunity's control software.

### Exercise 2

For *Curiosity*, NASA decided to improve the accuracy of their grid system by
using a Polar coordinate system. This interpretation of the grid system lends
itself to the concept of latitude and longitude: the sphere is sliced into an
even number of latitudes (central lines) and longitudes (evenly spaced lines
from North to South pole). In this model, coordinates (x, y) become abstract
representations of longitudes and latitudes.

While this model is closer to planets, it produces some significant edge cases
that complicate the control system. For example, the behavior is undefined at
the poles if we want the poles to be represented through the coordinates.

The chief engineer decided to constrain the solution to the following: the
poles are not "on the grid," so the rover moves "over" them but never rests
on them.

> [!NOTE]
> In a 4x4 grid, the following table shows the resulting position for a
> movement on the grid (edge cases shown in **bold**):
>
> | Position | x + 1  | x - 1      | y + 1  | y - 1      |
> |----------|--------|------------|--------|------------|
> | (0, 0)   | (1, 0) | **(3, 0)** | (0, 1) | **(2, 0)** |
> | (1, 0)   | (2, 0) | (0, 0)     | (1, 1) | **(3, 0)** |
> | (1, 1)   | (2, 1) | (0, 1)     | (1, 2) | (1, 0)     |

Your task is to extend the rover control software that you created for
*Opportunity* to support this new "polar" grid topology. The control software
should still support the older "torus" topology, and your implementation should
provide a way to select the desired topology.

### Exercise 3

Now, each grid point might contain an obstacle, so we need to implement
obstacle detection. If a given sequence of commands encounters an obstacle, the
rover moves up to the last possible point, aborts the sequence, and reports the
obstacle by throwing an exception.

Your task is to extend the rover control software to support obstacle
detection.

## Prerequisites

Required:

- [Git](https://git-scm.com/downloads)
- A compiler with C++17 support. Choose one:
  - [GCC](https://gcc.gnu.org/) 10+ on Linux
  - [LLVM Clang](https://llvm.org/) 14+ on Linux
  - [Apple Clang](https://developer.apple.com/xcode/) 17+ on macOS
  - [MSVC](https://visualstudio.microsoft.com/) 2022 on Windows
- [CMake 3.24 or later](https://cmake.org)

Optional:

- [GNU Make](https://www.gnu.org/software/make/), for shorter commands. Every
  required task also has direct CMake and CTest commands. Make may be
  unavailable on Windows.

You do not need to install GoogleTest separately. CMake finds an installed
copy or downloads the pinned release when needed.

## Set up the kata

1. Clone the repository:

   ```console
   git clone https://github.com/Coding-Cuddles/mars-rover-cpp-kata.git
   ```

2. Enter the repository directory:

   ```console
   cd mars-rover-cpp-kata
   ```

3. Run the starter test. Use Make when it is installed:

   ```console
   make test
   ```

   Otherwise, use CMake and CTest directly:

   ```console
   cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
   cmake --build build --config Debug
   ctest --test-dir build --build-config Debug --output-on-failure
   ```

The first run may download and build GoogleTest. CTest should report
`100% tests passed`.

If a command reports a missing compiler or CMake, install that prerequisite
and run the setup commands again.

Setup is complete when CTest reports `100% tests passed`.

## Make command reference

Make is optional. Run `make` or `make help` to list these commands in the
terminal.

| Command             | Result                                    |
| ------------------- | ----------------------------------------- |
| `make all`          | Build and run the test suite              |
| `make help`         | Show the Make command reference           |
| `make build`        | Configure and build without running tests |
| `make run`          | Build and run the main executable         |
| `make test`         | Build and run the test suite              |
| `make format`       | Format tracked C++ and header files       |
| `make format-check` | Check formatting without changing files   |
| `make clean`        | Remove generated build artifacts          |

## Credits and references

* <https://kata-log.rocks/mars-rover-kata>

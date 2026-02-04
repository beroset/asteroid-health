# Asteroid Health - GitHub Copilot Instructions

## Project Overview

Asteroid Health is a health tracking application for AsteroidOS smartwatches. It provides a graphical frontend for data collected by the `sensorlogd` daemon, displaying step counts, heart rate, and weight data with interactive graphs.

## Technology Stack

- **UI Framework**: QML (QtQuick 2.15)
- **Backend**: C++ (Qt5)
- **Build System**: CMake (minimum version 3.20)
- **Platform**: AsteroidOS (asteroid-app framework)
- **Target Architecture**: ARMv7 VEHF with NEON

## Project Structure

```
src/
├── main.qml                 # Main application entry point
├── main.cpp                 # C++ entry point
├── cpp/                     # C++ components
│   ├── lineGraph.h/cpp     # Custom line graph widget for data visualization
├── graphs/                  # Graph QML components
├── stepCounter/            # Step counter UI components
├── heartrate/              # Heart rate UI components
├── weight/                 # Weight tracking UI components
└── settings/               # Settings pages
```

## Code Style and Conventions

### QML Files

- Use QtQuick 2.15 imports
- Follow AsteroidOS UI patterns (Application, LayerStack, PageHeader, etc.)
- Use ConfigurationValue for persistent settings stored in dconf
- Component IDs should be camelCase (e.g., `id: pageStack`)
- Properties should be camelCase

### C++ Files

- Header guards should use `#ifndef FILENAME_H` / `#define FILENAME_H` pattern
- Use Q_OBJECT macro for QObject-derived classes
- Use Q_PROPERTY for properties exposed to QML
- Use Q_INVOKABLE for methods callable from QML
- Include copyright headers (BSD license for cpp/lineGraph, GPL v3 for other files)

### File Organization

- QML components are organized by feature (stepCounter, heartrate, weight, graphs, settings)
- Reusable graph components go in `graphs/`
- Each feature has its own directory with Preview and DetailPage components

## Data Storage

- User data is stored in `~/.asteroid-sensorlogd/`
- Each sensor has its own directory
- Data files are plain text with one recording per line
- Format: `[seconds since UNIX epoch]:[value]`
- Separate files for each day

## Key Features and Patterns

### Graph Components

- `LineGraph` (C++): Custom painted item for rendering line graphs
- Loads data from files using `loadGraphData(QVariant fileDataInput)`
- Supports relative mode for displaying changes over time
- Properties: lineWidth, lineColor, maxValue, minValue, maxTime, minTime

### Configuration

- Uses `Nemo.Configuration` for settings persistence
- Settings keys follow pattern: `/org/asteroidos/health/ui/[feature]/[setting]`
- Each feature can be shown/hidden via `showpreview` configuration value

### Logger Integration

- Uses `org.asteroid.sensorlogd 1.0` module
- `LoggerSettings` component provides interface to sensor logger daemon
- Can enable/disable step and heart rate tracking

## Building and Testing

### Build Commands

```bash
cmake -B build
cmake --build build
```

### Package Creation

The project uses CPack to create IPK packages for AsteroidOS. Package metadata is defined in `CMakeLists.txt`.

## Common Tasks

### Adding a New Sensor/Feature

1. Create a new directory under `src/` (e.g., `src/newsensor/`)
2. Add Preview and DetailPage QML components
3. Create graph component in `src/graphs/` if needed
4. Add preview to main.qml's Column
5. Add ConfigurationValue for showing/hiding the preview
6. Add settings controls in `settings/UiSettingsPage.qml`

### Adding a New Graph Type

1. Create QML component in `src/graphs/`
2. Follow existing patterns (TimeLabels, VerticalLabels for axes)
3. Use LineGraph C++ component or create new painted item if needed
4. Load data using the file format: `[timestamp]:[value]`

### Translations

- Translation infrastructure is set up via AsteroidTranslations CMake module
- Translation files should be placed in `i18n/` directory
- Currently, translations are TODO/not yet implemented

## Important Notes

- **Privacy**: This app stores all data locally. No internet connectivity or data sharing features
- **Data Access**: Users can access their data directly from `~/.asteroid-sensorlogd/`
- **Modularity**: The app doesn't yet expose all features that sensorlogd supports (e.g., custom recording intervals, GPS, altimeter, barometer)
- **UI Consistency**: Follow AsteroidOS design patterns for consistency with other system apps

## License

- Main project: GNU General Public License v3
- lineGraph component: BSD License

## Dependencies

- AsteroidApp framework (required)
- ECM (Extra CMake Modules)
- Qt5/QML modules

## Future Improvements (TODO)

- Add translations/internationalization
- Support for additional sensors (GPS, altimeter, barometer)
- Custom recording intervals configuration
- More graph types and visualization options

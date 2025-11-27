# Planar-Magnetic-Actuation-HTML-Interface

A web-based controller interface for an 8×8 electromagnet grid system used to actuate and move magnetic pucks using targeted electromagnetic pulses.

## Overview

This project provides an HTML/JavaScript interface to control a planar magnetic actuation system (PEMA). It allows users to:
- Define movement paths for a magnetic puck across an 8×8 grid of electromagnets
- Configure movement parameters like speed, intensity, and thickness
- Visualize the puck's position, active coils, and movement paths
- Control the system locally or send commands via WebSocket to a remote backend

## Features

### Core Functionality
- **Grid Visualization**: 8×8 grid display showing electromagnet coils and the moving puck
- **Path Planning**: Set start (From) and end (To) coordinates for the puck to follow
- **Movement Control**: 
  - Simple point-to-point movement along the shortest path (DDA algorithm)
  - Thick path mode for multi-cell wide movements
  - Adjustable speed (milliseconds per step) and intensity (0-255)
- **Real-time Animation**: Smooth puck movement with visual feedback
- **Single Active Coil**: Only one electromagnet coil is energized at a time during movement
- **Emergency Stop**: Immediate halt of all operations

### User Interface Controls
- **From/To Coordinates**: Input fields to specify movement start and end positions
- **Speed (ms/step)**: Control movement speed; default 120ms per cell
- **Intensity (0-255)**: Set electromagnet pulse strength; default 200
- **Thickness (cells)**: Define path width for wider movements; default 0 (single cell)
- **Move Button**: Execute animation of puck along calculated path
- **Send End Only**: Send only endpoint coordinates to backend (no full path animation)
- **EMERGENCY STOP**: Kill all running animations and stop the system
- **Seed Demo**: Load pre-configured demo grid state with sample coil values
- **Force Demo**: Toggle demo mode on/off

### Visual Elements
- **Coil Display**: Black circles at each grid cell representing electromagnets
- **Active Coil Highlight**: Golden-yellow radial gradient showing currently energized coil
- **Puck Representation**: Blue circle with glow effect showing current position and coordinates
- **Cell Background Color**: Faint color-coded background based on coil intensity values
- **Grid Lines**: Light gray lines delineating the 8×8 cell layout

### Path Algorithms
- **DDA Algorithm**: Digital Differential Analyzer for calculating shortest paths between two points
- **Thick Path Algorithm**: Generates wider paths by finding all cells within a specified distance from the line segment
  - Uses point-to-segment distance calculations
  - Ensures continuous path connectivity by sorting by parameter t along the line

## Technical Details

### Communication Protocol
- **WebSocket Integration**: Sends commands to a backend server via JSON messages
- **Command Format**: 
  - `move_path`: Full path with interpolation steps
  - `move_to`: Endpoint-only command with optional thickness
  - `stop`: Emergency stop command

### Animation System
- **Pre-activation Timing**: Electromagnet energizes slightly before puck movement (20% of step time, minimum 15ms)
- **Smooth Interpolation**: Puck movement uses frame-based interpolation at 60fps
- **Timeout Management**: Careful handling of async animations with cleanup on stop

### Key Constants
- Grid Size: 8×8 cells
- Canvas Size: 480×480 pixels
- Puck Radius Factor: 0.38 (relative to cell size)
- Demo Mode: Enabled by default with pre-seeded grid values

## Usage

1. **Open PEMA.html** in a modern web browser
2. **Set Coordinates**: Enter From and To row/column values (0-7 range)
3. **Configure Parameters**: Adjust speed, intensity, and thickness as needed
4. **Click on Grid** (optional): Click any cell to quickly set the To coordinate
5. **Execute Movement**: Click "Move" to animate the puck along the calculated path
6. **Monitor Status**: WebSocket connection status shown in top-right corner

## Demo Mode

The interface includes a demo mode for testing without a backend:
- Click "Seed Demo" to load sample data into the grid
- Click "Force Demo: ON/OFF" to toggle demo mode
- Full local animations run when demo mode is active, even without WebSocket connection
- Backend commands will not send if WebSocket is not connected

## Browser Requirements

- Modern HTML5 canvas support
- JavaScript ES6+ (Promise, async/await)
- WebSocket support (for backend communication)

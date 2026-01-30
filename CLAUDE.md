# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SDRTrunk is a cross-platform Java application for decoding, monitoring, recording, and streaming trunked mobile radio protocols using Software Defined Radios (SDR). It supports protocols including P25, DMR, LTR, MPT1327, Fleetsync, MDC1200, and others.

## Build Commands

```bash
# Run the application
./gradlew run

# Run tests
./gradlew test

# Build without running
./gradlew build

# Create release package for current OS
./gradlew runtimeZipCurrent

# Create release packages for Linux/OSX
./gradlew runtimeZipOthers

# Create release packages for Windows
./gradlew runtimeZipWindows
```

## Development Requirements

- **JDK**: Java 25+ with JavaFX modules (Bellsoft Liberica JDK Full recommended)
- **Preview Features**: Project Panama Vector API is used (requires `--enable-preview`)
- **Testing**: JUnit 6 (Jupiter)

## Code Architecture

### Source Structure
- `src/main/java/io/github/dsheirer/` - Main application code
- `src/test/java/` - Unit tests
- Main entry point: `io.github.dsheirer.gui.SDRTrunk`

### Key Packages

| Package | Purpose |
|---------|---------|
| `gui/` | Swing/JavaFX user interface |
| `source/tuner/` | SDR hardware interfaces (RTL-SDR, SDRPlay, Airspy, HackRF) |
| `module/decode/` | Protocol decoders (P25, DMR, LTR, MPT1327, etc.) |
| `dsp/` | Digital signal processing (filters, demodulation) |
| `audio/` | Audio playback, recording, and streaming |
| `channel/` | Channel processing pipeline management |
| `playlist/` | Configuration and playlist management |
| `alias/` | Talkgroup/radio ID aliasing and actions |
| `identifier/` | Radio/talkgroup/channel identifiers |
| `message/` | Decoded message representations |
| `record/` | Audio and baseband recording |

### Processing Pipeline

1. **Source** (tuner) provides RF samples
2. **Channel** extracts specific frequency from wideband samples
3. **Demodulator** converts RF to baseband
4. **Decoder** extracts protocol messages
5. **Module** processes messages for audio, recording, streaming

### Key Patterns

- Event-driven architecture using Guava EventBus (`MyEventBus`)
- Listener/broadcaster pattern for sample and message flow
- Configuration stored as XML via Jackson
- Playlists define channel configurations and aliases

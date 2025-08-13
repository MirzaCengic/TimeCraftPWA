# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Timecraft is a professional section-based metronome PWA designed for intermediate to advanced musicians. It allows creating different "sections" (A, B, C) with unique tempos, time signatures, and subdivisions, then arranging them into sequences for practice (e.g., A-A-B-A-C-B).

**Current State**: Fully functional HTML prototype ready for UI/UX improvements and PWA conversion.

## Development Commands

Since this is a client-side only HTML application, development is straightforward:

- **Run locally**: Open `metronome_prototype.html` in a web browser
- **Test audio timing**: Use Chrome/Edge for best Web Audio API support  
- **Mobile testing**: Use browser dev tools device simulation or test on actual devices
- **PWA testing**: Serve over HTTPS (required for Web Audio API and PWA features)

**No build process, package managers, or server required.**

## Core Architecture

### Single-File Architecture
The entire application is contained in `metronome_prototype.html` with three main sections:
1. **CSS Styles** (lines 7-278): Responsive UI with glassmorphism design
2. **HTML Structure** (lines 280-465): Three-panel layout (Section Builder, Arrangement Builder, Playback Control)  
3. **JavaScript Logic** (lines 467-1610): Pure vanilla JS with Web Audio API

### Key Technical Concepts

**Dual-Timeline System**: Critical for understanding the codebase
- `currentArrangementIndex/currentBar/currentBeat`: What's being scheduled next (lookahead)
- `playingArrangementIndex/playingBar/playingBeat`: What's currently audible
- This separation prevents visual jitter while maintaining audio precision

**Audio Scheduling Architecture**:
- Uses Web Audio API's precise timing (not setTimeout/setInterval)
- `scheduler()` function runs every 25ms checking what needs to be scheduled
- `scheduleAheadTime` (0.1s) ensures audio buffers are filled in advance
- Critical for drift-free timing across tempo changes

**Section Data Structure**:
```javascript
sections[name] = {
    tempo: number,
    bars: number, 
    timeSignature: "4/4",
    subdivision: number, // 1=quarters, 2=8ths, 3=triplets, etc.
    subdivisionVolume: number,
    sound: string, // 'classic', 'soft', 'sharp', 'deep', 'silent'
    accentSectionStart: boolean,
    swingFeel: boolean
}
```

**Global State Variables** (lines 468-493):
- `sections`: Object storing all created sections
- `arrangement`: Array of section names in sequence
- `isPlaying/isCountingIn`: Playback state
- `audioContext`: Web Audio API context
- `globalMainVolume/globalSubVolume`: Master volume controls

### Core Functions

**Audio Functions**:
- `scheduler()`: Main timing loop using Web Audio API precision
- `playClick()`: Generates metronome sounds with different waveforms
- `getSoundSettings()`: Maps sound types to frequencies and waveforms
- `getBeatDuration()`: Calculates timing based on tempo and time signature

**Section Management**:
- `addSection()`: Creates/updates sections with validation
- `updateSectionDisplay()`: Renders section list with edit/delete buttons
- `getUpcomingSection()`: Shows next section in arrangement (uses `currentArrangementIndex`)

**Arrangement Building**:
- `addToArrangement()`: Adds sections to sequence
- `updateArrangementDisplay()`: Visual arrangement with current section highlighting
- `loadPreset()`: Template arrangements (verse-chorus, drum groove, etc.)

**State Management**:
- `updateStatus()`: Updates BPM/section/bar/beat display
- `autoSave()/autoLoad()`: LocalStorage persistence
- Count-in logic integrated throughout scheduler

### Mobile & PWA Considerations

**Current Responsive Design**:
- Breakpoint at 768px switches to single-column layout
- Touch targets currently 8px padding (needs improvement to 44px minimum)
- Visual metronome has size options: normal (50px), large (100px), full (200px)

**Web Audio API Requirements**:
- Requires HTTPS in production
- Needs user gesture to initialize `audioContext`
- `initAudio()` handles context creation and resumption

### Data Persistence

**Auto-save**: Automatically saves to `localStorage` on section/arrangement changes
**Export/Import**: JSON format for sharing arrangements
**Templates**: Built-in presets for common musical structures

### Known Limitations & Next Steps

1. **UI/UX**: Currently optimized for desktop; needs mobile-first redesign
2. **PWA Conversion**: Needs manifest.json, service worker, app icons
3. **Touch Targets**: Too small for mobile use (8px → 44px minimum)
4. **Performance Mode**: Should separate setup/performance interfaces

### Testing Focus Areas

- **Audio Timing**: Test across browsers for drift-free playback
- **Complex Time Signatures**: 7/4, 19/16, irregular patterns
- **Subdivision Accuracy**: Triplets, quintuplets, septuplets timing
- **Mobile Audio**: Test autoplay policies and audio context initialization
- **PWA Installation**: Add to home screen functionality

### Code Style Notes

- Pure vanilla JavaScript (no frameworks)
- Global functions exposed on `window` object for HTML onclick handlers
- Event listeners added in `init()` function  
- Uses modern ES6+ features (const/let, template literals, arrow functions)
- Audio timing uses high-precision scheduling, not DOM timers
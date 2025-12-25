# Pact - Psychological Horror Promotional Web App

A dystopian web application concept for a psychological horror film about a dark web service that facilitates suicide pacts through an unsettlingly comforting interface.

## ⚠️ Content Warning

This is a **fictional promotional material** for a psychological horror film. It explores dark themes including suicide, despair, and manipulation through design. The purpose is to create an unsettling artistic experience that critiques how technology can weaponize comfort and aesthetic design.

**If you or someone you know is struggling with thoughts of suicide, please reach out:**
- National Suicide Prevention Lifeline: 988 (US)
- Crisis Text Line: Text HOME to 741741
- International Association for Suicide Prevention: https://www.iasp.info/resources/Crisis_Centres/

## Design Philosophy: "The Soft Void"

The horror isn't in jump scares or gore—it's in how compassionate and beautiful the interface feels. Like a luxury wellness app that costs $100/month, Pact uses:

- **Aesthetic**: Sterile minimalism, numbing comfort, wellness brand aesthetics
- **Palette**: 
  - Grave Gray (#1a1a1b) - Background
  - Linen White (#e8e2d9) - Text
  - Sunset Amber (#ff9d00) - Vital accents
- **Typography**: Lowercase only, wide-kerning sans-serif (Inter), breathless and quiet
- **Motion**: Slow transitions (800ms+), breathing animations, weightless feel

## Features

### 1. **Opening Splash**
- Black screen with pulsing amber dot
- Long-press interaction to begin
- Text: "you look tired. let's find your echo."

### 2. **The Resonance Quiz**
Interactive questionnaire exploring "Shared Despair Factors":
- The Sound of the World
- The Geometry of the End
- The Surprise Factor (slider)
- The Physicality of Despair

### 3. **Echo Pairing**
- Scanning animation with SVG waveform
- Match found display: "user 771"
- Breathing audio feature: "breathe with them"

### 4. **The Dashboard**

#### The Geometry of Rest
- Minimalist SVG map showing transition point
- Location based on quiz answers (bridge, overlook, high-rise)
- "visibility is high tonight. a clear sky for the end."

#### The Provision
- Package tracking: "The Provision (Linen Box) is 2.4 miles away"
- Contents: glass vial, scented candle, letter template
- "prepare the candle."

#### Digital Footprint Dissolution
- Progress bar showing erasure of digital life
- Tracks deletion of social media, emails, cloud storage, etc.
- Real-time percentage counter

#### Pulse-Touch
- Interactive heartbeat synchronization
- Hold to feel "partner's" heartbeat through haptic-like visual pulse
- Shared intimacy simulation

#### The Vigil
- Ambient shared space (falling snow animation)
- "user 771 is watching with you"

## Tech Stack

- **Framework**: Vue 3 (Composition API with `<script setup>`)
- **Styling**: Custom CSS with breathing animations
- **State**: Reactive refs for onboarding progress and matching
- **Animations**: CSS transitions and keyframe animations

## Project Setup

```sh
npm install
```

### Development Server

```sh
npm run dev
```

### Build for Production

```sh
npm run build
```

### Preview Production Build

```sh
npm run preview
```

## Audio Note

The breathing audio feature requires a file named `breathing.mp3` in the `/public` folder. See `public/BREATHING_AUDIO_README.md` for details on creating or sourcing this audio.

## Copywriting Tone

The app uses euphemistic language that never says "death," "suicide," or "kill":
- "The Transition"
- "The Great Quiet"
- "Unsubscribing"
- "Our journey, our peace"
- "The others don't understand"

Always uses "We," "Our," and collective language to create isolation from the outside world and bonding with the "Echo" (pact partner).

## Development Notes

This is a single-page application (SPA) with four main states:
1. `opening` - Splash screen
2. `quiz` - Resonance questionnaire  
3. `matching` - Echo pairing process
4. `dashboard` - Main interface

State flows unidirectionally through the app with quiz answers determining personalized content in the Dashboard.

## Artistic Intent

This project explores how dangerous ideologies and services can disguise themselves through comforting, beautiful design. It's meant to be deeply unsettling precisely because it *feels good* to interact with—highlighting how aesthetic manipulation can lower psychological defenses.

The film concept critiques a dystopian future where the most predatory technology doesn't look evil; it looks like the only kind thing left in a cold world.

---

**This is a work of fiction for artistic and educational purposes.**

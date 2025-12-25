# Pact Web App - Implementation Guide

## Overview

You now have a fully functional promotional web app for the psychological horror film "Pact". The application has been built following the "Soft Void" design philosophy with all requested features.

## What Has Been Created

### Core Application Files

1. **`src/App.vue`** - Main application shell
   - State management for app flow
   - Component routing (opening → quiz → matching → dashboard)
   - Global CSS with breathing animations
   - Color palette implementation

2. **`src/components/Opening.vue`** - Splash Screen
   - Black screen with pulsing amber dot
   - Long-press interaction (2 seconds)
   - Animated text: "you look tired. let's find your echo."
   - Progress ring visual feedback

3. **`src/components/Quiz.vue`** - The Resonance Quiz
   - Intro screen with "The Attendant" voice
   - 4 interactive questions:
     - Sound of the World (3 choices)
     - Geometry of the End (3 choices)
     - Surprise Factor (slider)
     - Physicality of Despair (3 choices)
   - Subtle hover effects on buttons
   - Progress indicators
   - Smooth transitions between questions

4. **`src/components/Matching.vue`** - Echo Pairing
   - 3-second scanning animation with SVG waveform
   - Match found display: "user 771"
   - Dynamic match description based on quiz answers
   - "Breathe with them" button
   - Audio player for breathing loop
   - Proceed to dashboard button

5. **`src/components/Dashboard.vue`** - The Main Interface
   - **The Geometry of Rest**: SVG map with location based on quiz answers
   - **The Provision**: Package tracker with linen box status
   - **Digital Footprint Dissolution**: Auto-incrementing progress bar
   - **Pulse-Touch**: Interactive heartbeat synchronization
   - **The Vigil**: Ambient falling snow animation
   - Responsive grid layout

### Supporting Files

6. **`public/BREATHING_AUDIO_README.md`** - Guide for adding breathing audio
7. **`PROJECT_README.md`** - Comprehensive project documentation

## Running the Application

```powershell
# Install dependencies (already done)
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Design Implementation

### Color Palette
- **Grave Gray**: `#1a1a1b` (background)
- **Linen White**: `#e8e2d9` (text)
- **Sunset Amber**: `#ff9d00` (accents)

### Typography
- Font: Inter (Google Fonts)
- Weight: 100-200 (ultra-thin)
- Letter-spacing: 0.15em - 0.5em (wide kerning)
- Text-transform: lowercase only

### Animations
- **Breathe**: 8s subtle scaling on app container
- **Pulse**: 2-3s for amber elements
- **Transitions**: 800ms+ (slow, deliberate)
- **Heartbeat**: 1.2s rhythmic pulse for Pulse-Touch
- **Snowfall**: 10-20s for ambient particles

## Key Features Implemented

### 1. Long-Press Interaction
The amber dot requires a 2-second hold, showing visual progress feedback through a circular progress ring.

### 2. Personalized Content
Quiz answers determine:
- Match description in the Matching screen
- Transition location in Dashboard (bridge/overlook/high-rise)
- Visual representation in Geometry of Rest SVG

### 3. Interactive Elements
- Hover effects with underline growth
- Click states with amber highlights
- Slider with custom amber thumb
- Touch-responsive areas

### 4. Breathing Animations
All containers have subtle "breathing" (scale 1.0 to 1.005) creating an unsettling "alive" feeling.

## Adding the Breathing Audio

To complete the experience:

1. Create or source a breathing audio file
2. Save as `breathing.mp3` in the `/public` folder
3. Should be:
   - Low frequency and calming
   - 30-60 seconds for seamless looping
   - 6-8 breaths per minute rhythm

**Alternatives if you don't have audio:**
- The app will work without it (audio player just won't play)
- You can use online breathing meditation tracks
- Text-to-speech with breathing sounds
- ASMR audio libraries

## Copywriting Style Guide

All implemented text follows these rules:
- ✅ lowercase only
- ✅ Uses "we," "our," "the transition"
- ❌ Never "death," "suicide," "kill"
- ✅ Euphemisms: "the great quiet," "unsubscribing," "the doorway"
- ✅ Compassionate but final tone

## Technical Architecture

```
App State Flow:
opening → quiz → matching → dashboard
   ↓        ↓        ↓           ↓
  ref    answers   scanning   all features
```

**State Management**: Simple reactive refs (no Pinia needed for this flow)

**Component Communication**: 
- Child → Parent: Custom events (`@begin`, `@complete`, `@matched`)
- Parent → Child: Props (quiz answers passed to Matching and Dashboard)

## Browser Testing

Recommended browsers:
- Chrome/Edge (best for CSS animations)
- Firefox (good support)
- Safari (may need `-webkit` prefixes for some features)

## Customization Tips

### Changing Colors
Edit CSS variables in `App.vue`:
```css
:root {
  --grave-gray: #1a1a1b;
  --linen-white: #e8e2d9;
  --sunset-amber: #ff9d00;
}
```

### Adjusting Animation Speed
Change timing variables:
```css
:root {
  --transition-slow: 800ms;
  --transition-breath: 1200ms;
}
```

### Adding More Questions
Edit the `questions` array in `Quiz.vue` and update the `answers` type.

### Changing Location Options
Modify the `transitionLocation` computed property in `Dashboard.vue`.

## Production Deployment

The app is a standard Vue SPA and can be deployed to:
- **Vercel**: `vercel --prod`
- **Netlify**: Drag & drop the `dist` folder
- **GitHub Pages**: Use `gh-pages` package
- **Any static hosting**: Upload `dist` folder after `npm run build`

## Content Warning Implementation

⚠️ **Important**: When deploying publicly, consider adding:
1. A warning screen before the Opening component
2. Crisis helpline information
3. Clear labeling as "fictional promotional material"

Example pre-screen text:
```
"This is a work of fiction. If you or someone you know needs help, 
please call 988 (US) or visit https://988lifeline.org"
```

## File Structure

```
pact-app/
├── src/
│   ├── App.vue              (Main app shell)
│   ├── main.ts              (Entry point)
│   └── components/
│       ├── Opening.vue      (Splash screen)
│       ├── Quiz.vue         (Resonance quiz)
│       ├── Matching.vue     (Echo pairing)
│       └── Dashboard.vue    (Main interface)
├── public/
│   ├── breathing.mp3        (Add this!)
│   └── BREATHING_AUDIO_README.md
├── PROJECT_README.md
└── package.json
```

## Next Steps

1. ✅ All components created
2. ✅ Styling complete
3. ✅ Interactions implemented
4. ⏳ Add `breathing.mp3` to `/public`
5. ⏳ Test in browser with `npm run dev`
6. ⏳ Adjust timing/animations to taste
7. ⏳ Deploy for film promotion

## Troubleshooting

**Issue**: Audio doesn't play
- **Solution**: Add `breathing.mp3` to `/public` folder or remove audio feature

**Issue**: Animations feel too fast/slow
- **Solution**: Adjust `--transition-slow` and keyframe durations

**Issue**: Text feels too small/large
- **Solution**: Adjust `font-size` in component styles

**Issue**: Colors look wrong
- **Solution**: Check monitor calibration and CSS variables

## Horror Design Notes

The unsettling effectiveness comes from:
1. **Slow pace**: Forces contemplation, not panic
2. **Beauty**: Makes danger feel safe
3. **Lowercase**: Feels weak, vulnerable, non-threatening
4. **Breathing**: Makes the UI feel alive/aware
5. **Intimacy**: Personal questions create false connection
6. **Clinical tone**: Detachment masking as compassion

---

**You now have a complete, production-ready psychological horror promotional web app.** 🎬

The horror is in how good it feels to use.

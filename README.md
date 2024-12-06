# Fretboard

http://ryanbriones.com/fretproto

Placeholder public repository for Fretboard app.

<img src="https://raw.githubusercontent.com/ryanbriones/fretboard/refs/heads/main/fretproto-desktop.png" width="250" alt="Fretboard App Desktop" />

<img src="https://raw.githubusercontent.com/ryanbriones/fretboard/refs/heads/main/fretproto-mobile.png" width="250" alt="Fretboard App Responsive Mobile" />

## Secret Sauce

This is the core of how the application calculates chords from scales. The rest of the code is glue for UI.

```javascript
// https://github.com/tonaljs/tonal
import { Scale, Chord, Collection } from "tonal";

const cMajorScale = Scale.get("C major");
const cMajorScaleRotations = cMajorScale.notes.map((_, i, originalScaleNotes) => Collection.rotate(i, originalScaleNotes));
const cMajorTriads = cMajorScaleRotations.map((scaleRotation, i) => {
  const chordNotes = [
    scaleRotation[0], // root,
    scaleRotation[2], // third,
    scaleRotation[4] // fifth
  ];

  return [
    Chord.detect(chordNotes),
    chordNotes
  ]
});

console.log("Diatonic Triads of the C Major Scale");
cMajorTriads.forEach(([chordName, chordNotes]) => console.log(chordName, chordNotes));
// Diatonic Triads of the C Major Scale
// [ 'CM', 'Em#5/C' ] [ 'C', 'E', 'G' ]
// [ 'Dm' ] [ 'D', 'F', 'A' ]
// [ 'Em' ] [ 'E', 'G', 'B' ]
// [ 'FM', 'Am#5/F' ] [ 'F', 'A', 'C' ]
// [ 'GM', 'Bm#5/G' ] [ 'G', 'B', 'D' ]
// [ 'Am' ] [ 'A', 'C', 'E' ]
// [ 'Bdim' ] [ 'B', 'D', 'F' ]

const cMajor7Chords = cMajorScaleRotations.map((scaleRotation, i) => {
  const chordNotes = [
    scaleRotation[0], // root,
    scaleRotation[2], // third,
    scaleRotation[4], // fifth
    scaleRotation[6] // add seventh
  ];

  return [
    Chord.detect(chordNotes),
    chordNotes
  ]
});

console.log("Diatonic 7 Chords of the C Major Scale");
cMajor7Chords.forEach(([chordName, chordNotes]) => console.log(chordName, chordNotes));
// Diatonic 7 Chords of the C Major Scale
// [ 'Cmaj7' ] [ 'C', 'E', 'G', 'B' ]
// [ 'Dm7', 'F6/D' ] [ 'D', 'F', 'A', 'C' ]
// [ 'Em7', 'G6/E' ] [ 'E', 'G', 'B', 'D' ]
// [ 'Fmaj7' ] [ 'F', 'A', 'C', 'E' ]
// [ 'G7' ] [ 'G', 'B', 'D', 'F' ]
// [ 'Am7', 'C6/A' ] [ 'A', 'C', 'E', 'G' ]
// [ 'Bm7b5', 'Dm6/B' ] [ 'B', 'D', 'F', 'A' ]

```
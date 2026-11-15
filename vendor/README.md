# Third-party files

These load only when you tap **Record a riff**.

| File | What it is | License |
|---|---|---|
| `basicpitch.min.js` | Spotify Basic Pitch 1.0.1 (`@spotify/basic-pitch`) bundled with TensorFlow.js 3.21 into one browser script that exposes `window.BasicPitchLib`. | Apache-2.0 (both) |
| `basic-pitch-model/` | Basic Pitch model weights (ICASSP 2022), copied unchanged from the npm package. | Apache-2.0 |
| `LICENSE-basic-pitch.txt` | Apache License 2.0 text, which covers both projects above. | |

Rebuild the bundle:

```sh
npm i @spotify/basic-pitch@1.0.1 esbuild
# bp-entry.js:
#   import * as tf from '@tensorflow/tfjs';
#   import { BasicPitch, outputToNotesPoly, addPitchBendsToNoteEvents, noteFramesToTime } from '@spotify/basic-pitch';
#   window.BasicPitchLib = { tf, BasicPitch, outputToNotesPoly, addPitchBendsToNoteEvents, noteFramesToTime };
npx esbuild bp-entry.js --bundle --minify --format=iife --platform=browser --target=es2019 \
  --define:process.env.NODE_ENV='"production"' --define:global=window --outfile=basicpitch.min.js
```

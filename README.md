# MP3 Frame Counter API (TypeScript + Express)

Express endpoint that accepts an MP3 upload and returns the number of MPEG Version 1 Layer III frames.

Features:
- TypeScript, Express, Multer
- No MP3 parsing packages
- Counts MPEG1 Layer3 frames by header scan and frame-size hop
- JSON response: { "frameCount": number }

## Prerequisites
- Node.js 18+

## Setup
1) Install dependencies

   npm install
   npm i -D @types/node

2) Start in dev mode

   npm run dev

   Or build and run:

   npm run build
   npm start

## API
- POST /file-upload
  - Form field name: file (multipart/form-data)
  - Response: { "frameCount": number }

Example (curl):

  curl -X POST \
    -F "file=@/absolute/path/to/audio.mp3" \
    http://localhost:3000/file-upload

Response:

  { "frameCount": 4182 }

## Notes
- Minimal frame parser for MPEG Version 1 Layer III
- Frame length: floor(144 * bitrate / sampleRate) + padding
- Byte-wise advance on invalid headers
- Out of scope: other MPEG versions/layers

## Error Handling
- 400: missing file
- 500: I/O or parsing errors
- Temporary file removed after processing

## Manual Test Steps
1) Start server (npm run dev)
2) Upload MP3 via curl (see example)
3) Verify JSON response

## Optional test script
Script: scripts/test-upload.sh

Usage:

  bash scripts/test-upload.sh http://localhost:3000 ./path/to/file.mp3

## Project Scripts
- npm run dev: ts-node-dev (watch)
- npm run build: compile to dist
- npm start: run compiled server

## File Structure
- src/index.ts: Express server and frame counter
- uploads_tmp/: temporary upload directory

## License
MIT

## Testing
Run integration tests:

  npm test

Tests cover:
- Valid MP3 file processing
- Missing file error handling
- File size limit validation

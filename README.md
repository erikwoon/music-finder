# Music Finder

Android app for natural-language music discovery. Describe the kind of music
you're after, get artist and song recommendations from an LLM, and load them
straight into a new Spotify playlist.

## How it works

1. Sign in / register (local Room database for accounts, history per user).
2. Type a free-text query (mood, genre, vibe, similar artist, etc.).
3. The query is sent to OpenAI's completions API (`gpt-3.5-turbo-instruct`),
   which returns a pipe-delimited list of artist or song suggestions.
4. Pick a recommended artist to get song suggestions, or go straight to songs.
5. Songs are looked up via the Spotify Web API, added to a new playlist, and
   played back through the Spotify App Remote SDK (requires the Spotify app
   installed and the user logged in).

## Stack

- Java, Android SDK (minSdk 31, target/compile 34)
- Spotify Android Auth + App Remote SDK (local `.aar`, in `app-remote-lib/`)
- OkHttp for REST calls (OpenAI completions, Spotify Web API)
- Room for local user/history persistence
- Gson for JSON (de)serialization

## Project layout

- `app/` — the application module (activities, fragments, Room database, API
  clients)
- `app-remote-lib/` — vendored Spotify App Remote SDK
- `auth-lib/`, `spotify-auth/` — Spotify authentication modules

## Setup

Requires an OpenAI API key and a Spotify app registered in the
[Spotify Developer Dashboard](https://developer.spotify.com/dashboard) (redirect
URI `com.example.musicfinder://callback`).

Add your OpenAI key to a gitignored `local.properties` at the repo root:

```properties
OPENAI_API_KEY=sk-...
```

(or set the `OPENAI_API_KEY` environment variable — both are read in
`app/build.gradle.kts` and never committed).

Then open the project in Android Studio, or build from the CLI:

```
./gradlew assembleDebug
```

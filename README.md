# Video Overlay — Claude Code Plugin

A Claude Code plugin that takes vertical talking-head footage and produces 3 polished video variations with animated overlay graphics — ready for TikTok, Instagram Reels, and YouTube Shorts.

Each output is a 1080x1920 video where animated graphics fill the top half and the speaker appears in the bottom half, synced to the transcript.

## What It Does

1. **Transcribes** your raw footage using OpenAI Whisper
2. **Researches** the topics discussed to create accurate, informative visuals
3. **Designs** 3 visually distinct overlay approaches (editorial infographic, dark blueprint, bold magazine, etc.)
4. **Builds** a Remotion project with all 3 versions as React components
5. **Renders** 3 final MP4s composited with your original footage

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- **Node.js 18+** (for Remotion)
- **Python 3** with `openai-whisper` (`pip install openai-whisper`)
- **ffmpeg** and **ffprobe** (for video analysis and rendering)

## Installation

### Option A: Install as a plugin (recommended)

From inside Claude Code:

```
/plugin install arjaythedev/video-overlay
```

This gives you the `/video-overlay:video-overlay` slash command.

### Option B: Manual install

Clone the repo and copy the skill into your personal skills directory:

```bash
git clone https://github.com/arjaythedev/video-overlay.git
cp -r video-overlay/skills/video-overlay ~/.claude/skills/video-overlay
```

This gives you `/video-overlay` as a slash command.

### Companion skills (required)

This skill depends on two other skills for the Remotion animation layer. Install them the same way — as plugins or by copying into `~/.claude/skills/`:

| Skill | Purpose |
|-------|---------|
| **remotion-video-creator** | Scene design, animation patterns, and TransitionSeries timing for the 1080x1080 overlay content |
| **remotion-best-practices** | Remotion API reference — 31 rule files covering animations, audio, video, captions, fonts, transitions, and more |

### Verify prerequisites

```bash
node --version    # Should be 18+
python3 -c "import whisper; print('Whisper OK')"
ffmpeg -version
```

## Usage

### 1. Create a directory with your raw footage

```bash
mkdir my-video-project
cp ~/path/to/your-footage.MOV my-video-project/
cd my-video-project
```

Your footage should be vertical (9:16) talking-head video — the kind you'd post as a Reel or TikTok.

### 2. Invoke the skill

Open Claude Code in your project directory and ask for overlays:

```
> Add animated overlays to your-footage.MOV
```

Or use the slash command directly:

```
> /video-overlay
```

Claude walks through the full pipeline: transcribe, research the content, design 3 visual approaches, build the Remotion project, and render all 3 versions.

### 3. Review the alterations

Once rendering completes, you'll find 3 final videos in `overlay/out/`:

```
overlay/out/
├── final_v1.mp4    # e.g., Editorial Infographic style
├── final_v2.mp4    # e.g., Dark Blueprint style
└── final_v3.mp4    # e.g., Bold Magazine style
```

Preview them in the Remotion studio to scrub frame-by-frame:

```bash
cd overlay && npm start
```

### 4. Pick your favorite or make more

- **Happy with one?** Post it.
- **Want tweaks?** Ask Claude to adjust colors, timing, text, or layout on any version.
- **Want more variations?** Ask Claude to generate additional versions with different visual approaches.

## How It Works

When the skill runs, it creates an `overlay/` Remotion project in your directory:

```
overlay/
├── public/
│   └── raw.MOV              # Your footage (copied here for Remotion)
├── src/
│   ├── index.ts              # Remotion entry point
│   ├── Root.tsx              # 6 compositions (3 overlay + 3 final)
│   ├── theme.ts              # Design system (colors, fonts, timing)
│   ├── transcript.ts         # Timestamped segments from Whisper
│   ├── FinalComposition.tsx  # Composites video + overlay
│   ├── v1/                   # Version 1 overlay
│   │   ├── MainVideoV1.tsx
│   │   └── scenes/
│   ├── v2/                   # Version 2 overlay
│   │   ├── MainVideoV2.tsx
│   │   └── scenes/
│   └── v3/                   # Version 3 overlay
│       ├── MainVideoV3.tsx
│       └── scenes/
├── out/                      # Rendered final videos
├── package.json
└── tsconfig.json
```

## Design System

The overlays follow a clean, investor-grade aesthetic — think Sequoia pitch deck meets *The Economist* data visualization:

- **Color palette**: Charcoal-navy hero sections, clean off-white content, blue/teal accents
- **Typography**: DM Sans for headings, Inter for body text
- **Animations**: Smooth easing curves (no spring/bounce physics), staggered fade-in reveals
- **Mobile-first**: Minimum 36px body text, 56px+ headings, 100px+ hero numbers
- **Layout**: Top 20% keep-out zone (avoids platform UI overlap), generous whitespace

## License

MIT

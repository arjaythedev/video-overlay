# Video Overlay Skill for Claude Code

A Claude Code skill that takes vertical talking-head footage and produces 3 polished video variations with animated overlay graphics — ready for TikTok, Instagram Reels, and YouTube Shorts.

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

### 1. Install the skill and its dependencies

This skill depends on two other skills that handle the Remotion animation layer:

```bash
# Clone this repo
git clone https://github.com/arjaythedev/video-overlay.git

# Install video-overlay skill
claude skill install /path/to/video-overlay

# Install required companion skills
claude skill install remotion-video-creator
claude skill install remotion-best-practices
```

> **Note:** `remotion-video-creator` handles the scene design and animation patterns for the 1080x1080 overlay content. `remotion-best-practices` provides the Remotion API reference (animations, transitions, fonts, captions, etc.). Both are required — the video-overlay skill references them during the build phase.

### 2. Verify prerequisites

```bash
node --version    # Should be 18+
python3 -c "import whisper; print('Whisper OK')"
ffmpeg -version
```

## Usage

### Step 1: Create a project directory with your raw footage

```bash
mkdir my-video-project
cp ~/path/to/your-footage.MOV my-video-project/
cd my-video-project
```

Your footage should be vertical (9:16) talking-head video — the kind you'd post as a Reel or TikTok.

### Step 2: Invoke the skill

Open Claude Code in your project directory and tell it to create overlays:

```
claude

> Add animated overlays to your-footage.MOV
```

Or use the slash command directly:

```
> /video-overlay
```

Claude will walk through the full pipeline: transcribe, research the content, design 3 visual approaches, build the Remotion project, and render all 3 versions.

### Step 3: Review the output

Once rendering completes, you'll find 3 final videos in `overlay/out/`:

```
overlay/out/
├── final_v1.mp4    # e.g., Editorial Infographic style
├── final_v2.mp4    # e.g., Dark Blueprint style
└── final_v3.mp4    # e.g., Bold Magazine style
```

You can also preview them in the Remotion studio:

```bash
cd overlay
npm start
```

This opens a browser where you can scrub through each version frame-by-frame.

### Step 4: Pick your favorite or iterate

- **Happy with one?** Post it directly.
- **Want tweaks?** Ask Claude to adjust colors, timing, text, or layout on any version.
- **Want more variations?** Ask Claude to create additional versions with different visual approaches.

## Project Structure

When the skill runs, it creates an `overlay/` directory in your project:

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

## Companion Skills

| Skill | Purpose |
|-------|---------|
| **remotion-video-creator** | Scene design, animation patterns, and TransitionSeries timing for the 1080x1080 overlay content |
| **remotion-best-practices** | Remotion API reference — 31 rule files covering animations, audio, video, captions, fonts, transitions, and more |

## License

MIT

# OpenMontage Reusable Production Guide (Zero-Cost Local Studio)

This guide documents the reusable, $0-budget video production workflows for OpenMontage on Windows 11. It covers three production styles using 100% free and open-source tools without paid APIs or GPU-heavy models.

---

## 1. Supported Production Styles

```
+-----------------------------------------------------------------------------------------+
|                                    OPENMONTAGE STUDIO                                   |
+----------------------------+----------------------------+-------------------------------+
| 1. CINEMATIC               | 2. EDUCATIONAL             | 3. SHORT-FORM (9:16)          |
| Footage-led documentary   | Motion-graphics explainer  | Vertical Shorts / Reels       |
| 16:9 or 21:9 letterboxed   | 1920x1080 (16:9)           | 1080x1920 (9:16)              |
| Open stock footage         | React visual components    | Bold centralized typography   |
| Atmospheric drone bed      | Technical diagrams/charts  | High-impact TikTok captions   |
+----------------------------+----------------------------+-------------------------------+
```

### Style 1: CINEMATIC
* **Best for**: History, nature, philosophy, brand stories, mood-driven documentaries.
* **Aspect Ratio**: 16:9 (`1920x1080`) or 21:9 (`1920x816` letterboxed).
* **Footage Source**: Open archives via `direct_clip_search` (`archive_org`, `wikimedia`, `nasa`, `coverr`, `nara`, `loc`, `pond5_pd`).
* **Narration**: Offline Piper TTS with contemplative, measured cadence (`length_scale: 1.08 - 1.15`).
* **Audio**: Deep harmonic ambient pads (locally synthesized via NumPy) or CC0 acoustic/drone tracks.
* **Subtitles**: Subtle lower-third subtitles.

### Style 2: EDUCATIONAL
* **Best for**: Science explainers, robotics, engineering, software architectures, data breakdowns.
* **Aspect Ratio**: 16:9 (`1920x1080`).
* **Visual Stack**: Remotion scene library (`HeroTitle`, `ComparisonCard`, `CalloutBox`, `KPIGrid`, `BarChart`, `ProgressBar`, `TextCard`) + Pillow/Mermaid technical diagrams.
* **Narration**: Offline Piper TTS with clear technical delivery (`length_scale: 1.00 - 1.02`).
* **Audio**: Soft ambient chord bed or pure voice-only.
* **Subtitles**: Dynamic word-level highlight captions (`CaptionOverlay`).

### Style 3: SHORT-FORM (Vertical 9:16)
* **Best for**: YouTube Shorts, Instagram Reels, TikTok, fast tips, viral tech facts.
* **Aspect Ratio**: 9:16 (`1080x1920`).
* **Visual Stack**: High-contrast vertical Remotion layout, rapid spring card reveals, upper-third headers, centered focus.
* **Narration**: Offline Piper TTS with punchy, energetic pacing (`length_scale: 0.90 - 0.96`).
* **Audio**: Energetic synth bed or voice-first punchy mix.
* **Subtitles**: Large, bold word-by-word highlighted captions (`fontSize: 54-60`, `wordsPerPage: 3`).

---

## 2. Reusable Production Lifecycle

Every production follows a strict 5-step state machine with human verification:

```
[1. User Prompt] ───> [2. Script & Storyboard] ───> [3. HUMAN APPROVAL GATE]
                                                              │
                                                        (User Approves)
                                                              │
[6. Delivery & ffprobe] <─── [5. Remotion + FFmpeg Render] <──┴─ [4. Asset & Voice Generation]
```

1. **Brief & Preflight**: The agent parses duration, style, and aspect ratio, verifying all tools are available at $0 budget.
2. **Script & Scene Plan**: The agent drafts timestamped narration sections and a visual storyboard, validating them against OpenMontage JSON schemas.
3. **Approval Gate (MANDATORY)**: The agent pauses and presents the script, visual plan, and cost ($0.00) for your review.
4. **Offline Asset Generation**:
   - Voice narration synthesized via `piper_tts` (`en_US-lessac-medium`).
   - Ambient soundbed synthesized locally with NumPy/SciPy.
   - Word timestamps extracted for caption sync.
5. **Remotion Composition & Render**: Remotion compiles the React components and burns captions using headless Chrome and FFmpeg into H.264/AAC MP4.
6. **Technical Verification**: Video validated via `ffprobe` (resolution, duration, FPS, audio channels, bit rate).

---

## 3. Ready-To-Use Example Prompts

Copy and paste these exact prompts to create videos in any of the three styles:

### Prompt A: Cinematic Documentary (16:9)
```text
Create a CINEMATIC documentary video about "The Deep Sea: Earth's Final Frontier".
- Duration: 45 seconds
- Format: 1920x1080 (16:9)
- Style: Cinematic, atmospheric, contemplative
- Footage: Free open archive footage (NASA / Wikimedia / Archive.org)
- Voice: Offline Piper TTS (measured, documentary tone)
- Audio: Locally synthesized warm ambient drone pad
- Captions: Subtle lower-third burned-in subtitles
- Budget: Strictly $0

Show me the script and scene plan for approval before rendering.
```

### Prompt B: Educational Explainer (16:9)
```text
Create an EDUCATIONAL explainer video about "How Large Language Models Predict the Next Word".
- Duration: 50 seconds
- Format: 1920x1080 (16:9)
- Style: Clean professional educational explainer
- Visuals: Hero title, comparison cards, neural network attention callout, illustrative token telemetry grid
- Voice: Offline Piper TTS (clear, engaging technical delivery)
- Audio: Soft ambient background pad
- Captions: Word-level synchronized burned-in subtitles
- Budget: Strictly $0

Show me the script and scene plan for approval before rendering.
```

### Prompt C: Short-Form Reel / Short (9:16 Vertical)
```text
Create a SHORT-FORM vertical video about "3 Mind-Blowing Facts About Quantum Computers".
- Duration: 30 seconds
- Format: 1080x1920 (9:16 Vertical for YouTube Shorts / Reels)
- Style: High-energy, punchy, modern
- Visuals: Bold vertical typography, rapid stat cards, animated highlight boxes
- Voice: Offline Piper TTS (energetic, fast pacing)
- Audio: Punchy local audio bed
- Captions: Large TikTok-style word-by-word highlighted captions
- Budget: Strictly $0

Show me the script and scene plan for approval before rendering.
```

---

## 4. Configuration Matrix

| Setting | Cinematic | Educational | Short-Form (9:16) |
| :--- | :--- | :--- | :--- |
| **Resolution** | `1920x1080` (or `1920x816`) | `1920x1080` | `1080x1920` |
| **Aspect Ratio** | `16:9` / `21:9` | `16:9` | `9:16` |
| **Composition ID** | `Explainer` / `CinematicRenderer` | `Explainer` | `Explainer` (`aspectRatio: "9:16"`) or `TalkingHead` |
| **Theme** | `clean-professional` / `flat-motion-graphics` | `clean-professional` | `flat-motion-graphics` |
| **Voice Speed** | `length_scale: 1.08 - 1.15` | `length_scale: 1.00 - 1.02` | `length_scale: 0.90 - 0.96` |
| **Caption Style** | Subtle phrase / lower-third | Word-level highlight (`fontSize: 42`) | Bold TikTok highlight (`fontSize: 54-58`, 3 words/page) |
| **Audio Mix** | Drone pad ($-22\text{ dBFS}$) | Soft chord bed ($-20\text{ dBFS}$) | Punchy bed ($-18\text{ dBFS}$) or Voice-only |
| **API Cost** | **$0.00** | **$0.00** | **$0.00** |

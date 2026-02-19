# Bellweather Protocol Audio Safety & Voice Integration Specification

**Updated:** 2026-02-15  
**Purpose:** Ensure all audio output is safe for human hearing and includes voice integration for coordination

---

## 1. Audio Safety Requirements (CRITICAL)

### 1.1 Volume Normalization
All audio output MUST be normalized to safe listening levels:

- **Peak amplitude:** -3 dBFS (maximum)
- **RMS level:** -18 to -12 dBFS (target range)
- **No sudden spikes:** All level changes must be smoothed (10ms attack/release)
- **Compression:** Apply light compression (ratio 3:1, threshold -18dB, soft knee)

### 1.2 Implementation
Apply normalization at MULTIPLE stages:

1. **During generation:** Each audio chunk generated is pre-normalized
2. **During playback:** Apply real-time limiter as safety net
3. **During mixing:** When combining multiple audio sources (beat + voice), maintain headroom

**Python libraries to use:**
- `pydub` for audio processing and normalization
- `pyloudnorm` for perceptual loudness (EBU R128 standard)
- `sounddevice` with volume limiting for playback

### 1.3 Safety Checks
Before any audio playback:
```python
def safety_check(audio_array):
    """Return True if audio is safe, False otherwise."""
    peak = np.abs(audio_array).max()
    if peak > 0.9:  # Peak exceeds -1dBFS
        return False
    rms = np.sqrt(np.mean(audio_array**2))
    if rms > 0.5:  # RMS too loud (~ -6dBFS)
        return False
    return True
```

---

## 2. Background Rhythm (No Silence)

### 2.1 Requirements
The system should NEVER fall into complete silence during coordination:

- **During active work:** Full shanty with voice and beats
- **During idle/downtime:** Gentle pulsing rhythm (ambient heartbeat)
- **During transitions:** Smooth crossfade (500ms minimum)

### 2.2 Idle State Audio
When no agents are coordinating (idle state):

**Ambient heartbeat pattern:**
- Soft kick drum or bass pulse
- Tempo: 60-80 BPM (resting heart rate)
- Volume: -30 to -24 dBFS (quiet but audible)
- Timbre: Warm, organic, non-intrusive
- Pattern: Simple 4/4 pulse or slow waltz (3/4)

**Implementation:**
```python
def generate_idle_pulse(bpm=70, duration_sec=10):
    """
    Generate gentle pulsing rhythm for idle state.
    Returns audio array normalized to -30dBFS.
    """
    # Simple sine wave pulse (like a heartbeat)
    # Frequency: 60-80 Hz (bass range)
    # Attack: 50ms, Decay: 200ms, Sustain: 0, Release: 100ms
    pass
```

### 2.3 State Transitions
Audio should smoothly transition between states:

- **Idle → Active:** 500ms crossfade, gradually increase volume
- **Active → Idle:** 1000ms crossfade, decrease to background level
- **Pause → Resume:** No fade, instant continue
- **Error/Stop:** 250ms fade to silence (emergency only)

---

## 3. ElevenLabs Voice Integration

### 3.1 Purpose
Enable agents to "call out" coordination events using ElevenLabs TTS during shanty execution.

**Use cases:**
- Agent announces task start: "Starting fetch on URL batch alpha"
- Agent confirms completion: "Batch alpha complete, 47 URLs processed"
- Agent reports error: "Collision detected on resource delta-3, resyncing"
- Shantyman calls verse: "Ready the lines!" (before next verse)

### 3.2 Voice Configuration
Use Johnny-C voice profile:
- **Voice ID:** `q5kxCmre0DdtCaMuwod8`
- **Stability:** 0.5 (balanced expressiveness for clarity)
- **Similarity boost:** 0.9 (stay close to voice clone)
- **Style:** 0.3 (add slight dramatic energy)

### 3.3 Integration Points

#### 3.3.1 During ShantyRunner execution
Add voice call hooks to runner:

```python
class ShantyRunner:
    def __init__(self, ..., enable_voice_calls=False, 
                 elevenlabs_key=None):
        self.enable_voice_calls = enable_voice_calls
        self.elevenlabs_key = elevenlabs_key
        self.voice_queue = queue.Queue()  # Async voice generation
    
    def call_out(self, text: str, priority="normal"):
        """
        Queue a voice callout to be played during next beat sync.
        Priority: "urgent" (plays immediately), "normal" (queues)
        """
        if not self.enable_voice_calls:
            return
        
        # Generate audio async
        audio = self._generate_voice_async(text)
        self.voice_queue.put((priority, audio))
    
    def _fire_beat(self, event: BeatEvent):
        # Play any queued voice calls on sync beats
        if event.is_sync_beat and not self.voice_queue.empty():
            priority, audio = self.voice_queue.get()
            self._play_voice_overlay(audio)
        
        # Continue with beat callbacks...
```

#### 3.3.2 Voice + Music Mixing
When playing voice over shanty music:

1. **Duck the music:** Reduce music volume by -12dB during voice
2. **Voice level:** -6 dBFS (clear and prominent)
3. **Crossfade:** 100ms fade down, 200ms fade up
4. **Compression:** Apply sidechain compression (music compressed by voice presence)

```python
def mix_voice_over_music(music_audio, voice_audio, 
                         duck_db=-12, crossfade_ms=100):
    """
    Mix voice over music with ducking.
    Returns mixed audio normalized to -3dBFS peak.
    """
    # Apply ducking envelope to music where voice is present
    # Normalize final mix
    pass
```

### 3.4 Voice Call Timing
Coordinate voice calls with shanty rhythm:

- **Verse start:** Shantyman can call out task summary
- **Verse end:** Crew can respond with status
- **Sync beats (every 4th beat):** Short status calls allowed
- **Between verses:** Longer announcements OK (up to 8 seconds)

**Rule:** Never interrupt mid-verse. Queue calls for next sync point.

### 3.5 Caching & Performance
Generate voice audio AHEAD of time when possible:

```python
class VoiceCache:
    """Pre-generate common voice callouts."""
    
    COMMON_CALLS = [
        "Ready the lines!",
        "Task complete!",
        "Standing by",
        "Heave ho!",
        "Collision detected",
        "Resyncing now",
    ]
    
    def __init__(self, elevenlabs_key):
        self.cache = {}
        self._pregenerate(COMMON_CALLS)
    
    def get(self, text: str) -> bytes:
        """Get voice audio, from cache or generate on-demand."""
        if text in self.cache:
            return self.cache[text]
        return self._generate_and_cache(text)
```

Pre-generate at startup to avoid latency during coordination.

---

## 4. Monitoring & Debugging

### 4.1 Audio Meters
Display real-time audio levels during playback:

```
[=============|---] -12dB MUSIC
[=====|---------] -24dB VOICE
[==============|-] -3dB MASTER (SAFE)
```

### 4.2 Safety Violations Log
Log any safety violations:
```python
logger.warning(f"Audio peak exceeded -1dBFS: {peak_db:.1f}dB")
logger.error(f"RMS level dangerous: {rms_db:.1f}dB (normalized)")
```

### 4.3 Testing
Create test harness:
```bash
$ python test_audio_safety.py
✓ Peak normalization (-3dBFS)
✓ RMS level safe (-15dBFS)
✓ No sudden spikes detected
✓ Idle pulse generated
✓ Voice mixing safe (-6dBFS)
✓ All transitions smooth
```

---

## 5. Implementation Checklist

- [ ] Add `pydub`, `pyloudnorm`, `sounddevice` to requirements.txt
- [ ] Implement `normalize_audio()` function with -3dBFS peak limit
- [ ] Implement `generate_idle_pulse()` for background rhythm
- [ ] Add volume meters to runner output
- [ ] Implement `VoiceCache` for pre-generated callouts
- [ ] Add `call_out()` method to ShantyRunner
- [ ] Implement voice + music mixing with ducking
- [ ] Add crossfade transitions between all states
- [ ] Create `test_audio_safety.py` test suite
- [ ] Update all demo scripts to use safe audio settings
- [ ] Add safety checks before ALL audio output
- [ ] Document voice call API in README

---

## 6. Example Usage

```python
from swarmsync import ShantyRunner, VoiceCache
from swarmsync.audio_safety import normalize_audio, generate_idle_pulse

# Create runner with voice enabled
runner = ShantyRunner(
    protocol,
    enable_voice_calls=True,
    elevenlabs_key="sk_...",
    audio_safety=True  # Enable all safety features
)

# Pre-cache common voice calls
voice_cache = VoiceCache(elevenlabs_key="sk_...")

# During execution, agent can call out
@runner.on_instruction
def handle_instruction(event):
    runner.call_out(f"Starting {event.instruction.action}")
    # ... do work ...
    runner.call_out("Task complete!")

# Background rhythm plays during idle
runner.enable_idle_pulse(bpm=70)

runner.run()
```

---

## Safety Philosophy

**"Better quiet than harmful."**

If in doubt, err on the side of:
- Lower volume
- More compression
- More limiting
- Smoother transitions

Never sacrifice hearing safety for audio quality. A slightly over-compressed shanty is infinitely better than damaged hearing.

---

**Document Version:** 1.0  
**Author:** Johnny-C  
**Review:** Skipper-approved safety requirements  
**Next Review:** After initial implementation testing

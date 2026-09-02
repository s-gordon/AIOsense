# Audio: media player & voice assistant

AIOsense can do two different audio things, and you pick **one**:

| You want | Enable | Gives you |
|----------|--------|-----------|
| Play TTS and notification sounds from Home Assistant | `media_player.yaml` | A `media_player` entity. No microphone. |
| Talk to the device and get answers | `voice_assistant-led.yaml` or `voice_assistant-rgb_led.yaml` | Microphone, speaker, wake word |

They are mutually exclusive. Both declare the same underlying components, so
enabling both fails to build.

If all you want is a doorbell chime, a laundry-finished announcement or TTS from
an automation, **you want the media player**, not the voice assistant. It is
simpler, uses less flash and does not put a live microphone in the room.

## Board support

Audio needs an I2S microphone and/or amplifier, which needs a board that breaks
out the I2S data pins. Not every CPU option can:

| Board | Audio? | LED package | Voice assistant package |
|-------|--------|-------------|--------------------------|
| **ESP32-C3 mini** | No | `rgb_led.yaml` | — |
| **ESP32-S2 mini** | Yes | `led.yaml` / `status_led.yaml` | `voice_assistant-led.yaml` |
| **ESP32-S3 mini** | Yes | `rgb_led.yaml` | `voice_assistant-rgb_led.yaml` |

The C3 mini defines no `i2s_din_pin` or `i2s_dout_pin`, so it cannot drive a
microphone or a speaker at all. It is a sensor-only board.

The pairing between LED and voice assistant package is not a style choice. Each
voice assistant package drives a light with `id: led` and asks for effects by
name, and only one LED package exists per board: the S2 is the only board
defining `${led_pin}`, and the S3 the only audio-capable board defining
`${rgb_led_pin}`.

## What each package gives you

### `media_player.yaml`

Speaker only. Home Assistant gets a `media_player` entity you can target with
TTS, `chime_tts`, Node-RED, or anything else that plays to a media player. The
LED pulses while an announcement plays.

Only an *announcement* pipeline is configured, so announcements cannot duck
background music -- there is no background stream. See
[Limitations](#limitations).

### `voice_assistant-rgb_led.yaml` (S3 mini)

The complete stack: microphone, speaker, a `media_player` entity **and**
on-device wake word. Because it declares its own media player, you get TTS and
voice control together.

Wake word is chosen at runtime through the **Wake word engine location** select:

- **In Home Assistant** -- the device streams audio to HA continuously and HA
  detects the wake word. Reliable, but the radio transmits constantly, which is
  the main reason these devices run warm.
- **On device** -- `micro_wake_word` runs locally and only opens a stream once a
  wake word fires. Much quieter on the network and usually cooler, at the cost
  of local CPU. Ships with `alexa` and `hey_jarvis`.
- **Disable** -- neither. This is the default, so a freshly flashed device does
  not stream audio anywhere until you ask it to.

### `voice_assistant-led.yaml` (S2 mini)

Microphone, speaker and voice control, paired with the single-colour LED. Two
things it does **not** have:

- **No `media_player` entity.** The assistant talks to the speaker directly, so
  Home Assistant has nothing to send TTS to. On the S2, voice control and
  `chime_tts` are an either/or -- see [Limitations](#limitations).
- **No on-device wake word.** `micro_wake_word` crashes on the S2 mini, so the
  selector offers only *In Home Assistant* and *Disable*.

## Enabling it

Uncomment the package you want in your device config, alongside a matching LED
package. An S3 mini with full voice control:

```yaml
packages:
  remote_package:
    url: https://github.com/s-gordon/AIOsense
    ref: aio-v2.0.0
    refresh: never
    files:
      - esphome/packages/config/base.yaml
      - esphome/packages/config/esp32-s3-mini.yaml

      - esphome/packages/sensors/voice_assistant-rgb_led.yaml
      - esphome/packages/sensors/rgb_led.yaml
```

An S2 mini that only needs to play announcements:

```yaml
      - esphome/packages/config/esp32-s2-mini.yaml

      - esphome/packages/sensors/media_player.yaml
      - esphome/packages/sensors/led.yaml
```

Then add the device to Home Assistant and, for voice control, assign it a voice
assistant pipeline under **Settings → Voice assistants**.

## Limitations

**Announcements cannot duck background media.** Only an announcement pipeline is
configured. Playing music and having a notification talk over it needs a second
pipeline plus mixer and resampler speakers -- see the
[speaker media player docs](https://esphome.io/components/media_player/speaker.html).

**On the S2, voice control and TTS are mutually exclusive.** `voice_assistant-led.yaml`
declares no media player, and you cannot add `media_player.yaml` alongside it.
If you need both on one device, use an S3 mini.

**On-device wake word is S3 only.** It crashes on the S2 mini.

## Troubleshooting

**No audio, or the microphone reads silence.** Check the wiring before the
config. On PCB v3.0.0-rc1 the I2S microphone footprint has **GND and VCC
swapped** ([issue #333](https://github.com/Schluggi/AIOsense/issues/333)),
acknowledged upstream but never fixed in that revision.

**The device runs warm while listening.** Expected to a degree -- continuous I2S
capture plus WiFi is real work. The largest single contributor is usually
*In Home Assistant* wake word mode, which streams audio nonstop. Switching to
*On device* removes that. `cpu_frequency: 160MHZ` is worth trying next.

**PSRAM errors, or wake word failing to load.** The audio packages declare
`psram: mode: quad`, which is correct for the 2 MB modules on the S2 and S3
mini. `octal` is for 8/16 MB modules and stops PSRAM working entirely.

**`Effect 'Fast Pulse' not found`.** The LED package you enabled does not define
the effect the audio package asks for. Use one of the LED packages listed in
[Board support](#board-support); each provides the effects its partner needs.

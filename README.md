# Bevy Kira audio

[![Crates.io](https://img.shields.io/crates/v/bevy_kira_audio.svg)](https://crates.io/crates/bevy_kira_audio)
[![docs](https://docs.rs/bevy_kira_audio/badge.svg)](https://docs.rs/bevy_kira_audio)
[![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/NiklasEi/bevy_kira_audio/blob/main/LICENSE.md)
[![Crates.io](https://img.shields.io/crates/d/bevy_kira_audio.svg)](https://crates.io/crates/bevy_kira_audio)

This bevy plugin is intended to try integrating [Kira][kira] into Bevy. The goal is to replace or update `bevy_audio`, if Kira turns out to be a good approach. Currently, this plugin can play `ogg`, `mp3`, `flac`, and `wav` formats and supports web builds for everything except `mp3`. It also supports streaming of generated audio.

You can check out the `examples` directory in this repository for a display of this plugin's functionality.

## Usage

*Note: `bevy_audio` is enabled by default and not compatible with this audio plugin. Make sure to not have the `bevy_audio` feature enabled if you want to use `bevy_kira_audio`. The same goes for Bevy's `mp3` feature.*

To initialize the corresponding `AssetLoaders`, use at least one of the features `bevy_kira_audio/ogg`, `bevy_kira_audio/mp3`, `bevy_kira_audio/wav`, or `bevy_kira_audio/flac`. The following example assumes that the feature `bevy_kira_audio/ogg` is enabled.

```rust
use bevy_kira_audio::{AudioChannel, Audio, AudioPlugin};
use bevy::prelude::*;

fn main() {
   let mut app = App::build();
   app
        .add_plugins(DefaultPlugins)
        .add_plugin(AudioPlugin)
        .add_startup_system(start_background_audio.system());
   app.run();
}

fn start_background_audio(asset_server: Res<AssetServer>, audio: Res<Audio>) {
    audio.play_looped(asset_server.load("background_audio.ogg"));
}
```

## Current state
- [x] play common audio formats
  - [x] `ogg`
  - [x] `mp3`
  - [x] `wav`
  - [x] `flac`
- [x] web support
  - The features `ogg`, `flac` and `wav` can be build for WASM and used in web builds. There are some differences between browsers:
    - Chrome requires an interaction with the website to play audio (e.g. a button click). This issue can be resolved with [a script][audio-context-resuming] in your `index.html` file ([usage example][bevy-game-template-audio-context-resuming]).
    - Firefox: The audio might sound distorted (this could be related to overall performance; see [issue #9][issue-9])
- [x] pause/resume and stop tracks
- [x] play a track on repeat
- [x] control volume
- [x] control playback rate
- [ ] control pitch (no change in playback rate)
- [x] control panning
- [ ] get the current status of a track (time elapsed/left)?
- [x] audio streaming

## Compatible Bevy versions

The main branch is up to date with the latest Bevy release. The branch `bevy_main` tracks the `main` branch of Bevy.

Compatibility of published `bevy_kira_audio` versions:
| `bevy_kira_audio` | `bevy` |
| :-- | :--  |
| `0.4` - `0.5` | `0.5` |
| `0.3` | `0.4` |

### License

Licensed under either of

* Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or https://www.apache.org/licenses/LICENSE-2.0)
* MIT license ([LICENSE-MIT](LICENSE-MIT) or https://opensource.org/licenses/MIT)

at your option.

Assets in the examples might be distributed under different terms. See the [readme](examples/README.md#credits) in the `examples` directory.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any
additional terms or conditions.



[kira]: https://github.com/tesselode/kira
[kira-license]: https://github.com/tesselode/kira/blob/main/license.md
[rodio]: https://github.com/RustAudio/rodio
[oicana]: https://github.com/NiklasEi/oicana
[oicana-audio]: https://github.com/NiklasEi/oicana/blob/master/crates/oicana_plugin/src/audio.rs
[issue-9]: https://github.com/NiklasEi/bevy_kira_audio/issues/9
[audio-context-resuming]: https://developers.google.com/web/updates/2018/11/web-audio-autoplay#moving-forward
[bevy-game-template-audio-context-resuming]: https://github.com/NiklasEi/bevy_game_template/blob/main/index.html#L27-L90

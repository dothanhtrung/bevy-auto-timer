<div align="center">

bevy-auto-timer
===============

[![crates.io](https://img.shields.io/crates/v/bevy-auto-timer)](https://crates.io/crates/bevy_auto_timer)
[![docs.rs](https://docs.rs/bevy_auto_timer/badge.svg)](https://docs.rs/bevy_auto_timer)
[![dependency status](https://deps.rs/crate/bevy_auto_timer/latest/status.svg)](https://deps.rs/crate/bevy_auto_timer/latest)
[![pipeline status](https://gitlab.com/245project/bevy-plugin/bevy-auto-timer/badges/master/pipeline.svg)](https://gitlab.com/245project/bevy-plugin/bevy-auto-timer/-/commits/master)


[![Gitlab](https://img.shields.io/badge/gitlab-%23181717.svg?style=for-the-badge&logo=gitlab&logoColor=white)](https://gitlab.com/245project/bevy-plugin/bevy-auto-timer)
[![Github](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)](https://github.com/dothanhtrung/bevy-auto-timer)

</div>

A convenient timer which ticks automatically.

Quickstart
----------

```rust
use bevy::prelude::*;
use bevy-auto-timer::{AutoTimer, AutoTimerFinished, AutoTimerPlugin};

#[derive(States, Debug, Clone, Copy, Eq, PartialEq, Hash, Default)]
enum GameState {
    #[default]
    Menu,
    InGame,
    EndGame,
}

fn main() {
    let mut app = App::new();
    app.add_plugins(DefaultPlugins).init_state::<GameState>();

    // Add this plugin and specify which state it will run in
    app.add_plugins(AutoTimerPlugin::new(vec![GameState::InGame, GameState::EndGame]));
    // or you can use `AutoTimerPluginAnyState` for running on any states.
    // app.add_plugins(AutoTimerPluginAnyState::any());

    app.add_systems(Startup, setup).run();
}

fn setup(mut commands: Commands, mut next_state: ResMut<NextState<GameState>>) {
    // Spawn timer as a component
    commands
        .spawn(AutoTimer::from_seconds(1., TimerMode::Repeating))
        .observe(timeout); // observe event `AutoTimerFinished`

    next_state.set(GameState::InGame);
}

fn timeout(_: On<AutoTimerFinished>) {
    info!("Timer finished!");
}
```

License
-------

Please see [LICENSE](./LICENSE).


Compatible Bevy Versions
------------------------

| bevy | bevy-auto-timer |
|------|-----------------|
| 0.19 | 0.4             |
| 0.18 | 0.3             |
| 0.17 | 0.2-0.3         |
| 0.16 | 0.1             |

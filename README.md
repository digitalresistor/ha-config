# ha-config

Home Assistant blueprints for my house. Most of them started as forks of
[zpriddy's Home Assistant config](https://github.com/zpriddy/Home-Assistant)
and have since been reworked to drive lights through Adaptive Lighting and to
recover cleanly from restarts.

## Blueprints

| Blueprint | Type | What it is for |
|---|---|---|
| [Motion-activated Light With Enable Switch and Modes](blueprints/motion_activated_lights.yml) | automation | Room lights on motion, only in the selected house modes and while the room's automation switch is on. |
| [Motion-activated Lights](blueprints/motion_activated_lights_always_on.yml) | automation | Same, but always runs, regardless of mode or an enable switch. |
| [Occupancy-activated Lights](blueprints/occupancy_lights.yml) | automation | Lights for spaces with several sensors of mixed types and doors, such as a garage: on while anything detects someone or a door is in use, and kept on while a door (rolling garage door) is open. |
| [Door-activated Lights](blueprints/door_lights.yml) | automation | Lights for closets without a motion sensor: on while the door is open, with a timeout in Away mode. |
| [Entry Light](blueprints/entry_light.yml) | automation | Entry light when someone comes in through a locked door, or as soon as they unlock it after arriving home in the dark. |
| [Bed Night Light](blueprints/bed_night_light.yml) | automation | Dim red light at night when someone gets out of bed, dimmer if someone else is still asleep, off once they are back in bed. Uses per-side bed presence sensors. |
| [Bathroom Fan](blueprints/bathroom_fan.yml) | automation | Runs the fan for toilet visits and showers (humidity spike) and makes sure it is never left running. |
| [Away Mode](blueprints/away_mode.yml) | automation | Switches the house mode between Home and Away from presence, motion and Guest/Partner toggles, and asks via push notification when it is unsure. |
| [Make It Dark](blueprints/make_it_dark.yaml) | script | Turns off an Adaptive Lighting area. In Night mode it first primes every light with the sleep color, so lights come back on dim and red instead of bright white. |
| [Set Adaptive Lighting](blueprints/set_adaptive_lighting.yaml) | script | Applies the same Adaptive Lighting settings to every instance. Run on Home Assistant start, because these settings do not survive a restart. |

Each blueprint's description (shown when importing it) explains its inputs in
more detail.

## Conventions

- **House mode.** Mode-aware blueprints read an `input_select` with the options
  Home, Night, Movie, Away and No Automation. Night is the sleep mode, Away is
  set by the Away Mode blueprint, and No Automation pauses mode changes.
- **Adaptive Lighting.** Light blueprints can take an Adaptive Lighting switch
  and turn on every light in that instance *by entity id*. Adaptive Lighting
  only adapts lights that are turned on by entity id, so targeting lights by
  device or area makes them come on in the wrong color. The instances need
  `include_config_in_attributes` enabled so their light list can be read; Set
  Adaptive Lighting keeps it on.
- **Restarts.** Automations recover their state on Home Assistant start and when
  automations are reloaded, so a restart never leaves lights or fans on. Turn-off
  times are worked out from timestamps rather than from a running timer.
- **Shared lists in Home Assistant, not here.** Which switches Night and Away
  turn off is set with the *Lights Off* label, and which fans Away turns off with
  the *Away: turn off* label.

## Using a blueprint

Import it in Home Assistant under **Settings → Automations & scenes →
Blueprints → Import blueprint** with the file's GitHub URL, for example
`https://github.com/digitalresistor/ha-config/blob/main/blueprints/away_mode.yml`.
The `source_url` in each blueprint points back at `main`, so re-importing picks
up the latest version.

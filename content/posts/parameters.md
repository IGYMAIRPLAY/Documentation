---
title: "Tunable Parameters"
---

Every configurable parameter in the iGYM system, grouped by what they affect. **Inspector** = editable in Unity Inspector. **Static** = global flag set at runtime.

---

## Ball Physics & Difficulty

In `gameMgr.cs`, applied by `ApplyMode()` when the server syncs `syncedMode`.

### Per-Mode Values

| Mode | `ballSpeed` | `ballRadius` | `goalSize` | `friction` |
|------|-------------|-------------|------------|-----------|
| Easy (0) | `0.15` | `4.0` | `2215` | `0.4` |
| Medium (1) | `0.50` | `2.0` | `2211` | `0.4` |
| Hard (2) | `0.75` | `1.0` | `2200` | `0.4` |

`goalSize` encodes four values: `1000×leftGoal + 100×leftExpansion + 10×rightGoal + rightExpansion`.

### Additional Physics Parameters

**`hockeyctr.cs`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `kickFactor` | `1.0` | Yes | Global multiplier on kick velocity |
| `startpoint` | Scene-set | Yes | Ball spawn position on reset |

**`gameMgr.cs`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `buttonSpeed` | `1.0` | No | Speed multiplier for button kicks |

**`PlayerControl.cs`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `kickForce` | `10.0` | Yes | Base kick force magnitude |
| `kickFactorPreset` | `[1.0, 2.0]` | Yes | Per-preset kick multipliers |
| `localScalePreset` | `[0.7×, 1.0×]` | Yes | Paddle scale per preset |

---

## Client-Side Prediction

All in `hockeyctr.cs`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `reconcileSnapThreshold` | `0.05` units | Yes | Snap to authority below this distance |
| `reconcileHalfLifeSec` | `0.08` s | Yes | Exponential blend rate during reconciliation |
| `reconcileMaxSpeed` | `20.0` units/s | Yes | Max correction speed toward authority |
| `showPredictionColors` | `false` | Yes | Tints ball by visual state for debugging |
| `debugmode` | `false` | Yes | Enables debug overlay visualizations |

---

## Snapshot Interpolation

All in `hockeyctr.cs`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `minRenderDelaySec` | `0.020` s | Yes | Minimum adaptive render delay |
| `maxRenderDelaySec` | `0.250` s | Yes | Maximum adaptive render delay |
| `renderDelaySmoothSec` | `0.300` s | Yes | How slowly render delay adapts to jitter |
| `discSameEpsilon` | `0.0005` | Yes | Epsilon for position discrepancy logging |
| `MaxBufferCapacity` | `128` | No (const) | Max snapshots in the circular buffer |

Adaptive delay formula: `oneWayRtt × 0.35 + jitterEWMA × 0.5 + skippedTicksPenalty`, clamped to `[min, max]`.

---

## Network Simulation (Testing)

In `NetDelaySlider.cs`. Simulates bad network conditions without a real WAN link.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `delayMsSlider` | UI-driven | Yes | Artificial one-way packet delay (ms) |
| `jitterMsSlider` | UI-driven | Yes | Random jitter around the delay (ms) |
| `lossPercentSlider` | UI-driven | Yes | Simulated packet loss (%) |
| `delaySeconds` | `5.0` s | Yes | Delay before simulator activates at startup |
| `stepMs` | `10` ms | No | Step size for keyboard slider adjustment |

---

## RTT Measurement

In `NetworkLatencyUI.cs`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `pingInterval` | `1.0` s | Yes | How often ping/pong RPC is measured |
| `CurrentRTT` (static) | `50.0` ms | No | Current round-trip time in ms; read globally |

---

## Game Rules & Timer

In `GameManager.cs`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `totalTime` | Scene-set | Yes | Total match duration in seconds |
| `timeRemaining` | `= totalTime` | Yes | Countdown; synced via `syncedTime` NetworkVariable |
| `syncedMode` | `1` (Medium) | No | Difficulty pushed from server: 0=Easy, 1=Medium, 2=Hard |

---

## Runtime Feature Flags

In `NetworkFeaturesController.cs`. Global static flags toggled via UI at runtime.

| Flag | Default | Description |
|------|---------|-------------|
| `isInterpolationEnabled` | `true` | When false, clients jump directly to server positions |
| `isPredictionEnabled` | `true` | When false, clients wait for server before moving ball |
| `isDebugModeEnabled` | `true` | Controls debug visualization across the game |

Toggle from code: `NetworkFeaturesController.isInterpolationEnabled = false`.

---

## Lobby & Connection

In `SimpleLobbyManager.cs`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `lobbyBaseUrl` | `http://127.0.0.1:8080` | Yes | FastAPI lobby server URL |
| `dedicatedServerPort` | `7777` | Yes | DS bind port (overridden by `-port` CLI arg) |
| `dedicatedBindAddress` | `0.0.0.0` | Yes | DS listen address |
| `gameplaySceneName` | `"AirHockey"` | Yes | Scene to load once all players connect |
| `useConnectionApproval` | `true` | Yes | Token-based approval; disable only for local testing |
| `sharedSecret` | `"dev_secret_change_me"` | Yes | Token HMAC secret — **change in production** |

---

## Dedicated Server Lifecycle

In `DedicatedServerLifecycle.cs`. CLI args override inspector defaults at launch.

### Inspector Defaults

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `heartbeatIntervalSeconds` | `5.0` s | Yes | How often DS POSTs heartbeat to lobby |
| `idleShutdownSecondsDefault` | `10.0` s | Yes | Seconds with no clients before DS self-quits |
| `initialDelaySeconds` | `0.5` s | Yes | Delay before first `/ds/started` call |
| `tryReadFromGameManager` | `true` | Yes | Reads match state from GameManager for heartbeat |
| `forceEnableInEditor` | `false` | Yes | Force DS lifecycle active in editor |

### Command-Line Arguments

| Argument | Type | Purpose |
|----------|------|---------|
| `-server` / `-dedicated` | flag | Activates dedicated server mode |
| `-serverView` | flag | Enables spectator window on server |
| `-roomId` | string | Room ID for lifecycle API calls |
| `-port` | int | UDP port to listen on |
| `-lobbyUrl` | string | Lobby base URL for callbacks |
| `-dsSecret` | string | Bearer token for DS→lobby auth |
| `-idleShutdownSeconds` | float | Overrides idle shutdown timeout |
| `-maxPlayers` | int | Maximum players allowed |
| `-ownerPlayerId` | string | Player ID of the room owner |

---

## Position Source & CV Coordinate Mapping

In `PosMessageNetworked.cs` and `NetworkedCircleController.cs`.

**`PosMessageNetworked.cs`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `cv_width` | `917.0` | Yes | CV frame width (px) for X normalization |
| `cv_height` | `609.0` | Yes | CV frame height (px) for Y normalization |
| `doLatencyTest` | `false` | Yes | Enables CSV latency logging |

**`NetworkedCircleController.cs`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `expansionT` | `0.8` | Yes | Interpolation param for boundary expansion |
| `Hardcode` | `true` | Yes | Use hardcoded instead of dynamic expansion |

**PlayerPrefs overrides** (persist between sessions): `"CV_Width"`, `"CV_Height"`. See `CVDimensionsOverride.cs` for a runtime UI.

---

## Display & Screen Setup

In `DisplaySetup.cs` and `ScreenConfigSO.cs`.

**`ScreenConfigSO`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `projectorWidth` | `768.0` px | Yes | Projector width for field scaling |
| `projectorHeight` | `1024.0` px | Yes | Projector height |

**`DisplaySetup.cs`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `refreshRate` | `60` Hz | Yes | Target display refresh rate |
| `activateAdditionalDisplays` | `false` | Yes | Activates secondary Unity displays |

**`PositionSourceSO`**

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `currentSource` | `Ros2` | Yes | Input source: `Mouse` (testing) or `Ros2` |

---

## Trace Recording & Replay

In `TraceSession.cs`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `Mode` | `Off` | Yes | `Off` / `Record` / `Replay` |
| `RecordFileName` | `""` (auto) | Yes | Output CSV filename; auto-timestamped if empty |
| `ReplayFileName` | `""` | Yes | Input CSV filename to replay |
| `UseNgoTick` | `true` | Yes | Use NGO tick as timestamp for reproducible replay |
| `FallbackTickRate` | `50` | Yes | Tick rate if NGO unavailable |
| `ReplayTickRate` | `50` | Yes | Playback tick rate during replay |
| `ReplayUseFixedTickClock` | `true` | Yes | Fixed-tick clock for deterministic replay |
| `EnableHotkeys` | `true` | Yes | Enables F6/F7/F8 shortcuts |
| `SetOffKey` | `F6` | Yes | Stop tracing |
| `SetRecordKey` | `F7` | Yes | Start recording |
| `SetReplayKey` | `F8` | Yes | Start replay |

---

## Position Simulator (Mouse — Testing Only)

In `PositionSimulator.cs`. Used when `currentSource = Mouse`.

| Parameter | Default | Inspector | Purpose |
|-----------|---------|-----------|---------|
| `radius` | `50.0` | No | Simulated circle radius in screen pixels |

---

## Quick Reference

| Goal | Parameter | Script |
|------|-----------|--------|
| Ball too fast / slow | `ballSpeed` (via mode) | `gameMgr.cs` |
| Kick too weak / strong | `kickFactor` | `hockeyctr.cs` |
| Prediction feels wrong | `reconcileHalfLifeSec`, `reconcileMaxSpeed` | `hockeyctr.cs` |
| Remote ball stutters | `minRenderDelaySec`, `maxRenderDelaySec` | `hockeyctr.cs` |
| Snap jumps visible | `reconcileSnapThreshold` | `hockeyctr.cs` |
| RTT measurement rate | `pingInterval` | `NetworkLatencyUI.cs` |
| Match duration | `totalTime` | `GameManager.cs` |
| CV coords off | `cv_width`, `cv_height` | `PosMessageNetworked.cs` |
| DS shuts down too fast | `idleShutdownSecondsDefault` | `DedicatedServerLifecycle.cs` |
| Lobby URL | `lobbyBaseUrl` | `SimpleLobbyManager.cs` |

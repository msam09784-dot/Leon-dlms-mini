# LEON AUDIO DLMS — BLE protocol v1

## Compatibility status
The referenced firmware `LEON_AUDIO_DLMS_MINI_v0.9.4_BOOT_LOGO_ONLY.ino` does not currently contain a BLE/GATT server. This Android project is a native UI + BLE client prototype. It cannot control DSP audio until the matching GATT server is integrated into ESP32-S3 firmware.

## GATT
- Advertised name: `LEON AUDIO DLMS`
- Service UUID: `8d7f1000-6b5a-4c2d-9e10-4f2a6c8b0001`
- Write characteristic: `8d7f1001-6b5a-4c2d-9e10-4f2a6c8b0001` (WRITE)
- Notify characteristic: `8d7f1002-6b5a-4c2d-9e10-4f2a6c8b0001` (NOTIFY, CCCD 0x2902)
- UTF-8 JSON; one command per GATT write; payload <=180 bytes.
- Notifications are UTF-8 JSON.

## Commands
`{"cmd":"set","target":"input","ch":0,"param":"gain","value":-3.0}`
`{"cmd":"set","target":"output","ch":2,"param":"gain","value":-6.0}`
`{"cmd":"set","target":"output","ch":0,"param":"hpf","value":45.0}`
`{"cmd":"set","target":"output","ch":0,"param":"lpf","value":16000.0}`
`{"cmd":"set","target":"input","ch":1,"param":"delay","value":2.5}`
`{"cmd":"preset","action":"save","slot":1}`
`{"cmd":"preset","action":"load","slot":1}`
`{"cmd":"status"}`

Expected notification:
`{"ok":true,"cmd":"set","target":"output","ch":0,"param":"gain","value":-6.0}`

## Firmware integration requirements
1. Start BLE GATT server with these UUIDs; advertise `LEON AUDIO DLMS`.
2. Parse JSON with ArduinoJson 7.
3. Never modify `gConfig` from a BLE callback. Copy commands into a bounded queue.
4. Apply queued commands in the normal control/UI task under existing `gConfigMux`; clamp using local UI limits, increment `gConfigRevision`, set `configDirty`, and trigger existing DSP refresh.
5. SAVE/LOAD must call existing preset functions; do not invent a second preset format.
6. Notify only after a command is accepted/applied.
7. Map EQ bands, crossover type/slope, compressor, routing, phase, mute and master mute only after mapping to exact existing config fields and validation limits.

BLE callbacks must not block audio processing. Rate-limit slider updates to about 20–30 commands/second per control and send a final value when the user releases the slider.


## Android UI behavior
- `Demo UI` is enabled by default. It allows interaction with the screen without sending parameter changes to the DSP.
- Turn `Demo UI` off before expecting the app to transmit BLE commands.
- Slider changes are committed when the user releases the slider, reducing BLE write bursts.

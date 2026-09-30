# 07 - Atomic Red Team on WS01

**Purpose:** Generate adversary behaviour on WS01 and build our detections.

**Prerequisites:** 05

# Steps

1. Make a snapshot of WS01, Atomics will create new users, tasks, and registry keys. Using a snapshot will make it much easier to reverse rather than manually cleaning everything up.
2. Install Atomic Red Team in an elevated powershell.
3. Windows will prevent some atomics from downloading/running. Whether to disable it or add an exclusion path is up to you. I decided to add an exclusion and keep Defender as a secondary telemetry source.
4. The lab is now fully setup.

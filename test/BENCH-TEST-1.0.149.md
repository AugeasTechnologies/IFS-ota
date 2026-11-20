# IFS Control Suite 1.0.149: bench test

This build changes how the app shuts plasma off on a run, a pause, a stop, a quit and an update.
The simulators already pass it. It needs one pass on a real station before it goes to customers.

**Needed:** a Windows PC, a Plasma StandAlone or Tip Charger with its Opta, and the display on its
usual port. The Puretip tablet is optional.

**Install:** download `IFS-1.0.149.exe` from the link you were sent and check its SHA-256 against
`SHA256.txt`. Run it as you would run IFS.exe. The installed 1.0.148 is not touched.

Write down pass or fail for each line. If something looks wrong, take a photo of the screen and
the station.

## 1. First launch
1. The licence agreement appears. Accept it, then sign in.
2. Close and reopen the app. The agreement does not appear again.
3. The Opta and the display connect on their own.

## 2. Plasma during a run
Make a protocol with one **Plasma for a time** step of 10 s.

4. Run it. Plasma comes on, stays on for 10 s, then goes off. The run ends as "finished".
5. Run it again and press **Pause** about 4 s in. Plasma goes off straight away.
6. Wait 10 s, then press **Resume**. Plasma comes back on for the remaining ~6 s, then goes off.

Make a second protocol: **Plasma on or off** (On), then **Wait** 20 s.

7. Run it and press **Pause** within 5 s. Plasma goes off.
8. Press **Resume**. Plasma stays OFF, and the log says so.
9. Run it again and press **Stop** within 5 s. Plasma goes off and the run ends as "stopped".

## 3. Leaving while plasma is on
Use the first protocol, set to 30 s, for each of these.

10. Close the app window while plasma is on. The run stops and plasma goes off before the app
    closes.
11. Reopen the app, start the run again, and try to update the Opta firmware while it is going.
    The app refuses and says a protocol run is in progress.

## 4. Firmware
12. With no run going, update the Opta firmware from the Firmware screen. It finishes, and the
    Opta reconnects within about 15 s. Press RESET once if it does not.
13. Start a flash and try to start a run while it is going. The run is refused.

## 5. Everyday use
14. Run one normal cleaning cycle from Control.
15. Export the Data Log as CSV and open it.
16. Optional, with the Puretip tablet: connect it and run one Puretip step.

## Reporting
Send back the numbers that failed, with what you saw. If every line passed, a short "all pass" is
enough: 1.0.149 then goes to every station and to the Mac App Store.

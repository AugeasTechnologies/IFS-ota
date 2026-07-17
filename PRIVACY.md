# IFS Control Suite - Privacy Statement

_Last updated: 2026-07-17. Publisher: Augeas Technologies._

The IFS Control Suite ("the application") is a local desktop application used to control
IonField plasma-station laboratory equipment (an Arduino Opta controller, a 4D Systems
display, and a Puretip One tablet).

## Personal information collected

**None.** The application does not collect, store, or transmit any personal information.

- It runs entirely on the local machine and serves its interface only on the loopback
  address (`127.0.0.1`). It is not a public website and has no user accounts or logins.
- It communicates only with locally connected hardware (USB serial, USB/ADB, or devices on
  the local network).
- There is no analytics, advertising, tracking, or telemetry of any kind.

## Network access

The only outbound network request the application makes is to check for and download software
updates from its public update channel on GitHub
(`https://raw.githubusercontent.com/AugeasTechnologies/IFS-ota/...`). This is an anonymous
HTTPS GET request for a version manifest and, if you choose to update, the signed installer.
No user data is sent with these requests.

## Contact

Augeas Technologies - info@augeastechnologies.com

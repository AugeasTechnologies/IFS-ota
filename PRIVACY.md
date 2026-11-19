# IFS Control Suite - Privacy Statement

_Last updated: 2026-11-19. Publisher: Augeas Technologies (software developer) for IonField Systems._

The IFS Control Suite ("the application") is a desktop application that controls IonField
plasma-station laboratory equipment: the Arduino Opta controller, 4D Systems touch displays, the
Puretip One tablet station and Bioyond lab automation. The full privacy statement is at
https://augeastechnologies.com/privacy. This page summarises what the application itself sends.

## What leaves your computer

**Station heartbeat (can be switched off).** Unless you turn off the telemetry option in
Settings, the application sends a short status message about every five minutes to Augeas's
fleet service. It contains a random station identifier, the station name, the computer's host
name and operating-system version, the application version, which instruments are connected and
their basic state (idle, running, fault), licence status and licence identifier, the time zone,
the site label if you set one, and a summary health score. It contains **no** run data, recipes,
results, operator names or passwords. It lets your organisation see its stations and lets Augeas
and IonField support them.

**Update checks.** The application asks this public channel on GitHub
(`https://raw.githubusercontent.com/AugeasTechnologies/IFS-ota/...`) whether a newer, signed
version of the application or of the equipment firmware exists. These are anonymous HTTPS
requests; like any download they reveal your IP address to GitHub.

**Licence activation.** Licence files are checked on your computer, offline. If you activate or
release a licence key, the key, the station identifier and the station name are sent to Augeas so
it can issue the licence and count seats.

**IFS AI (only if you set it up).** If you use IFS AI, your question and the station information
needed to answer it are sent to the AI provider you chose (for example Anthropic, OpenAI, or a
model on your own network), using your own account and key where you supplied one. That
provider's own privacy terms apply.

## What stays on your computer

Operator accounts and passwords, run logs, the Data Log, Insight history, protocols, recipes and
settings are stored in your own user profile and are never uploaded by the application.

The application's interface is served only on the loopback address (`127.0.0.1`) and is not
reachable from other computers.

## Contact

Augeas Technologies - info@augeastechnologies.com

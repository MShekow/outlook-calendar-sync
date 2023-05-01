# outlook-calendar-sync

A Microsoft Power Automate flow to synchronize two Outlook 365 calendars.

Download the zip archieve from [here](https://github.com/MShekow/outlook-calendar-sync/raw/main/Outlook%20calendar%20sync%20v0.4.zip).

Please see [this blog post](https://www.augmentedmind.de/?p=2990) for details.

## Change log

### v0.4 (2023-05-01)

* New setting: configure whether event reminders should be shown for blocker events
  (on by default, set to trigger 15 minutes prior to the event start time). The
  use case is to set this to `false` if you configured Outlook to show both
  calendars anyway

### v0.3 (2023-03-23)

* Simplify the set up: now you only need to specify the calendar names (as shown in Outlook) - it is
  no longer necessary to change the calendar ID in each Outlook-related Power Automate action
* Synchronize the "showAs" field of the blocker events

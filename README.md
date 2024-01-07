# outlook-calendar-sync

A Microsoft Power Automate flow to synchronize two Outlook 365 calendars.

Download the zip archive from [here](https://github.com/MShekow/outlook-calendar-sync/raw/main/Outlook%20calendar%20sync%20v0.8.zip).

If you want to get rid of all blocker events, you can use [this](https://github.com/MShekow/outlook-calendar-sync/raw/main/Delete%20SyncBlocker%20events.zip) helper PowerAuto flow.

Please see [this blog post](https://www.augmentedmind.de/?p=2990) for details.

**Note: there is also a fork of this flow that synchronizes your Outlook <-> _Google_ calendar, see [here](https://github.com/MShekow/outlook-google-calendar-sync).**

## Change log

### v0.8 (2024-01-05)

* Add support for (optional) **uni-directional** synchronization, by adding
  two new settings named **Synchronize from calendar X to calendar Y**
  which you can set to `false` as desired (default is `true`)
* Add new setting **Synchronize events without other attendees** (default is `true`).
  Set this to `false` if you do not want to synchronize events that you created just
  for yourself (e.g. reminders for a doctor's appointment), i.e., events that have
  neither optional nor required attendees (other than yourself)

### v0.7 (2023-11-12)

* Add new (optional) setting **Category for SyncBlocker events in calendar 1/2** which you
  can set to an array of one or more Outlook category names, which are assigned
  to your SyncBlocker events. The updated categories will also be retroactively
  applied to already-existing SyncBlocker events

### v0.6 (2023-09-24)

* Fix errors such as `InvalidTemplate. The execution of template action 'Cal1_SB_events_with_matching_location' failed [...] property 'iCalUId' doesn't exist [...]`
  by filtering out events that do not have a `iCalUId` property for some reason
* Add new setting **Hide event details in calendar 1/2**. If you set this to
  `true`, the sync blocker event will not have the real subject and
  body of the corresponding real calendar event, but a dummy text.
  This is useful for privacy reasons.

### v0.5 (2023-05-19)

* Fix the displayed date of _all-day_ events be distorted
  (see https://github.com/MShekow/outlook-calendar-sync/issues/2), by fixing
  the "all day" event attribute
* Fix the _sensitivity_ event attribute, so that _private_ events also
  appear as _private_ for the blocker events
* Improve synchronization stability by using the unique CalDAV ID (instead of
  Outlook's internal event ID), and specifying an upper limit when querying for
  events. This should reduce any accidental duplicated blocker events.

### v0.4 (2023-05-01)

* New setting: configure whether event reminders should be shown for blocker events
  (on by default, set to trigger 15 minutes prior to the event start time). The
  use case is to set this to `false` if you configured Outlook to show both
  calendars anyway

### v0.3 (2023-03-23)

* Simplify the set up: now you only need to specify the calendar names (as shown in Outlook) - it is
  no longer necessary to change the calendar ID in each Outlook-related Power Automate action
* Synchronize the "showAs" field of the blocker events

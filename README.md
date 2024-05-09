# outlook-calendar-sync

A Microsoft Power Automate flow to synchronize two Outlook 365 calendars.

Download the zip archive from [here](https://github.com/MShekow/outlook-calendar-sync/raw/main/Outlook%20calendar%20sync%20v0.9.zip).

If you want to get rid of all blocker events, you can use [this](https://github.com/MShekow/outlook-calendar-sync/raw/main/Delete%20SyncBlocker%20events.zip) helper PowerAuto flow.

Please see [this blog post](https://www.augmentedmind.de/?p=2990) for details.

**Note: there is also a fork of this flow that synchronizes your Outlook <-> _Google_ calendar, see [here](https://github.com/MShekow/outlook-google-calendar-sync).**

## Change log

### v0.9 (2024-05-09)



> [!IMPORTANT]
> v0.9 introduces **major** changes to the synchronization core logic. The main new feature is that the _location_ field is now synchronized. Please carefully read the points below for more details.
> 
> As a consequence, the flow consumes a few more actions, and you might reach your daily quota/limit more quickly. If you don't have a strong need to synchronize the location field, you might want to stay on a previous version!

* **WARNING**: if you are upgrading from 0.8 or older, **you _must_ first run the _Delete SyncBlocker events_ flow** to delete all old SyncBlocker events (because they are incompatible with v0.9 and newer). You must run the _Delete SyncBlocker events_ flow once, for _every_ calendar that is affected by your v0.8 (or older) synchronization flow.
  * Note: the zip archive of the _Delete Syncblocker events_ flow has been updated, so that it can delete SyncBlocker events created with v0.9 or newer. Should you run into problems with v0.9 and want to revert to v0.8, make sure that you download the new version of the _Delete SyncBlocker events_ flow (and **re**-import it into Power Automate), to properly clean your SyncBlocker events.
* **Feature**: The _location_ field of your calendar events is now synchronized to the SyncBlocker events. In v0.8 and older, that location field of the SyncBlocker events was used to store the **ID** of the source event. In v0.9, that ID is now stored in a **fake email address** in the first required _attendee_ of the SyncBlocker event. Make sure not to delete this attendee!
  * Note: by default, whenever a Power Automate action creates a calendar event _with attendees_, the Outlook 365 platform automatically sends invitation emails to these attendees. The v0.9 flow uses email addresses such as `do-not-delete-this-attendee-or-sync-breaks@<sanitized-event-id>.invalid`. Normally, sending emails to such non-existent addresses would fail, causing a "delivery failed notice" email to be placed in your inbox for every synchronized event, which would be extremely annoying. However, the `<sanitized-event-id>` is over 110 characters long, and the Outlook 365 platform (fortunately) _skips_ sending invite emails to email addresses with such a long host name (for reasons we don't know). However, there is a chance that Microsoft changes this behavior, or that Outlook 365 behaves differently for _your_ account/tenant. Consequently, **_if_ you get these failed delivery notice emails, you will need to revert to v0.8 of this flow**.
* **Feature**: The setting _"SyncBlocker prefix"_ is now optional, because the synchronization algorithm now disambiguates real events from SyncBlocker events via the _required attendees_ field. If you want, you can now set the SyncBlocker prefix to an empty string, and instead use a _category_ (configured with a custom color) to visually disambiguate your real vs. blocker events in Outlook. You may now also change the SyncBlocker prefix _at any time_, the flow will automatically update the subject/title of all events accordingly (in v0.8 and older, you were not allowed to change the prefix after running the first sync).
* **Feature:** For already-synchronized events, changes made to the title / subject of the source event are now propagated to the SyncBlocker event.


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

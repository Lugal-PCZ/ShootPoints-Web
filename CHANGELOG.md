# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

<!-- (Added, Changed, Deprecated, Removed, Fixed, Security) -->

## [Unreleased]

- Addition of CHANGELOG file.
- Moving the configs settings from the configs.ini file into the ShootPoints database so that configs can be backed up along with the project database.

## [1.7.5] - 2025-11-05

### Changes
- Make “End Session” checkbox in Raspberry Pi power off form checked by default.
- Remove subclass id from delete_subclass() error message.
- Sort sessions in reverse chronological order in Export Session menu so that the most recent session is always first.

### Fixed
- Clear the Subclasses menu when creating a new grouping.
- Improve logic of handle_special_subclasses() and add handling of Elevation Control Points.


## [1.7.4] - 2025-10-20

### Changed
- Improved the appearance of the Prism Offsets form on phones.

### Fixed
- Revert previous change to inputmode="numeric" because of the lack of decimal point in the numeric keyboard.
- Force decimal keyboard where most apt, and automatically convert commas to periods when used with European keyboards.


## [1.7.3] - 2025-10-19

### Changes
- Show the stakeout widgets (disabled) in cancelShotForm and saveLastShotForm, in order to minimize confusion.
- Improve spacing between form elements in the UI.
- Force a numeric keyboard for all numeric fields in the UI.

### Fixed
- Fixed a mistake in the installation instructions.
- Hide SessionFormIndicator regardless of success of start new session call.


## [1.7.2] - 2025-09-23

### Fixed
- Fixed the database version check when upgrading from an old (un-versioned) database.


## [1.7.1] - 2025-08-22

### Changed
- Disabled the duplicate coordinates check for a new station.
- Simplified user feedback when the app is run in demo mode.
- Made sessions menus in Utilities popup wider.
- Automatically hide the cancel grouping and cancel session buttons when taking a shot.

### Fixed
- Fix form values to work properly with newer versions of FastAPI.
- Reset the Delete Session menu after resetting the database.


## [1.7.0] - 2025-08-22

### Changed
- Save start session by resection steps to the database for better fault tolerance.
- Use startswith() instead of string slicing in Python code for comparisons.
- Refactored export_session_for_livemap() function.
- Improved wording of backsight outside of limits error message.
- Update load_current_session_info() to optionally check for a stale session.

### Fixed
- Fix or suppress linter warnings/errors in Python code.
- Edge case fix: clear setup errors at start of _load_application().


## [1.6.3] - 2025-04-20

### Fixed
- Improved handling and error reporting of bad serial connections.


## [1.6.2] - 2025-04-15

### Added
- Created a mechanism for upgrading the database schema.

### Changed
- Adjusted the CSS to display better on small phone screens.


## [1.6.1] - 2025-03-24

### Fixed
- Replaced double backslashes in f-strings with chr(92) so that they don’t break Python 3.11.


## [1.6.0] - 2024-12-19

### Changed
- Updated the installation instructions.
- Rename “Raspberry Pi” to “data collector” in the UI and update the message returned when it is rebooted.

### Fixed
- Updated filenames and paths to work on Windows.
- Dropped lower air pressure limit to 480mmHG to accommodate high altitudes.
- Re-populate temp and pressure fields in new session form when an old session has been closed without starting a new session.


## [1.5.4] - 2024-11-04

### Changed
- Automatically strip leading and trailing whitespace from survey session label and surveyor name.
- Add sequential number to default label for each session started in the same day.
- Back-end changes to support making session info popups consistent in the web app.
- Make session info popups more consistent.


## [1.5.3] - 2024-11-01

### Changed
- Make UI elements in on-the-fly adjustments and utilities popups collapsible to save space on cell phones.
- Pre-populate temperature and pressure fields when starting a new session.

### Fixed
- Improve handling of 60 second rounding edge case.
- Fix LiveMap javascript error when closing a session without any current unsaved shot.
- Explicitly set currentsessionid attribute to 0 in data.js when there is no current session.


## [1.5.2] - 2024-10-23

### Changed
- Change format of date string for setting date/time on the Raspberry Pi.
- Stop LiveMap from deleting the last point of polygons.
- Re-word label for downloading the database.
- Other minor UI tweaks.

### Fixed
- Fix rare case where seconds rounds to 60 when calculating an azimuth.
- Permit latitudes in the southern hemisphere.


## [1.5.1] - 2024-10-22

### Changed
- Update start new session images in README.
- Bundle Leaflet with frontend.
- Update README to mention bundling of Leaflet.
- Permit leading zeroes left of the decimal point when entering azimuth.
- Update messaging when rebooting or shutting down the Raspberry Pi.
- Add color to shutdown confirmation message.
- Other minor UI improvements.

### Fixed
- Correct SQLite syntax of NULL default values in blank database template.
- Remove testing code left behind in data.js.
- Always end the current grouping when a session is ended.
- Gracefully handle livemap background when internet is unreachable.


## [1.5.0] - 2024-09-01

### Changed
- Replace Google Maps background imagery in mapping function with [OpenStreetMap](https://www.openstreetmap.org).
- Update README to reflect UI changes.
- Rename resection backsight functions for consistency with normal backsight.
- Apply atmospheric corrections in the totalstation module instead of survey.py.
- Split version numbering into separate app and database versions in anticipation of handling database migrations.
- Tweak how backsight variance limit is handled.
- Add instrument height to tripod.py when starting session by resection.
- Replace testing code with more useful output when loading the app.
- Prompt the surveyor explicitly name the new station when starting a session by resection instead of using an auto-generated name.
- Improve appearance and behavior of Start New Session form.
- Simplify behavior of Abort Resection button.
- Improve clarity of “End Current Session” and “End Current Grouping” buttons by making them red Xes, and standardize the way that this widget appears in the prism offsets panel.
- Replace string concatenation with template literals for cleaner, more readable javascript code.
- Heavy refactoring of start_new_session() function in survey.py for better error handling of errors and user cancels, especially while performing a resection.

### Fixed
- Fix exporting of PRJ files for shapefiles, which was improperly indented in the Python code.
- Correct the math of the occupied point elevation when starting new session by resection.
- Fix logic in handling of user-canceled shots when using an actual total station.
- Fix behavior of End Current Session button.


## [1.4.0] - 2024-08-22

### Added
- Live mapping of current surveying session implemented with [Leaflet](https://leafletjs.com).
- Prompt to end current session if it was started over 12 hours prior.

### Changed
- Links to GitHub repositories for api and frontend added to README.
- Automatically end the current grouping when ending the current session.
- Updated the default geometry descriptions in the blank database to match those in the README.
- Update format_outcome() to accept a list of special keys, instead of just a string with one key.
- Return a better error message when an error occurs while saving to the database.

### Fixed
- Remove error checking code accidentally left in.
- Fix typo that returned the occupied point’s lat and lon as a tuples instead of as numbers.


## [1.3.2] - 2024-07-03

### Changed
- Improved messages to the user.
- Numerous minor UI improvements.


## [1.3.1] - 2024-06-30

### Fixed
- Update installation instructions in README to reflect new submodule paths.


## [1.3.0] - 2024-06-13

### Added
- Displays the version of ShootPoints-Web in the footer of the Utilities Panel.

### Changed
- New paths to application submodules (api and frontend).


## [1.2.1] - 2024-05-27

### Changed
- Updates to the README file.


## [1.2.0] - 2024-05-26
This is the first version of ShootPoints-Web with a public-facing version number. Informal internal versioning began with v1 in 2022, and was quickly replaced with v1.1. Version 1.2.0 rolls together a large number of fixes and enhancements.

### Added
- Created a routine to start a new session by resection (i.e., setting up on an arbitrary point and shooting backsights to two existing stations).
- Added routine to reset the ShootPoints database.
- Added the CRS to exported shapefiles so that QGIS plots them automatically, without prompting the user for the files’s CRS.
- Created a special case to deal with Raspberry Pi UART port.
- Added code comments explaining serial port options.

### Changed
- Improved pattern validation in HTML forms.
- Changed the name of exported database file to include the date.
- Changed the names of exported sessions to include the session labels.
- Improved creation and handling of configs.ini file.
- Improved session exports with spatial control information.

### Removed
- Removed the ability to auto-select the serial port, since it adds confusion for minimal gain.

### Fixed
- Adapted to changes to pattern matching in HTML forms processed by FastAPI.
- Fixed doubled newlines in CSVs exported for viewing on Windows OS.
- Fixed the letter on UTM zones so that it is only “N” or “S” instead of the full list returned by the utm Python module. (The other letters are not actually part of the UTM spec. [See the Wikipedia note on Latitude Bands](https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system#Latitude_bands).)
- Fix typing so that it works with Python 3.9 (which is what gets installed with Raspbian).
- Update OS check to work with recent versions of Raspbian.
- Fixed a failure to load the saved session properly.
- Fixed an incorrect date calculation when running on Rasbian.
- Show the currently selected port in the configs panel menu.
- Minor UI improvements and bug fixes.

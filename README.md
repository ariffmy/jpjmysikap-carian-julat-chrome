# MySIKAP Range Runner

**A JavaScript-based semi-automated vehicle registration number range checker for Malaysia's MySIKAP portal.**

MySIKAP Range Runner is a browser-based JavaScript utility designed to simplify repetitive vehicle registration number range searches on Malaysia's MySIKAP portal.

The tool provides dynamic registration series selection, automatic form preparation, range progression, and search result detection while requiring manual confirmation for each search.

> **Disclaimer:** This is an independent, unofficial community project. It is not developed, endorsed, affiliated with, or maintained by Jabatan Pengangkutan Jalan Malaysia (JPJ).

## Features

- **Dynamic Registration Categories** — Supports both current and special registration number series.
- **Dynamic State and Series Selection** — Retrieves available options directly from the MySIKAP interface.
- **ZK Framework Compatibility** — Supports standard and rounded ZK combobox components.
- **Automatic Range Preparation** — Prepares registration number ranges of up to 50 numbers per search.
- **Custom Starting Number** — Allows searches to begin from any number between 1 and 9999.
- **Automatic Range Progression** — Advances to the next range after a recognized unsuccessful search.
- **Search Result Detection** — Stops when an available registration number list is detected.
- **Manual Search Confirmation** — Requires the user to initiate each search using the MySIKAP interface.
- **Persistent Configuration** — Saves selected categories, series and progress using browser local storage.
- **Floating Control Panel** — Provides a compact interface for configuration, progress and status monitoring.

## Supported Registration Categories

### Current Registration Numbers

**Nombor Pendaftaran Semasa**

The tool follows the registration series dependency structure:

`Kategori Siri → Negeri / Kawasan → Siri Awalan → Siri Akhiran → Nombor`

Available states, prefixes and suffixes are retrieved dynamically from MySIKAP.

### Special Registration Numbers

**Nombor Pendaftaran Istimewa**

Special registration numbers follow a different selection structure:

`Kategori Siri → Siri Awalan → Siri Akhiran → Nombor`

The State/Area field is automatically hidden when this category is selected.

Examples of special prefixes observed during development include:

`A`, `FC`, `FD`, `FE`, `FH`, `GM`, `GOLD`, `M`, `MADANI`, `MYSI`, `P`, `PETRA`, `VIPS`, `VIP`

Available prefixes and suffixes depend on the MySIKAP portal and may change.

## Requirements

- Google Chrome or another compatible Chromium-based browser
- Access to the MySIKAP portal
- JavaScript enabled
- Browser Developer Tools
- An active MySIKAP session, where required

No external JavaScript libraries, Node.js installation or build process are required.

## Installation

1. Open the MySIKAP portal in your browser.
2. Navigate to the vehicle registration number search page.
3. Open Chrome Developer Tools by pressing `F12`.
4. Select the **Console** tab.
5. Copy the complete JavaScript source from this repository.
6. Paste the script into the Console and execute it.

The **MySIKAP Range Runner** floating control panel should appear in the top-right corner of the page.

Chrome may require you to explicitly allow pasting into the Developer Tools Console.

**Security notice:** Always review JavaScript before executing it in your browser. Only run code from sources you trust.

## Usage

### Step 1: Select Registration Category

Choose either:

- Nombor Pendaftaran Semasa
- Nombor Pendaftaran Istimewa

The control panel automatically adjusts its available fields based on the selected category.

### Step 2: Configure Registration Series

For current registration numbers, select the State/Area, Prefix and Suffix.

For special registration numbers, select the Prefix and Suffix where available.

Options are loaded dynamically from the active MySIKAP form.

### Step 3: Set Starting Number

Enter a starting registration number between **1 and 9999**.

For example, starting from `2437` produces the following ranges:

| Search | Number Range |
|---|---|
| 1 | 2437–2486 |
| 2 | 2487–2536 |
| 3 | 2537–2586 |
| 4 | 2587–2636 |

The final range is limited to 9999.

### Step 4: Activate Semi-Auto Mode

Click **AKTIFKAN SEMI-AUTO**.

The script prepares the selected registration series and number range, then focuses the MySIKAP **Cari** button.

### Step 5: Confirm Each Search

Press **Enter** while the **Cari** button is focused, or click it manually.

The script does not automatically submit searches.

### Step 6: Review Results

When the portal displays the recognized unsuccessful-search message:

`Nombor yang diberikan tiada dalam julat.`

The tool attempts to close that specific message, waits for the form to reset, and prepares the next range.

When a list of available registration numbers is detected, the runner stops and leaves the results available for manual review.

## How It Works

```text
Start
  |
  v
Select Registration Category
  |
  +-- Current Series
  |     |
  |     v
  |   Select State / Area
  |     |
  |     v
  |   Select Prefix and Suffix
  |
  +-- Special Series
        |
        v
      Select Prefix and Suffix
        |
        v
Prepare Number Range
        |
        v
Focus Cari Button
        |
        v
User Manually Confirms Search
        |
        v
Check MySIKAP Response
        |
        +-- Available Numbers Found
        |       |
        |       v
        |   Stop and Display Status
        |
        +-- Recognized Unsuccessful Result
                |
                v
          Close Specific Popup
                |
                v
          Wait for Form Reset
                |
                v
          Advance Range by 50
                |
                v
          Prepare Next Range
```

## Technical Details

The MySIKAP portal uses the ZK Framework, which dynamically manages form components and element identifiers.

MySIKAP Range Runner avoids relying on fixed element IDs and instead identifies active components using visible form labels and supported ZK selectors.

Supported combobox input classes include:

- `z-combobox-inp`
- `z-combobox-rounded-inp`

This is particularly important because MySIKAP may retain hidden components from inactive registration categories in the document.

The tool prioritizes visible form elements to reduce interference from inactive components.

### Local Storage

Configuration and progress are stored locally using:

`mysikap_runner_v45`

Stored information includes the selected registration category, state where applicable, prefix, suffix and current starting number.

No external storage service is used by the script.

## Limitations

- Requires manual confirmation for every search.
- Depends on the current structure and behavior of the MySIKAP portal.
- May require updates if MySIKAP changes its HTML, ZK components or response messages.
- Requires the relevant search page to remain open.
- Does not bypass authentication, CAPTCHA, session restrictions or access controls.
- Does not guarantee registration number availability or eligibility.
- Does not reserve, purchase or register vehicle numbers.
- Does not automatically select an available number or proceed with registration.
- The v4.5 implementation should be tested against the live portal before being considered stable.

## Responsible Use

This project is intended as a convenience utility for authorized, interactive use.

Users are responsible for complying with JPJ requirements, MySIKAP terms of use, applicable laws and any portal usage limits.

Do not use this project to bypass access restrictions, overwhelm government infrastructure or perform unattended bulk submissions.

## Troubleshooting

**Control panel does not appear**

Verify that the script was executed on the correct MySIKAP page and check the browser Console for errors.

**Registration options are empty**

Ensure that the relevant category or parent field has been selected and allow time for the MySIKAP interface to update.

**Search does not start**

The tool intentionally requires manual confirmation through the **Cari** button.

**The runner stops unexpectedly**

Check whether the MySIKAP session has expired, an unexpected dialog appeared, or the page structure has changed.

**Permissions policy warning appears**

Some browser Console warnings, including those related to the `unload` event, may originate from the portal's framework rather than the runner itself.

## Version History

### v4.5 — Dynamic Category and ZK Component Support

- Added category-specific field handling.
- Improved compatibility with standard and rounded ZK comboboxes.
- Added active/visible form component detection.
- Added dynamic special registration series selection.
- Improved configuration restoration and range preparation.

## Roadmap

Potential future improvements include:

- More comprehensive error handling and recovery.
- Improved progress tracking and session handling.
- Better support for changes to the MySIKAP interface.
- Additional validation and diagnostic tools.
- A packaged browser extension or userscript distribution.

These are proposed enhancements, not currently implemented features.

## License

No license has been specified yet. A license should be added before distributing or accepting contributions under defined reuse terms.

## Disclaimer

**MySIKAP Range Runner is an unofficial, independent open-source-oriented project and is not affiliated with Jabatan Pengangkutan Jalan Malaysia (JPJ) or the Government of Malaysia.**

All vehicle registration number availability information is provided by the official MySIKAP portal. Results displayed by this utility are for convenience only and must be verified through official JPJ channels.

Use this software at your own risk. The project is provided without any guarantee of uninterrupted operation, accuracy, suitability or continued compatibility with the MySIKAP portal.

# starscript

This is a python script for automating the game [Space Traders](https://spacetraders.io), and is built on the [SpacePyTraders SDK](https://github.com/ZacHooper/spacePyTraders).

## Installation

Install pipenv: `pip install --user pipenv`

Source your profile: `source ~./profile`

Install dependencies: `pipenv install`

Run `launchdev.sh`

## Changelog (click to expand)

<details>
<summary> Latest - v0.0.3 - Threading & Live Commands</summary>

- Added threading
- Command Queue now runs on a separate thread from main, user can enter their own commands into main
- - `exit`/`quit`/`stop` quit execution
- - `:<command>` queue following command (passthrough for plan commands)
- - `help` lists available commands and their functions
- Added feedback for invalid commands
- `printLevel` renamed to `printlv`
- `:help` lists available plan commands and their functions
- `:help <cmd>` functionality exists, no help text yet - see .starcommands on roadmap (doesn't check for non-existant commands)
</details>

<details>
<summary>v0.0.2 - Preliminary command implementation</summary>

- Read `.starplan` text files and parse out basic commands
- - `# <comment>` Ignored
- - `get ...` Get request
- - - `info` Specifies account info (credits, ship count, etc.)
- - - `loans` Specifies loan info (all loans associated with account)
- - - `loc ...` Specify location request
- - - - `info` Specifies location info (traits, type, name, etc.)
- - - - `ships` Specifies list of ships docked at location (ship id, owner, type)
- - - - `market` Specifies market data from location (good names, prices, quantity, etc.)
- - - `ships` Specifies list of all owned ships
- - - `ship` Specifies info about single ship
    </details>

<details>
<summary>v0.0.1</summary>

- SpacePyTraders SDK installed, able to do basic query
- Setup pipenv
</details>

## Roadmap

- Ensure command parsing case-insensitive (auto-capitalize ids, force all cmds to lower)
- 1:1 command parity with SDK/API endpoints
- `.starcommands` file for specifying available commands/arguments and their functions, help text, etc. (allows for easily creating and sharing custom commands for advanced users plus keeps commands from being hardcoded)
- `:help <cmd>` lists detailed help for the specified cmd
- `.starsession` file for remembering credentials
- - Auto-register & run startup script (+notify) on server wipe
- `.starsettings` file for remembering settings (e.g. log verbosity, notification preferences, etc.)
- `.starlog` file for logging longitudinal data (changes will be streamed to captains-log for filing in DB for long-term storage)
- `.starhistory` file for logging commands and their source (plan \[and how plan was started/assigned], cli, helmsman, etc.), errors, notable events, etc.
- Script Variables
- - Save data returned by requests for use in conditional commands
- Notifications
- - Will be able to notify user through various avenues when plan (cmd) or script (hard-coded events like wipe or unrecoverable errors) encounters the need to
- - Integration with Discord as a bot?
- - Email?
- - SMS?
- Commands for working with **Plans** in `/plans`
- - `list`, `new`, `edit`, `del`, etc.
- - `loop` and `goto` commands for use within plans
- - conditionals
- - Consider more how plans interact with ships, probably most simply by keeping a list of ships locally, and storing an assigned plan for each. OR by storing a list of active plans, and an associated list of assigned ships?
- Interactive CLI, perhaps using [Nubia](https://github.com/facebookincubator/python-nubia)
- Consider alternative UI options later (e.g. web/windows)
- API for integration with helmsman
- - Primarily for working on plans remotely
- - Also allows assigning different plans to ships remotely (using plans local to starscript)
- Integration with captains-log
- - Can save API calls by passing information directly from starscript to captains-log (if passed relevant data CL can skip next automatic data poll, otherwise will simply ignore the input)
- space-opera: Allow for ship `groups` and `pools`
- - `groups`: Ships that are grouped will all use the same plan (maybe include in base starscript)
- - `pools`: Ships that are part of a pool can be dynamically assigned to certain plans conditionally. Ships can be a part of multiple pools but (maybe?) are only able to be active in one pool at a time

### Research/Thoughts

```
Starcript:
Maybe it is more of an intermediary for more advanced scripting
	Like an SDK wrapping the lower level SDK
		Can either edit a (not started) init plan to kick off their automation on script startup, and/or:
		https://stackoverflow.com/questions/43861164/passing-data-between-separately-running-python-scripts
		Use multiprocessing and a queue() or pipe(): https://stackoverflow.com/questions/8463008/multiprocessing-pipe-vs-queue/58015119#58015119
    Which would also potentially be the route to go for integrating with space-opera

Would a multiprocessing queue be a good substitute for like a redis cache of the most up-to-date data on ship locations and such? Need something that can handle multiple threads reading/writing simultaneously to it as they get/need info.

Then space-opera is basically orchestrating starscript plans. Need a way for plans to request an external response and wait for it, as well as outputting info for space-opera conditionals... Plans need like... Events or something...
```

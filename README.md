# claudespeak

A mod for Claude Code. When Claude finishes a reply, it reads a 1-3 sentence summary out loud with [Fish Audio](https://fish.audio). Short plain replies are read as they are. It streams, so speech starts as the audio arrives.

Only one voice plays at a time, even with several sessions open: a new reply cuts off the one before it. It stays quiet in sub-agents, `claude -p`, scheduled desktop routines, and a `claude` started from inside another session.

## Requirements

- **macOS.** That is the only OS it has been tested on. Playback uses ffmpeg's `audiotoolbox` output, which is macOS only, so it will not play on Linux or Windows as written.
- **ffmpeg**: `brew install ffmpeg`
- **A Fish Audio account and API key.** Get a key at <https://fish.audio/app/api-keys/>.
- **Claude Code** with a build that loads hooks modules. Tested on 2.1.309.

## Install

```
claude plugin marketplace add albertshiney/claudespeak
claude plugin install claudespeak@claudespeak
```

Then restart Claude Code, or run `/reload-plugins` in an open session.

### Add your Fish Audio key

Open the .env at `~/.claude/claudespeak/.env`, paste your Fish Audio API key after `FISH_API_KEY=`, save.

The file does not exist until claudespeak makes it, so run `/speak key` to create it and open it. If you skip this, your first finished reply does the same thing and tells you the path. The file is created readable only by you, and it lives outside the plugin folder so updates don't wipe it. A `FISH_API_KEY` environment variable takes priority over the file.

## Commands

It is **on by default**. `/speak on` and `/speak off` apply to every session on the machine.

| Command | What it does |
| --- | --- |
| `/speak` | Says whether it is on, and whether this session can speak |
| `/speak on` | Reads replies aloud in every session |
| `/speak off` | Stops reading aloud in every session |
| `/speak stop` | Cuts off the current speech in any session, and leaves the setting alone |
| `/speak key` | Creates the .env if needed and opens it |

## Voice and speed

Both are constants at the top of `hooks/register.ts`:

- `DEFAULTS.voiceId`: the Fish voice, Sawyer by default. It is also an option in the plugin's settings.
- `SPEECH_SPEED`: 1.1 by default, Fish's `prosody.speed` (0.5 to 2.0).

## Cost

Every spoken reply uses Fish Audio credits, and because it is on by default that means every reply in every session a person is attached to. Long replies also use one Haiku call on your Claude account to write the summary. Use `/speak off` to stop both.

## Status

Tested on macOS with scripted Claude Code sessions. Not yet confirmed by hand in the desktop app's Code tab, and sessions the app starts in the background may behave differently there.

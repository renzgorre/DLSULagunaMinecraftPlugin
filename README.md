# DLSU Laguna Minecraft Plugin

A Spigot/Paper plugin for the DLSU Laguna community Minecraft server. Players join a class section, and everything they do in game earns points for that section. Section standings are shown on an in-game scoreboard and posted to Discord.

## Features

- **Sections:** players join a section, chat privately with it, and compete for the top spot
- **Automatic scoring** from player activity (see [Scoring](#scoring))
- **Leaderboards:** a live sidebar scoreboard plus per-section player rankings
- **Events:** duels between players, rotating bounties, and a Build Battle world with land claims
- **Discord updates** through DiscordSRV, with standings posted every minute

## Scoring

Points are recalculated from each player's tracked stats:

| Activity | Points |
| --- | --- |
| Blocks broken | 1 per 500 |
| Blocks placed | 1 per 300 |
| Player kills, mob kills | 1 per 100 |
| Distance travelled | 1 per 1,000 blocks |
| Fish caught | 1 per 10 |
| Items crafted | 1 per 100 |
| Villager trades | 1 per 10 |
| Duels and bounties | Points won or lost directly |
| Deaths | Minus 1 per 3 |

A section's score is the sum of its members' points.

## Commands

| Command | Description | Default access |
| --- | --- | --- |
| `/joinsection <section>` | Join a section (resets your stats if you switch) | Everyone |
| `/sectionchat <message>` | Message your section | Everyone |
| `/seepoints` | See how many points you have contributed | Everyone |
| `/sectionleaderboard` | Top contributors in your section | Everyone |
| `/duel <player>`, `/duelaccept <player>`, `/dueldeny <player>` | Challenge another player | Everyone |
| `/trackplayer <player>`, `/bountylist` | Track bounty targets | Everyone |
| `/buildbattle [claim\|back]` | Enter the Build Battle world | Everyone |
| `/createsection`, `/deletesection` | Manage sections | Operators |
| `/leavesection <player>` | Remove a player from their section | Operators |
| `/setplayerpoints <player> <category> <points>` | Adjust a player's stats | Operators |
| `/tallypoints` | Recalculate section totals | Operators |
| `/clearpoints`, `/clearplayerstats <player>` | Reset points or one player's stats | Operators |
| `/doomsday` | Reset everything | Operators |

## Requirements

- Java 21
- A Spigot or Paper 1.21 server
- [DiscordSRV](https://github.com/DiscordSRV/DiscordSRV) installed on the server

## Building

1. Download the DiscordSRV jar and place it at `libs/DiscordSRV-Build-1.29.0.jar` (it is a compile-only dependency and is not committed).
2. Build with Gradle 8.10 or newer:

   ```bash
   gradle build
   ```

3. Copy the jar from `build/libs/` into your server's `plugins/` folder.

To try it on a local test server, run `gradle runServer`.

## Configuration

The Discord channel that receives score updates is set in `DLSULaguna.java` and `ScoreUpdateTask.java`. Change the channel ID there before building for a different server.

Sections and stats are stored as YAML in the plugin's data folder (`sections.yml`, `players_stats.yml`, `section_stats.yml`), so they persist across restarts.

## Project structure

```
src/main/java/zoy/dLSULaguna/
├── DLSULaguna.java      Plugin entry point and scheduled tasks
├── commands/            One class per command
├── listeners/           Join, chat and stat-tracking listeners
├── data/                Player data management
└── utils/               Scoring, scoreboard, Discord and file helpers
    └── playerevents/    Duels, bounties and Build Battle
```

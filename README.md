In this guide, I will show you how to manage user permissions in [SourceMod](https://www.sourcemod.net/about.php).

SourceMod is a [MetaMod](https://www.metamodsource.net/about) addon that supports most games running on the Source Engine. It is a powerful and highly optimized platform for creating plugins and managing server administration. SourceMod is widely used in games such as [Team Fortress 2](https://store.steampowered.com/app/440/Team_Fortress_2/), [Left 4 Dead 2](https://store.steampowered.com/app/550/Left_4_Dead_2/), and [Counter-Strike: Source](https://store.steampowered.com/app/240/CounterStrike_Source/)!

When running a game server with SourceMod installed, it is likely that at some point you'll want to grant yourself or others permissions such as the ability to kick and ban players (admin access).

## Table Of Contents
* [Finding Steam IDs](#finding-steam-ids)
* [Flags](#flags)
* [Passwords](#passwords)
* [Managing User Permissions](#managing-user-permissions)
    * [Using The `admins_simple.ini` File](#using-the-admins_simpleini-file)
    * [Using The `admins.cfg` File](#using-the-adminscfg-file)
* [Reloading Users](#reloading-users)
* [See Also](#see-also)

## Finding Steam IDs
Before assigning permissions to users, you will need to find their Steam ID. Both SteamID2 (`STEAM_X:X:XXXXXXXX`) and SteamID3 (`[U:X:XXXXXX]`) formats are supported in SourceMod.

If you're connected to a server running on the Source Engine, you can execute the `status` command in the Developer Console to retrieve the Steam IDs of the current players connected to the server.

Otherwise, you may use tools such as [Steam ID Finder](https://www.steamidfinder.com/) and [Steam ID I/O](https://steamid.io/) to find Steam IDs.

## Flags
To determine what specific permissions you want to assign to users, you need to understand SourceMod's flags system and what access they provide. Each flag is assigned using a single character.

Here is a list of flags available in SourceMod.

| Name | Flag | Purpose |
| ---- | ---- | ------- |
| reservation |	`a` | Grants reserved slot access. |
| generic | `b` | Grants generic admin (required for admin). |
| kick | `c` | Grants access to kick other players. |
| ban |	`d` | Grants access to ban other players. |
| unban | `e` | Grants access to remove bans. |
| slay | `f` | Grants access to slay and harm other players. |
| changemap | `g` | Grants access to change maps or major gameplay features. | 
| cvar | `h` | Grants access to change most ConVars. |
| config | `i` | Grants access to execute config files. |
| chat | `j` | Grants special chat privileges. |
| vote | `k` | Grants access to start or create votes. |
| password | `l` | Grants access to set a password on the server. |
| rcon | `m` | Grants access to use [RCON](https://developer.valvesoftware.com/wiki/Source_RCON_Protocol) commands. |
| cheats | `n` | Grants access to change the `sv_cheats` ConVar or use cheating commands. |
| root | `z` | Enables all flags and ignores immunity values. |

Additionally, there are **custom** flags which can be used by plugins and such.

| Name | Flag
| ---- | ----
| custom1 | `o` |
| custom2 | `p` |
| custom3 | `q` |
| custom4 | `r` |
| custom5 | `s` |
| custom6 | `t` |

More information about flags may be found [here](https://wiki.alliedmods.net/Adding_Admins_(SourceMod)#Levels).

## Passwords
If you'd like, you can require specific users to input a password before being able to use the custom permissions assigned to them. They will need to input this password every time they connect to the game server, but they can automate this process on their side if they'd like.

Firstly, you'll want to modify the `addons/sourcemod/configs/core.cfg` file and change the `PassInfoVar` line value from `_password` to something custom such as `_test`. The user will need to know their specific password and the password you set `PassInfoVar` to.

When the user connects to the game server, they will need to execute the following command in the Developer Console.

```
setinfo "<PassInfoVar Password>" "<User Password>"
```

For example:

```
setinfo "_test" "myCustomPass123"
```

If the user wants to automate this process on their side, they'll need to modify the file `<Game Dir>/cfg/autoexec.cfg` and add the command above to the file.

## Managing User Permissions
Managing user permissions and assigning flags is done by modifying a file inside of the `addons/sourcemod/configs` directory (or `addons\sourcemod\configs` folder on Windows).

There are two files you may edit which are explained below.

### Using The `admins_simple.ini` File
This file is used for quickly assigning user permissions. This is best for simple setups or wanting to quickly assign permissions to a user. When assigning permissions in this file, each line should have the following format.

```
"<Steam ID/!IP/Steam name>" "[Immunity Level:]<Flags/@group>" ["password"]
```

Here is an example of granting a user the `reservation`, `generic`, `kick`, and `ban` flags with `70` immunity.

```
"STEAM_0:0:36969327" "70:abcd"
```

### Using The `admins.cfg` File
Using this file requires inputting more lines when assigning permissions to users, but may be considered cleaner. The format of the file uses SourceMod's [KeyValues](https://wiki.alliedmods.net/KeyValues_(SourceMod_Scripting)). Users should be added inside of the `Admins` section.

Here is a generic format for adding users.

```
"<Name>"
{
    "auth"          "steam"
    "identity"      "<Steam ID>"
    "flags"         "<Flags>"
    "immunity"      "<Immunity>"
}
```

Here is a list of *keys* you can assign to each user.

* `auth` - Must be one of `steam`, `name`, or `ip` (unless there is a custom auth method), and instructs SourceMod how to interpret the identity value.
* `identity` - A qnique value that allows SourceMod to find the user given an authentication method and the given value.
* `password` - Specifies the password the user must enter.
* `group` - Specifies a group name the user should inherit if available. More than one *group* line can be specified. There should be no `@` symbol as there is no ambiguity.
* `flags` - The access flags the user should receive.
* `immunity` - The immunity level the user should receive.

## Reloading Users
After assigning permissions to user(s) through the files mentioned above, you will need to reload the admin cache so that the permissions propagates.

The cache should automatically reload when the map changes or server restarts. You may use the server command `sm_reloadadmins` to reload the cache manually so you don't have to wait until a map change or server restart.

## See Also
* [Adding Admins](https://wiki.alliedmods.net/Adding_Admins_(SourceMod)) - Detailed Wiki on adding admins officially by AlliedMods.
* [SourceBans++](https://sbpp.github.io/) - A popular plugin that makes it easier to manage users, groups, bans, and more!

For any questions or feedback, please reply to the forum topic [here](https://forum.moddingcommunity.com/t/how-to-manage-user-permissions-in-sourcemod/193)!
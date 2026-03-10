mrp_voicezones

Advanced voice zones system for FiveM with in-game polygon editor, pma-voice integration and oxmysql database storage.

This resource allows administrators to create voice proximity zones directly in-game and automatically change the player's voice range when entering the zone.

Features

In-game polygon zone editor

Voice mode override per zone

pma-voice integration

oxmysql database storage

Automatic SQL table creation

Automatic invalid row cleanup

ESX / QBCore / QBX / Standalone compatible

ACE permission support

Multi-language support

Cfx Escrow ready

Dependencies

This script requires the following resources:

ox_lib
oxmysql
pma-voice
PolyZone

Make sure they are installed and started before mrp_voicezones.

Example server.cfg order:

ensure ox_lib
ensure oxmysql
ensure PolyZone
ensure pma-voice
ensure mrp_voicezones

Installation
1. Install the resource

Place the folder inside your resources directory.

Example:

resources/[standalone]/mrp_voicezones

2. Start the resource

Add this to your server.cfg:

ensure mrp_voicezones

3. Database

This script uses oxmysql only.

You do NOT need to import any SQL manually.

The database table is automatically created on resource startup.

Default table name:

mrp_voicezones

You can change it inside:

shared/config.lua

Example:

Config.Database.table = "mrp_voicezones"

Permissions

Admins must have permission to open the voice zone menu.

Default command:

/voicezones

Permission example using ACE:

add_ace group.admin mrp.voicezones allow
add_ace group.superadmin mrp.voicezones allow

You can also configure permissions using:

framework groups

player identifiers

ACE permissions

All settings are located in:

shared/config.lua

Configuration

Main configuration file:

shared/config.lua

Example important settings:

Locale:

Config.Locale = "en"
Config.FallbackLocale = "en"

Permission system:

Config.UseAcePermissions = true
Config.AcePermission = "mrp.voicezones"
Config.FrameworkPermissionMode = "group"

Database:

Config.Database = {
table = "mrp_voicezones",
startupRetryCount = 20,
startupRetryDelayMs = 500,
cleanupInvalidRowsOnBoot = true
}

Voice Modes

Voice modes define how players talk inside zones.

Example configuration:

Config.VoiceModes = {

whisper = {
range = 1.5,
mode = 1,
priority = 30
},

normal = {
range = 7.0,
mode = 2,
priority = 20
},

shout = {
range = 15.0,
mode = 3,
priority = 10
}

}

Explanation:

range = voice distance
mode = pma-voice talking mode index
priority = zone priority when zones overlap

How It Works

Admin opens /voicezones

Creates a polygon zone

Selects voice mode

Zone is saved in MySQL

All players receive zone sync

When entering the zone voice range changes automatically

When leaving the zone voice returns to default

Server Exports

Get all zones:

local zones = exports.mrp_voicezones:GetZones()

Get a zone by ID:

local zone = exports.mrp_voicezones:GetZoneById(1)

Get best zone for coordinates:

local zone = exports.mrp_voicezones:GetBestZoneForCoords(vector3(441.2, -981.9, 30.6))

or

local zone = exports.mrp_voicezones:GetBestZoneForCoords(441.2, -981.9, 30.6)

Client Exports

Get current player zone:

local zone = exports.mrp_voicezones:GetCurrentZone()

Get all synced zones:

local zones = exports.mrp_voicezones:GetZones()

Escrow Editable Files

This resource supports Cfx Escrow.

Editable files:

shared/config.lua
shared/locale.lua
locales/*.lua

Example fxmanifest.lua escrow configuration:

escrow_ignore {
"shared/config.lua",
"shared/locale.lua",
"locales/*.lua"
}

File Structure

mrp_voicezones

fxmanifest.lua
README.md

bridge/
client.lua
server.lua

client/
main.lua

server/
main.lua

shared/
config.lua
locale.lua

locales/
en.lua
fr.lua
es.lua
it.lua

Troubleshooting

Menu does not open:

check ACE permission

check dependency start order

check pma-voice installation

check ox_lib installation

Script does not start:

verify oxmysql is running

verify PolyZone is installed

verify resource start order

Default Setup Example

server.cfg example:

ensure ox_lib
ensure oxmysql
ensure PolyZone
ensure pma-voice
ensure mrp_voicezones

add_ace group.admin mrp.voicezones allow
add_ace group.superadmin mrp.voicezones allow

License

This resource is intended for FiveM servers only.
Redistribution or resale without permission from the author is not allowed.

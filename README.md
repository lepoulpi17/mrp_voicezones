# mrp_voicezones

**Advanced Voice Zones System for FiveM**

Create polygon voice zones directly in-game and automatically change player voice range using **pma-voice**.

Zones are stored in **oxmysql** and synchronized to all players.

---

# Features

- In-game polygon zone editor
- Automatic voice range override
- pma-voice integration
- oxmysql database storage
- Automatic SQL table creation
- Automatic invalid row cleanup
- ESX / QBCore / QBX / Standalone compatible
- ACE permission support
- Multi-language support
- Cfx Escrow ready

---

# Dependencies

This resource requires:

- ox_lib
- oxmysql
- pma-voice
- PolyZone

Make sure they start **before mrp_voicezones**.

Example server.cfg:

```
ensure ox_lib
ensure oxmysql
ensure PolyZone
ensure pma-voice
ensure mrp_voicezones
```

---

# Installation

## 1. Install the resource

Place the folder inside your resources directory.

Example:

```
resources/[standalone]/mrp_voicezones
```

---

## 2. Start the resource

Add to your server.cfg:

```
ensure mrp_voicezones
```

---

## 3. Database

This script uses **oxmysql only**.

You **do not need to import any SQL file**.

The table is automatically created when the resource starts.

Default table name:

```
mrp_voicezones
```

You can change it inside:

```
shared/config.lua
```

Example:

```lua
Config.Database.table = "mrp_voicezones"
```

---

# Permissions

Admins must have permission to open the voice zone menu.

Default command:

```
/voicezones
```

Example ACE permissions:

```
add_ace group.admin mrp.voicezones allow
add_ace group.superadmin mrp.voicezones allow
```

Permissions can also be configured using:

- framework groups
- identifiers
- ACE permissions

Configuration file:

```
shared/config.lua
```

---

# Configuration

Main configuration file:

```
shared/config.lua
```

Example settings:

## Locale

```lua
Config.Locale = "en"
Config.FallbackLocale = "en"
```

## Permission system

```lua
Config.UseAcePermissions = true
Config.AcePermission = "mrp.voicezones"
Config.FrameworkPermissionMode = "group"
```

## Database

```lua
Config.Database = {
    table = "mrp_voicezones",
    startupRetryCount = 20,
    startupRetryDelayMs = 500,
    cleanupInvalidRowsOnBoot = true
}
```

---

# Voice Modes

Voice modes control the talking range inside zones.

Example:

```lua
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
```

Explanation:

| Setting | Description |
|--------|-------------|
| range | voice distance |
| mode | pma-voice talking mode |
| priority | zone priority |

Higher priority zones override lower ones.

---

# How It Works

1. Admin runs `/voicezones`
2. Creates a polygon zone
3. Selects a voice mode
4. Zone is saved in MySQL
5. All players receive zone sync
6. Voice range changes when entering the zone
7. Voice returns to default when leaving

---

# Server Exports

Get all zones:

```lua
local zones = exports.mrp_voicezones:GetZones()
```

Get a zone by ID:

```lua
local zone = exports.mrp_voicezones:GetZoneById(1)
```

Get best zone for coordinates:

```lua
local zone = exports.mrp_voicezones:GetBestZoneForCoords(vector3(441.2, -981.9, 30.6))
```

or

```lua
local zone = exports.mrp_voicezones:GetBestZoneForCoords(441.2, -981.9, 30.6)
```

---

# Client Exports

Get current zone:

```lua
local zone = exports.mrp_voicezones:GetCurrentZone()
```

Get all zones:

```lua
local zones = exports.mrp_voicezones:GetZones()
```

---

# Escrow Editable Files

This resource supports **Cfx Escrow**.

Editable files:

```
shared/config.lua
shared/locale.lua
locales/*.lua
```

Example fxmanifest configuration:

```lua
escrow_ignore {
    "shared/config.lua",
    "shared/locale.lua",
    "locales/*.lua"
}
```

---

# File Structure

```
mrp_voicezones
│
├─ fxmanifest.lua
├─ README.md
│
├─ bridge
│  ├─ client.lua
│  └─ server.lua
│
├─ client
│  └─ main.lua
│
├─ server
│  └─ main.lua
│
├─ shared
│  ├─ config.lua
│  └─ locale.lua
│
└─ locales
   ├─ en.lua
   ├─ fr.lua
   ├─ es.lua
   └─ it.lua
```

---

# Troubleshooting

Menu does not open:

- Check ACE permissions
- Check dependency start order
- Verify ox_lib installation

Script does not start:

- Verify oxmysql is running
- Verify PolyZone is installed
- Verify resource start order

---

# License

This resource is intended for **FiveM servers only**.

Redistribution, resale or leaking without permission from the author is not allowed.

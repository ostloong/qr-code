# Lyra QR Code specification

## History
Version 0.2, edited on May 15, 2023 by Alexander Borsuk
- Fixed issues in the first draft

## Introduction

The specification describes QR code format used in Lyra AR glasses and Sirius AR ski goggles.

## Supported API

The following [MeCard-like format](https://en.wikipedia.org/wiki/MeCard_(QR_code)) is used:
`<Command>:<Tag1>:<Value1>;<Tag2>:<Value2>;;`

### 1. WiFi connection setup (`WIFI:`)

Encodes WiFi connection parameters to connect glasses to the hotstpot/router.

Use already established and popular format that is described in [zxing wiki][]:

Always starts with `WIFI:`. Tags can be in any order, see [zxing wiki][] for details
- Sample 1: `WIFI:S:MySSID;T:WPA;P:MyPassW0rd;;`
- Sample 2 (with UTF-8): `WIFI:T:WEP;S:我的 wifi 网络;P:mypass;;`

References:
- https://github.com/zxing/zxing/wiki/Barcode-Contents#wi-fi-network-config-android-ios-11
- https://en.wikipedia.org/wiki/QR_code#Joining_a_Wi%E2%80%91Fi_network
- https://www.androidauthority.com/share-wifi-password-android-3184916/

### 2. Translation settings (`TR:`)

Specifies the (optional) source language and the target language for translation. If source language is missing, the source language is automatically detected.

Always starts with `TR:`. Supported tags (in any order):
- `S` = optional source language
- `T` = target language

Values are case-insensitive ISO-639 or BCP-47 case-insensitive language codes.

- Sample 1: `TR:S:en;T:zh-CN;;`
- Sample 2: `TR:T:fr;S:be;;`
- Sample 3 (autodetect source language): `TR:T:PT-BR;;`

References:
- https://cloud.google.com/translate/docs/languages
- https://www.deepl.com/en/docs-api/translate-text/

### 3. Geo location and/or address for navigation (`NAV:`)

Encodes coordinates and/or text address of the destination place that can be used for navigation and directions.

Always starts with `NAV:`. Supported tags (in any order):
- `C` = latitude and longitude in decimal format, separated by comma (optional if address is specified)
- `N` = optional name
- `T` = optional type of the destination place
- `A` = address (optional if coordinates are specified)
- `M` = optional transportation mode (assume walk if not specified), one of
   - `C` = car
   - `W` = walk
   - `B` = bicycle
   - `P` = public transport

- Sample 1 (UTF-8): `NAV:C:47.38426,8.57420;M:W;N:Zoocafe;A:Zürichbergstrasse 219,8044,Zürich,Switzerland;;`
- Sample 2 (Cyrillic UTF-8): `NAV:N:Зоо кафе;C:47.38426,8.57420;;`
- Sample 3: `NAV:M:C;A:4435 Mission St,San Francisco,CA,94112,United States;N:Amami Sushi Bistro SF;;`

### 4. AI Assistant command/question (`AI:`)

Send AI command or question to glasses.

Always starts with `AI:`. Supported tags (in any order):
- `Q` = question text for AI assistant
- `S` = optional source language (ISO-639 or BCP-47) for the question
- `T` = optional target language (ISO-639 or BCP-47) for the answer

- Sample 1: `AI:Q:What do you see on the picture? Describe it in 160 characters or less.;;`
- Sample 2: `AI:S:en;T:zh-CN;Q:What do you see on the picture?;;`

### 5. Any text (`T:`)

Send text that should be displayed on glasses.

Always starts with `T:`. Supported tag:
- `T` = text to display

- Sample 1: `T:T:有五棵盛开的樱花树。;;`



[zxing wiki]: https://github.com/zxing/zxing/wiki/Barcode-Contents#wi-fi-network-config-android-ios-11

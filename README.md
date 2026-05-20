# nostr-php-nwc-demo

Demos for [nostr-php-nwc](https://github.com/dukeh3/nostr-php-nwc) — a PHP client for the Nostr Wallet Connect protocol (NIP-47).

## Prerequisites

- PHP 8.1+ with `gmp`, `curl`, and `sodium` extensions
- Composer
- An NWC-compatible wallet (e.g. [Alby Hub](https://github.com/getAlby/hub))

## Setup

```bash
composer install
cp .env.example .env
```

Edit `.env` with your NWC connection URI:

```
NWC_URI=nostr+walletconnect://WALLET_PUBKEY?relay=wss://relay.example.com&secret=YOUR_SECRET
```

## Demos

### CLI: Basic info and balance

```bash
php cli/demo.php
```

Connects to the wallet via NWC, queries `get_info` and `get_balance`.

### Web: Send and receive UI

```bash
php -S localhost:18080 -t web/
```

Open http://localhost:18080. The web app provides:

- **Wallet balance** displayed at the top
- **Receive** — creates a bolt11 invoice via `make_invoice`, then polls `lookup_invoice` until payment is detected
- **Send** — pays a bolt11 invoice via `pay_invoice`

## How It Works

NWC (NIP-47) communicates with a Lightning wallet over Nostr relays:

1. Parses the NWC URI to get the wallet's pubkey, relay URL, and client secret
2. Opens a WebSocket to the relay
3. Sends encrypted NWC requests (kind 23194) signed with the client key
4. Receives encrypted responses (kind 23195) from the wallet service
5. Decrypts and displays the results

Encryption uses NIP-44 v2. No REST API, no API keys — just Nostr events over a relay.

## License

Copyright (C) 2025

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.

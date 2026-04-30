# MediFast AI

India-first medicine assistant for Telegram.

MediFast AI helps people search medicines, understand simple Hindi/Hinglish symptom queries, manage family medicine needs, browse nearby pharmacies, and repeat common medicine searches faster.

It is built as a production-safe MVP on top of the existing MediFast Telegram bot. Existing medicine search, pharmacy inventory, SOS, admin commands, and REST APIs are preserved.

> Medical safety note: This bot helps users discover medicines and pharmacy availability. It is not a replacement for a doctor.

## What Changed In This Upgrade

This patch upgrades the old Jaipur pharmacy availability bot into a more polished product demo called **MediFast AI**.

New things added:

- Hinglish and Hindi smart search layer
- Family medicine profiles
- Cleaner premium search result messages
- Recent search history for repeat/reorder use cases
- Nearby pharmacy location-ready architecture
- Better Telegram buttons and onboarding
- Safer medical disclaimer in user-facing flows
- Removed accidental bot token logging from startup

## Main Features

### 1. Smart Hinglish / Hindi Search

Users no longer need to type only exact medicine names.

They can type natural queries like:

```text
Sar dard ki dawa chaiye
Bukhar ki tablet
Pet dard medicine
Headache medicine
Khansi ke liye kuch
Zukham dawa
Fever medicine for child
Gas acidity tablet
Ulti ki medicine
```

The bot understands common intent and maps it to useful medicine search categories.

Examples:

| User says | Bot understands |
| --- | --- |
| sar dard | headache / pain relief |
| bukhar | fever |
| khansi | cough |
| zukham | cold / allergy |
| pet dard | stomach pain |
| gas / acidity | acidity |
| ulti | nausea / vomiting |

If the bot is not confident, it asks the user for a clearer search instead of guessing.

### 2. Family Profiles

Users can save basic family member details and search in a more natural way.

Family member fields:

- Name
- Relation
- Age group: child, adult, or senior
- Notes, optional

Commands:

```text
/family
/addmember
/members
/removeMember
```

Example:

```text
/addmember Papa|papa|senior|diabetes and BP
```

After this, users can type:

```text
papa fever medicine
mom acidity tablet
reorder papa medicine
```

### 3. Better Search Results

Search responses are now cleaner and more demo-ready.

Results show:

- Medicine name
- Use case
- Salt / generic composition
- Brand or alternative name when available
- Price
- Pharmacy name and area
- Address and contact details
- Availability confidence
- Last verified time
- Prescription tag where needed
- Medical safety disclaimer

### 4. Recent Search / Reorder Flow

The bot stores recent searches per Telegram user.

If the same user searches the same medicine again, the bot can suggest:

```text
Need to reorder previous medicine?
```

For family members, users can try:

```text
reorder papa medicine
```

The bot checks recent family-linked search history and shows the last medicine searched for that member.

### 5. Nearby Pharmacy Ready

The `/nearby` command now supports two modes:

- Browse pharmacies by area
- Share location

When location is shared, the bot returns:

```text
Nearby pharmacy module ready for integration.
```

The code now has a separate nearby pharmacy service so future integrations can be added cleanly:

- Google Maps
- Live stock systems
- Pharmacy partner APIs
- Distance based sorting

### 6. Better Telegram UX

The bot now has inline buttons for:

- Search again
- Add family member
- View members
- Nearby pharmacy
- SOS alert
- Language preference on start

The `/start`, `/help`, and `/about` messages now use the MediFast AI product tone.

## Commands

### User Commands

```text
/start
/help
/about
/feedback
/search <medicine or symptom>
/nearby
/areas
/sos <medicine name>
/family
/addmember
/members
/removeMember <name or relation>
```

### Admin Commands

```text
/admin
/addpharmacy
/addmedicine
/updatestock
/opensos
/stats
```

## Demo Script

Use these in Telegram to show the upgraded MVP:

```text
/start
Bukhar ki tablet
Sar dard ki dawa chaiye
Gas acidity tablet
/addmember Papa|papa|senior|diabetes and BP
papa fever medicine
reorder papa medicine
/family
/members
/nearby
/sos rare medicine name
```

## Tech Stack

- Node.js
- grammY for Telegram bot handling
- Express for API server
- MongoDB with Mongoose
- Fuse.js for fuzzy medicine search
- Winston for logging
- dotenv for environment config

## Project Structure

```text
medifast-bot/
├── config/
│   └── database.js
├── scripts/
│   └── seed.js
├── src/
│   ├── bot/
│   │   ├── commands/
│   │   │   ├── admin.js
│   │   │   ├── family.js
│   │   │   ├── nearby.js
│   │   │   ├── search.js
│   │   │   └── sos.js
│   │   ├── middleware/
│   │   │   ├── adminGuard.js
│   │   │   └── rateLimiter.js
│   │   └── index.js
│   ├── models/
│   │   ├── Inventory.js
│   │   ├── Pharmacy.js
│   │   ├── SearchHistory.js
│   │   ├── SosRequest.js
│   │   └── UserProfile.js
│   ├── services/
│   │   ├── familyService.js
│   │   ├── historyService.js
│   │   ├── intentEngine.js
│   │   ├── nearbyPharmacyService.js
│   │   └── searchService.js
│   ├── utils/
│   │   ├── formatter.js
│   │   └── logger.js
│   ├── index.js
│   └── server.js
├── .env.example
├── package.json
└── render.yaml
```

## Setup

### 1. Install dependencies

```bash
npm install
```

### 2. Create `.env`

Copy `.env.example` into `.env` and fill in values.

```text
TELEGRAM_BOT_TOKEN=your_token_from_botfather
MONGODB_URI=your_mongodb_connection_string
PORT=3001
```

For local MongoDB, an example URI is:

```text
mongodb://localhost:27017/medifast
```

### 3. Seed demo data

```bash
npm run seed
```

### 4. Start the bot

```bash
npm start
```

For development:

```bash
npm run dev
```

## API Endpoints

### Health

```text
GET /health
```

### Search

```text
GET /api/search?q=Paracetamol
```

### Pharmacies

```text
GET /api/pharmacies
GET /api/pharmacies?area=Mansarovar
```

## Database Notes

Existing collections:

- pharmacies
- inventories
- sosrequests

New collections from this upgrade:

- userprofiles
- searchhistories

No manual migration is required. Mongoose creates these collections when the features are first used.

## Safety Rules In The Product

- The bot does not replace a doctor.
- Prescription medicines are marked with an Rx tag when available in inventory data.
- Symptom search is used for discovery, not diagnosis.
- The bot encourages users to confirm stock with the pharmacy.
- Serious symptoms should still go to a doctor or emergency care.

## Next Scale Ideas

Good next features to add:

1. Real automated tests for search, family profiles, and Telegram callbacks.
2. WhatsApp support using the same service layer.
3. Distance based nearby pharmacy sorting.
4. Real Google Maps or Mapbox integration.
5. Pharmacy partner dashboard for live stock updates.
6. Pharmacist inventory upload from CSV or invoice images.
7. Safer medicine classification for prescription-only drugs.
8. Better multilingual support for Hindi, Hinglish, and regional spellings.
9. Open-source LLM fallback later, only for low-confidence intent understanding.
10. Local or low-cost open-source models such as Llama, Mistral, Gemma, or Indic language models, depending on hosting cost and accuracy.
11. Refill reminders for chronic medicines.
12. Family health cabinet with saved recurring medicines.
13. Order handoff to pharmacy WhatsApp or partner checkout.
14. Admin analytics for most searched medicines and shortage trends.

The future AI direction should stay cost-conscious. The current MVP uses lightweight rules and Fuse.js. A cheaper open-source LLM can be added later only where rules are not enough.

## Authors

Built and maintained in the MediFast bot fork by [Ruchin Audichya](https://github.com/Ruchin-Audichya).

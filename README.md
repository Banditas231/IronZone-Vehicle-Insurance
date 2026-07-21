# IronZone Vehicle Insurance

Server-side **soft vehicle insurance** for DayZ servers using **DayZ Expansion Quests** (NPC Peter).

Claim recovers the same vehicle type and mechanical parts only — **no cargo**.

## Get the mod

| What | Where |
|------|--------|
| **Mod (PBO) — subscribe / download** | **[Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3768859014)** |
| **Quest JSON examples** | This repo → folder [`quest_examples/`](quest_examples/) |
| **Docs & license** | This repo → `README.md`, `LICENSE` |

- **Server hosts:** subscribe on Steam Workshop, add the mod to **`-serverMod=`** (clients do not need it).
- **This GitHub repo does not contain the compiled PBO** — only documentation and quest profile examples.

## Requirements

- Community Framework (CF)
- DayZ Expansion: **Core**, **Vehicles**, **Market**, **Garage**, **Quests**
- Load as **`-serverMod=`** (clients do not need this mod)

## Install

1. Add the mod to your server `-serverMod=` line.
2. Copy quest examples into Expansion Quests profiles:
   - `profiles/ExpansionMod/Quests/Quests/` → `920-Vehicle_Insurance_Buy.json`, `921-Vehicle_Insurance_Claim.json`
   - `profiles/ExpansionMod/Quests/Objectives/Travel/` → `20-Vehicle_Insurance_Buy.json`, `21-Vehicle_Insurance_Claim.json`
   
   Both quests use **Travel** objectives only. Do **not** add any Collection objective for buy — payment is handled by the mod.
3. Restart the server. Config is created at:
   - `profiles/IronZone_Insurance/config.json`
4. Check server log for: `[IronZone_Insurance] Config loaded` and `System initialized.`

Quest examples are in this repository under `quest_examples/` (when published) or ship with the release package.

## Player usage

1. Own a vehicle and carry a **paired** `ExpansionCarKey` for that vehicle (not an admin key).
2. Bring the vehicle near **Peter** → take quest **920** → turn in to buy insurance.
3. If the insured vehicle is **truly gone** → quest **921** to claim soft recovery + **1× new ExpansionCarKey**.
4. After a successful claim, buy insurance again — the policy is used up.

## Price

```
cost = Round(Expansion Market MaxPriceThreshold × InsurancePricePercent / 100)
```

- Uses market **file** prices (`MaxPriceThreshold`), not trader-zone UI markup.
- Classnames are matched **case-insensitively** (e.g. `Offroad_02` → `offroad_02`).
- If the classname is not in market: `FallbackInsurancePrice` × percent.

Payment is taken by the mod (Expansion Market money). Quest **920** and **921** are both **Travel** objectives (see Peter). No Collection / banknote turn-in objective.

## Claim rules (anti-abuse)

Claim is **denied** when:

- the insured vehicle still exists on the map (including ruined wreck);
- it is in **Expansion Virtual Garage**;
- it is under **Expansion Vehicle Cover** (camo);
- the recovery position (`LastPosition`) is occupied by another transport or cover placeholder.

## Config (`profiles/IronZone_Insurance/config.json`)

| Key | Meaning |
|-----|---------|
| QuestBuyId / QuestClaimId | Default 920 / 921 |
| SearchRadius | Metres to nearest vehicle for buy |
| InsurancePricePercent | % of market MaxPriceThreshold |
| FallbackInsurancePrice | Used if classname not in market |
| UseGarageMaxPolicies | Sync policy limit with Virtual Garage max |
| ExpirationDays | Soft policy timer (mission uptime) |
| SpawnWithFuel / SpawnPristine / RestoreAttachments | Claim spawn options |
| EnableLog | Writes `profiles/IronZone_Insurance/insurance.log` |

## Weekly quests note

If your mission uses a custom weekly quest counter in `init.c`, exclude service quests **920** and **921** (same as repair/medical service quests), so insurance does not count toward weekly rewards.

## License

See **`LICENSE`** in this repository.

- Free to use on DayZ servers and to study / modify for your server.
- **Not for sale** — do not sell or commercially redistribute this mod as a product.
- Contact: https://github.com/Banditas231

## Credits

IronZone / Banditas (Kestutis).

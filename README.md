# API Combat - Bruno Collection

A [Bruno](https://www.usebruno.com) API collection for playing API Combat entirely from your API client.

## Quick Start

1. Open Bruno
2. **Open Collection** → navigate to this `bruno-collection/` folder
3. Select the **Production** environment (top-right dropdown)
4. Run **Auth > Register** (or **Login** if you already have an account)
5. Your JWT token is saved automatically — all other requests will use it

## Environments

| Environment | Base URL |
|------------|----------|
| Production | `https://apicombat.com/api/v1` |
| Development | `https://localhost:7064/api/v1` |

## The Game Loop

Play through these requests in order:

### 1. Register or Login
**Auth > Register** — creates your account and saves the JWT token automatically.

Already have an account? Use **Auth > Login** instead.

### 2. Check Your Profile
**Player > Get Profile** — see your level, currency, rating, and tier.

New accounts start with 500g and 3 starter units.

### 3. Browse the Shop
**Player > Browse Shop** — see all available units with their stats and costs.

Each unit has a class (Warrior, Mage, Ranger, Healer, Tank, Assassin), base stats (HP, ATK, DEF, SPD), and abilities.

### 4. Check Your Roster
**Player > Get Roster** — see your owned units and their current stats.

Want more units? Use **Player > Unlock Unit** with a `templateUnitId` from the shop.

### 5. Create a Team
**Player > Create Team** — assemble up to 5 units into a battle squad.

Paste unit IDs from your roster into the `unitIds` array. Set a strategy:

| Formation | Style |
|-----------|-------|
| `aggressive` | High damage, low survivability |
| `defensive` | Tanky, slow but steady |
| `balanced` | Middle ground |

Target priorities tell your units who to attack first: `lowest_hp`, `healers`, `highest_attack`, `highest_defense`, `random`.

### 6. Queue a Battle
**Battle > Queue Battle** — enter the matchmaking queue.

Modes: `casual` (no rating change) or `ranked` (rating goes up/down).

The `battleId` is saved automatically for the next steps.

### 7. Check Battle Status
**Battle > Battle Status** — poll until status is `Completed`.

Battles resolve server-side in seconds. Status goes: `Queued` → `InProgress` → `Completed`.

### 8. View Results
**Battle > Battle Results** — get the full turn-by-turn combat log.

See who won, rating changes, gold earned, XP gained, and every action that happened.

### 9. Repeat!
Tweak your strategy, try different formations, unlock new units, climb the ranks.

## Strategy Deep Dive

The `strategy` object on your team controls how your units fight:

```json
{
  "formation": "balanced",
  "targetPriority": ["lowest_hp", "healers"],
  "abilities": {
    "Healing Light": {
      "when": "ally_hp_below_50",
      "target": "lowest_ally_hp"
    },
    "Fireball": {
      "when": "enemy_count_gte_2",
      "target": "all_enemies"
    }
  }
}
```

**Ability conditions** (`when`): `always`, `ally_hp_below_50`, `ally_hp_below_25`, `enemy_count_gte_2`, `enemy_count_gte_3`, `cooldown_ready`

**Ability targets** (`target`): `priority`, `lowest_ally_hp`, `self`, `all_enemies`, `all_allies`

## Rating Tiers

Your Arena Power Index (API) determines your tier:

| Tier | Vibe |
|------|------|
| Rubber Duck | You just registered. Welcome. |
| Copy Pasta | You can copy-paste curl commands. Congrats. |
| Code Monkey | You wrote an actual client. Respect. |
| Bug Hunter | You exploit edge cases for fun. |
| 10x Dev | Consistently winning. Scary good. |
| Wizard | Top-tier. Your client basically plays itself. |
| I Use Arch btw | Peak. You have transcended. |

## Resources

- **API Docs**: https://apicombat.com/api-docs/v1
- **OpenAPI Spec**: https://apicombat.com/openapi/v1.json
- **Leaderboard**: https://apicombat.com/Leaderboard
- **Discord**: https://discord.gg/jfSCSfAN49
- **GitHub**: https://github.com/api-combat-game

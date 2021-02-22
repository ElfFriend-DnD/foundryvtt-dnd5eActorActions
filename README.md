# Character Actions 5e

![Latest Release Download Count](https://img.shields.io/badge/dynamic/json?label=Downloads@latest&query=assets%5B1%5D.download_count&url=https%3A%2F%2Fapi.github.com%2Frepos%2FElfFriend-DnD%2Ffoundryvtt-dnd5eCharacterActions%2Freleases%2Flatest)
![Forge Installs](https://img.shields.io/badge/dynamic/json?label=Forge%20Installs&query=package.installs&suffix=%25&url=https%3A%2F%2Fforge-vtt.com%2Fapi%2Fbazaar%2Fpackage%2Fcharacter-actions-list-5e&colorB=4aa94a)
![Foundry Core Compatible Version](https://img.shields.io/badge/dynamic/json.svg?url=https%3A%2F%2Fraw.githubusercontent.com%2FElfFriend-DnD%2Ffoundryvtt-dnd5eCharacterActions%2Fmain%2Fsrc%2Fmodule.json&label=Foundry%20Version&query=$.compatibleCoreVersion&colorB=orange)
[![ko-fi](https://img.shields.io/badge/-buy%20me%20a%20coke-%23FF5E5B)](https://ko-fi.com/elffriend)
[![patreon](https://img.shields.io/badge/-patreon-%23FF424D)](https://www.patreon.com/ElfFriend_DnD)


This module provides a placable reusable "component" which details all of the actions a given Character Actor can take, intending to replicate the Actions Tab of the D&DBeyond character sheet. The module has two ways in which it can be used: it will either inject the actions tab itself, or another module can leverage the API it provides and use that to inject the proper HTML wherever it desires.

## Installation

Module JSON:

```
https://github.com/ElfFriend-DnD/foundryvtt-dnd5eCharacterActions/releases/latest/download/module.json
```

## Options

| **Name**                                  | Description                                                                                                                              |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Limit Actions to Cantrips**             | Instead of showing all spells that deal damage in the Actions tab, limit it to only cantrips. This is the default D&DBeyond behavior.    |
| **Include Minute-long Spells as Actions** | Include spells with a duration of one minute and an activation time of 1 Action or 1 Bonus Action in the Actions tab (e.g. Bless, Bane). |

## API

After the hook `CharacterActions5eReady` is fired, the following api methods are expected to be available on the `globalThis`: `CAL5E`:

### `async renderActionsList(actorData: Actor5eCharacter, appId: number): HTMLElement`

Returns the output of `renderTemplate` (an `HTMLElement`) after getting the provided actor's action data. This can then be injected wherever in your own DOM.

### Example:

```ts
class MyCoolCharacterSheet extends ActorSheet5e {
  
  // other stuff your sheet module does...

  async activateListeners(html) {
    try {
      const actionsTab = html.find('.actions');

      const actionsTabHtml = $(await CAL5E.renderActionsList(this.actor, this.appId));

      actionsTab.html(actionsTabHtml);
    } catch (e) {
      log(true, e);
    }

    super.activateListeners(html);
  }
}
```


### Compatibility

I'm honestly not sure how well this will play with modules that affect character sheets or dice rolls, I'll try to test as many as possible but if something is obviously breaking please create and issue here and I'll see what I can do.

| **Name**                                                                                            |       Works        | Notes                                                             |
| --------------------------------------------------------------------------------------------------- | :----------------: | ----------------------------------------------------------------- |
| [Better Rolls 5e](https://github.com/RedReign/FoundryVTT-BetterRolls5e)                             | :white_check_mark: | Compatible with version 1.3.7+.                                   |
| [Midi-QOL](https://gitlab.com/tposney/midi-qol)                                                     | :white_check_mark: | Works as expected.                                                |
| [Minimal Roll Enhancements](https://github.com/schultzcole/FVTT-Minimal-Rolling-Enhancements-DND5E) | :white_check_mark: | Works as expected.                                                |
| [Mars 5e](https://github.com/Moerill/fvtt-mars-5e)                                                  | :white_check_mark: | Works as expected.                                                |
| [FoundryVTT Magic Items](https://gitlab.com/riccisi/foundryvtt-magic-items)                         |      :shrug:       | Spells assigned to magic items do not appear in the Actions List. |
| [Inventory+](https://github.com/syl3r86/inventory-plus)                                             | :white_check_mark: | Inventory+ organization has no effect on Actions Tab              |

## Known Issues
- Using an item which changes charges or spell slots on any sheet that does not natively implement CharacterActions causes the tab to change.

## Acknowledgements

Bootstrapped with Nick East's [create-foundry-project](https://gitlab.com/foundry-projects/foundry-pc/create-foundry-project).

Mad props to the [League of Extraordinary FoundryVTT Developers](https://forums.forge-vtt.com/c/package-development/11) community which helped me figure out a lot.

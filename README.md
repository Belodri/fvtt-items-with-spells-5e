This module has been created by [Calego](https://github.com/ElfFriend-DnD/foundryvtt-items-with-spells-5e) and previously been maintained by [Zhell](https://github.com/krbz999/foundryvtt-items-with-spells-5e) and [MPMB](https://github.com/morepurplemorebetter). This is a fork for use in my personal games only.

# Items with Spells D&D 5e
This module aims to allow users to attach spells to items in such a way that when the parent item is added to an actor, the spells attached come with, pre-configured with consumption overrides and other small QOL changes.

This module only works with D&D 5e.

It also adds a new display of spell-attached items to the Spellbook.

https://user-images.githubusercontent.com/7644614/190871810-824db216-56fa-4411-9e03-ada2a9c0f251.mp4
> <sup>Demonstration of adding "Web" to a "Wand of Web", adding it to an actor and having it display in its spellbook.
Beware that this video is outdated, the character sheets look different and more options have been added in the dialogs of this module.</sup>

&nbsp;

## Compatibility with other modules
This module is intended to be compatible with the following, but integration is still ongoing:
- [Tidy 5e Sheets](https://github.com/kgar/foundry-vtt-tidy-5e-sheets) by kgar
- [Token Action HUD D&D 5e](https://github.com/Larkinabout/fvtt-token-action-hud-dnd5e) by larkinabout

&nbsp;

# Features
This module does not replace or augment any core item workflow logic, only allows a way to signify that some items also add spells to actors when added to an actor.

The scope of this module is intentionally limited and will not fully replace the edge-cases that [Magic Items](https://github.com/PwQt/magic-items-2) attempts to accommodate.

Where Magic Items adds a link to the spell, this module does a regular import of a spell to the actor sheet. As a result, there are less things that can break in the system and/or other modules. See which works best for you.

### Recommended use cases
- Magic items which have spells attached to them.
- Features which specifically add spells that have their own limited uses, not meant to be used with spell slots.

### Not recommended for
- Class spell lists
- Subclass specific spells (e.g. "always prepared" spells)

&nbsp;

## 1. Adding spells to items
![Drag and Drop the "Web" spell from the SRD Compendium to add it to a "Wand of Web" item.](https://user-images.githubusercontent.com/7644614/190871191-9255a1af-c784-41a3-a9f2-722fc90cef26.png)
> <sup>Drag and Drop the "Web" spell from the SRD Compendium to add it to a "Wand of Web" item.</sup>

A "Spells" tab is added to each item sheet, except for spells.

This tab allows spells to be dragged and dropped into it from either the items tab on the sidebar or any loaded compendium.
**Do not drop spells from an actor (i.e. from a character sheet)!**

&nbsp;

## 2. Configuring Overrides
This only works for items that are not owned by an actor (i.e. on a character sheet). Drag the item into the sidebar before trying to set these overrides. On actor-owned items, the "Edit" button will only open the spell's sheet.

![Demonstration of overrides for the Wand of Web.](https://user-images.githubusercontent.com/7644614/190871004-22077815-bbcc-4348-b6d6-b8cb033813fc.png)
> <sup>Demonstration of overrides for the Wand of Web.</sup>

Once the Spell is added to the item, clicking on the "Edit" button will open the spell override configuration.

The override configuration allows an item's spells to be configured in the two primary ways that RAW D&D 5e makes spells possible to use on items, either as limited used per spell or by using charges from the main item.

### 2.1. Limited uses per spell independently
The "Limited Uses" override any setting on the spell itself.
Set this as you would normally for an item, e.g. `1` available use of `2` maximum uses per `Day`.
Leave this empty if the spell has no individual limited uses.

`Be aware, this module does not add the ability for attached spells to be cast using spell slots. The spell will only consume its own limited uses and/or those of the parent item.`

### 2.2. Charges consumed of the "parent" item
The "Charges" section is there to define if and how many uses of the parent item are deducted when casting the spell.
If the spell can be cast at a higher level by using more charges of the magic item, enable the "Resource Scaling" checkbox (not depicted in the old screenshot above).

### 2.3 Saving throw override
If an item's spell has a specified DC calculation rules, change the "vs DC" input accordingly. Be sure to change the `Spellcasting` dropdown to `Flat` if the DC is a flat DC.
Set this as you would normally set the saving throw for a spell, for example vs DC. `15` `Flat` for Wand of Web casting "Web".

## 3. Items with spells on actors
![Demonstration of "Web" from "Wand of Web" displayed on the spellbook tab of an actor sheet.](https://user-images.githubusercontent.com/7644614/190870987-ba49d749-47cb-49f7-91d7-1f869f2f8190.png)
> <sup>Demonstration of "Web" from "Wand of Web" displayed on the spellbook tab of an actor sheet.</sup>

When an item with spells attached is added to an actor (character sheet), the attached spells are also added. Any overrides configured are applied at this moment.

The spells of an item will appear in a custom section per item.

`Be aware that if you change the overrides on an item in the sidebar, these changes do not get applied to copies of that items owned by actors. You will have to delete the item and add it to the actor again for the overrides to take effect. Alternatively, you can edit the spells on the actors directly.`

### 3.1 Spells only visible when applicable
The spells won't be visible on the spellbook unless the item is `equipped`, `identified`, and `attuned (or doesn't require attunement)`.

You can disable checking the equipment status in the module's settings (default is to hide when unequipped).

### 3.2 Using a spell
The spells can be used as normal. The core system's consumption behavior takes over at this point.
Editing a spell that is already on an actor can be done by editing the spell from the spellbook tab.

&nbsp;

# API
When programmatically deleting items that have spells attached, it is possible to skip the prompt for the user by providing the following object in the [DocumentModificationContext](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#delete) option of the `delete` method.

```js
someItem.delete({
  itemsWithSpells5e: {alsoDeleteChildSpells: true} // or false
})
```

The logic within this module will respect the boolean provided, deleting the child item spells itself if `true`, or leaving them alone if `false`.

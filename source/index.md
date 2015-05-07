# Enums

These enums are stored in the language independent [minecraft-data](https://github.com/PrismarineJS/minecraft-data) project,
 and accessed through [node-minecraft-data](https://github.com/PrismarineJS/node-minecraft-data).
 
## mineflayer.data
Provide access to the full [node-minecraft-data](https://github.com/PrismarineJS/node-minecraft-data) module
(it is possible to use this module by requiring it, but mineflayer.data is the version used by mineflayer)

## mineflayer.blocks
blocks indexed by id

## mineflayer.items
items indexed by id

## mineflayer.materials

The key is the material. The value is an object with the key as the item id
of the tool and the value as the efficiency multiplier.

## mineflayer.recipes
recipes indexed by id

## mineflayer.instruments
instruments indexed by id

## mineflayer.biomes
biomes indexed by id

## mineflayer.entities
entities indexed by id

# Classes

## mineflayer.vec3

See [andrewrk/node-vec3](https://github.com/andrewrk/node-vec3)

All points in mineflayer are supplied as instances of this class.

 * x - south
 * y - up
 * z - west

Functions and methods which require a point argument accept `Vec3` instances
as well as an array with 3 values, and an object with `x`, `y`, and `z`
properties.

## mineflayer.Location

## mineflayer.Entity

Entities represent players, mobs, and objects. They are emitted
in many events, and you can access your own entity with `bot.entity`.

### entity.id

### entity.type

Choices:

 * `player`
 * `mob`
 * `object`
 * `global` - lightning
 * `orb` - experience orb.

### entity.username

If the entity type is `player`, this field will be set.

### entity.mobType

If the entity type is `mob`, this field will be set.

### entity.displayName

Field set for mob and object. A long name in multiple words.

### entity.entityType

Field set for mob and object. The numerical type of the entity (1,2,...)

### entity.kind

Field set for mob and object. The kind of entity (for example Hostile mobs, Passive mobs, NPCs).

### entity.name

Field set for mob and object. A short name for the entity.

### entity.objectType

If the entity type is `object`, this field will be set.

### entity.count

If the entity type is `orb`, this field will be how much experience you
get from collecting the orb.

### entity.position

### entity.velocity

### entity.yaw

### entity.pitch

### entity.height

### entity.onGround

### entity.equipment[5]

 * `0` - held item
 * `1` - head
 * `2` - torso
 * `3` - legging
 * `4` - shoes

### entity.heldItem

Equivalent to `entity.equipment[0]`.

### entity.metadata

See http://wiki.vg/Entities#Entity_Metadata_Format for more details.

## mineflayer.Block

### block.position

Vec3 instance.

### block.type

Numerical id.

### block.name

### block.displayName

### block.metadata

Number which represents different things depending on the block.
See http://www.minecraftwiki.net/wiki/Data_values#Data

### block.light

### block.skyLight

### block.hardness

### block.biome

A biome instance. See `Biome`.

### block.signText

If the block is a sign, contains the sign text.

### block.painting

If the block is a painting, contains information about the painting.

 * `id`
 * `position`
 * `name`
 * `direction` - direction vector telling how the painting is facing.

### block.diggable

Boolean, whether the block is considered diggable.

### block.boundingBox

The shape of the block according to the physics engine's collision decection. Currently one of:

 * `block` - currently, partially solid blocks, such as half-slabs and ladders, are considered entirely solid.
 * `empty` - such as flowers and lava.

### block.material

This tells what types of tools will be effective against the block. Possible
values are: `null`, `rock`, `wood`, `plant`, `melon`, `leaves`, `dirt`, `web`, and `wool`.

See http://www.minecraftwiki.net/wiki/Digging and the `toolMultipliers`
variable at the top of lib/plugins/digging.js for more info.

### block.harvestTools

The set of tools that will allow you to harvest the block.

### block.drops

The blocks or items dropped by that block.

## mineflayer.Biome

### biome.id

Numerical id.

### biome.color

### biome.height

### biome.name

### biome.rainfall

### biome.temperature

## mineflayer.Item

### item.type

Numerical id.

### item.count

### item.metadata

Number which represents different things depending on the item.
See http://www.minecraftwiki.net/wiki/Data_values#Data

### item.nbt

Buffer.

### item.name

### item.displayName

### item.stackSize

## mineflayer.windows.Window (base class)

### window.id

### window.type

### window.title

"Inventory", "Chest", "Large chest", "Crafting", "Furnace", or "Trap"

### window.slots

Map of slot index to `Item` instance.

### window.selectedItem

In vanilla client, this is the item you are holding with the mouse cursor.

### window.findInventoryItem(itemType, metadata, [notFull])

 * `itemType` - numerical id that you are looking for
 * `metadata` -  metadata value that you are looking for. `null`
   means unspecified.
 * `notFull` - (optional) - if `true`, means that the returned
   item should not be at its `stackSize`.

### window.count(itemType, [metadata])

Returns how many you have in the inventory section of the window.

 * `itemType` - numerical id that you are looking for
 * `metadata` - (optional) metadata value that you are looking for.
   defaults to unspecified

### window.items()

Returns a list of `Item` instances from the inventory section of the window.

### window.emptySlotCount()

### window "windowUpdate" (slot, oldItem, newItem)

Fired whenever any slot in the window changes for any reason.
Watching `bot.inventory.on("windowUpdate")` is the best way to watch for changes in your inventory.

 * `slot` - index of changed slot.
 * `oldItem`, `newItem` - either an [`Item`](#mineflayeritem) instance or `null`.

`newItem === window.slots[slot]`.

## mineflayer.windows.InventoryWindow
## mineflayer.windows.ChestWindow
## mineflayer.windows.CraftingTableWindow
## mineflayer.windows.FurnaceWindow
## mineflayer.windows.DispenserWindow
## mineflayer.windows.EnchantmentTableWindow
## mineflayer.windows.BrewingStandWindow

## mineflayer.Recipe

### Recipe.find(itemType, [metadata])

Returns a list of matching `Recipe` instances.

 * `itemType` - numerical id
 * `metadata` - metadata to match. `null` means match anything.

### recipe.result

The output item. It's a recipeItem :
```js
{
  id:45,
  metadata:3,
  count:1
}
```

### recipe.inShape

Looks like this:

```js
[
  [recipeItem, recipeItem],
  [recipeItem, recipeItem],
  [recipeItem, recipeItem],
]
```

### recipe.outShape

Looks the same as `inShape`. Only relevant for cake.

### recipe.ingredients

List of shape-independent ingredients. Looks like this:

```js
[
  recipeItem,
  recipeItem
]
```

### recipe.requiresTable

Boolean.

### recipe.delta

Map of item type to how much more or less you will have after you use
the recipe.

This is what it looks like for the chest recipe:

```js
[
  recipeItem,
  recipeItem
]
```

## mineflayer.Chest

Represents a single session of opening and closing a chest.
See `bot.openChest(chestBlock)`.

### chest.window

If the chest is open, this property is a `ChestWindow` instance.
If the chest is closed, this property is `null`.

### chest "open"

Fires when the chest has successfully been opened.

### chest "close"

Fires when the chest closes.

### chest "updateSlot" (oldItem, newItem)

Fires when the chest you are looking at is updated.

### chest.close()

### chest.deposit(itemType, metadata, count, [callback])

 * `itemType` - numerical item id
 * `metadata` - numerical value. `null` means match anything.
 * `count` - how many to deposit. `null` is an alias to 1.
 * `callback(err)` - (optional) - called when done depositing

### chest.withdraw(itemType, metadata, count, [callback])

 * `itemType` - numerical item id
 * `metadata` - numerical value. `null` means match anything.
 * `count` - how many to withdraw. `null` is an alias to 1.
 * `callback(err)` - (optional) - called when done withdrawing

### chest.count(itemType, [metadata])

Return how many of a certain type of item are in the chest.

 * `itemType` - numerical item id
 * `metadata` - (optional) numerical value. `null` means match anything.

### chest.items()

Returns a list of `Item` instances from the chest.

## mineflayer.Furnace

See `bot.openFurnace(furnaceBlock)`.

### furnace "open"

Fires when the furnace has successfully been opened.

### furnace "close"

Fires when the furnace closes.

### furnace "update"

Fires when `furnace.fuel` and/or `furnace.progress` update.

### furnace "updateSlot" (oldItem, newItem)

Fires when a slot in the furnace you have open has updated.

### furnace.close()

### furnace.takeInput([callback])

 * `callback(err, item)`

### furnace.takeFuel([callback])

 * `callback(err, item)`

### furnace.takeOutput([callback])

 * `callback(err, item)`

### furnace.putInput(itemType, metadata, count, [cb])

### furnace.putFuel(itemType, metadata, count, [cb])

### furnace.inputItem()

Returns `Item` instance which is the input.

### furnace.fuelItem()

Returns `Item` instance which is the fuel.

### furnace.outputItem()

Returns `Item` instance which is the output.

### furnace.fuel

How much fuel is left between 0 and 1.

### furnace.progress

How much cooked the input is between 0 and 1.

## mineflayer.Dispenser

See `bot.openDispenser(dispenserBlock)`.

### dispenser "open"

Fires when the dispenser has successfully been opened.

### dispenser "close"

Fires when the dispenser closes.

### dispenser "updateSlot" (oldItem, newItem)

Fires when a slot in the dispenser you have open has updated.

### dispenser.close()

### dispenser.items()

Returns a list of `Item` instances from the dispenser.

### dispenser.deposit(itemType, metadata, count, [callback])

 * `itemType` - numerical item id
 * `metadata` - numerical value. `null` means match anything.
 * `count` - how many to deposit. `null` is an alias to 1.
 * `callback(err)` - (optional) - called when done depositing

### dispenser.withdraw(itemType, metadata, count, [callback])

 * `itemType` - numerical item id
 * `metadata` - numerical value. `null` means match anything.
 * `count` - how many to withdraw. `null` is an alias to 1.
 * `callback(err)` - (optional) - called when done withdrawing

### dispenser.count(itemType, [metadata])

Return how many of a certain type of item are in the dispenser.

 * `itemType` - numerical item id
 * `metadata` - (optional) numerical value. `null` means match anything.

## mineflayer.EnchantmentTable

See `bot.openEnchantmentTable(enchantmentTableBlock)`.

### enchantmentTable "open"

Fires when the enchantment table has successfully been opened.

### enchantmentTable "close"

Fires when the enchantment table closes.

### enchantmentTable "updateSlot" (oldItem, newItem)

Fires when a slot in the enchantment table you have open has updated.

### enchantmentTable "ready"

Fires when `enchantmentTable.enchantments` is fully populated and you
may make a selection by calling `enchantmentTable.enchant(choice)`.

### enchantmentTable.close()

### enchantmentTable.targetItem()

Gets the target item. This is both the input and the output of the
enchantment table.

### enchantmentTable.enchantments

Array of length 3 which are the 3 enchantments to choose from.
`level` can be `null` if the server has not sent the data yet.

Looks like:

```js
[
  {
    "level": 3
  },
  {
    "level": 4
  },
  {
    "level": 9
  }
]
```

### enchantmentTable.enchant(choice, [callback])

 * `choice` - [0-2], the index of the enchantment you want to pick.
 * `callback(err, item)` - (optional) called when the item has been enchanted

### enchantmentTable.takeTargetItem([callback])

 * `callback(err, item)`

### enchantmentTable.putTargetItem(item, [callback])

 * `callback(err)`

# Bot

## Properties

### bot.entity

Your own entity. See `Entity`.

### bot.entities

All nearby entities. This object is a map of entityId to entity.

### bot.username

Use this to find out your own name.

### bot.spawnPoint

Coordinates to the main spawn point, where all compasses point to.

### bot.game.levelType

### bot.game.dimension

### bot.game.difficulty

### bot.game.gameMode

### bot.game.hardcore

### bot.game.worldHeight

### bot.game.maxPlayers

### bot.players

Map of username to people playing the game. A player looks like this:

```js
{
  username: 'player',
  ping: 28,
  entity: entity, // null if you are too far away
}
```

### bot.isRaining

### bot.chatPatterns

This is an array of pattern objects, of the following format:
{ /regex/, "chattype", "description")
 * /regex/ - a regular expression pattern, that should have at least two capture groups
 * 'chattype' - the type of chat the pattern matches, ex "chat" or "whisper", but can be anything.
 * 'description' - description of what the pattern is for, optional.

### bot.settings.chat

Choices:

 * `enabled` (default)
 * `commandsOnly`
 * `disabled`

### bot.settings.colorsEnabled

Default true, whether or not you receive color codes in chats from the server.

### bot.settings.viewDistance

Choices:
 * `far` (default)
 * `normal`
 * `short`
 * `tiny`

### bot.settings.difficulty

Same as from server.properties.

### bot.settings.showCape

If you have a cape you can turn it off by setting this to false.

### bot.experience.level

### bot.experience.points

Total experience points.

### bot.experience.progress

Between 0 and 1 - amount to get to the next level.

### bot.health

Number in the range [0, 20] representing the number of half-hearts.

### bot.food

Number, in the range [0, 20] representing the number of half-turkey-legs.

### bot.foodSaturation

Food saturation acts as a food "overcharge". Food values will not decrease
while the saturation is over zero. Players logging in automatically get a
saturation of 5.0. Eating food increases the saturation as well as the food bar.


### bot.physics

Edit these numbers to tweak gravity, jump speed, terminal velocity, etc.
Do this at your own risk.

### bot.time.day

Time of the day, in ticks.

Time is based on ticks, where 20 ticks happen every second. There are 24000
ticks in a day, making Minecraft days exactly 20 minutes long.

The time of day is based on the timestamp modulo 24000. 0 is sunrise, 6000
is noon, 12000 is sunset, and 18000 is midnight.

### bot.time.age

Age of the world, in ticks.

### bot.quickBarSlot

Which quick bar slot is selected (0 - 8).

### bot.inventory

A `Window` instance representing your inventory.

### bot.targetDigBlock

The `block` that you are currently digging, or `null`.

### bot.isSleeping

Boolean, whether or not you are in bed.

## Events

### "chat" (username, message, translate, jsonMsg, matches)

Only emitted when a player chats publicly.

 * `username` - who said the message (compare with `bot.username` to ignore your own chat)
 * `message` - stripped of all color and control characters
 * `translate` - chat message type. Null for most bukkit chat messages
 * `jsonMsg` - unmodified JSON message from the server
 * `matches` - array of returned matches from regular expressions. May be null

### "whisper" (username, message, translate, jsonMsg, matches)

Only emitted when a player chats to you privately.

 * `username` - who said the message
 * `message` - stripped of all color and control characters
 * `translate` - chat message type. Null for most bukkit chat messages
 * `jsonMsg` - unmodified JSON message from the server
 * `matches` - array of returned matches from regular expressions. May be null

### "message" (jsonMsg)

Emitted for every server message, including chats.

 * `jsonMsg` - unmodified JSON message from the server

### "login"

Fires after you successfully login to the server.
You probably want to wait for the `spawn` event
before doing anything though.

### "spawn"

Emitted once after you log in and spawn for the first time
and then emitted when you respawn after death.

This is usually the event that you want to listen to
before doing anything on the server.

### "respawn"

Emitted when you change dimensions and just before you spawn.
Usually you want to ignore this event and wait until the "spawn"
event instead.

### "game"

Emitted when the server changes any of the game properties.

### "rain"

Emitted when it starts or stops raining. If you join a
server where it is already raining, this event will fire.

### "time"

Emitted when the server sends a time update. See `bot.time`.

### "kicked" (reason)

Emitted when the bot is kicked from the server. `reason`
is a string explaining why you were kicked.

### "end"

Emitted when you are no longer connected to the server.

### "spawnReset"

Fires when you cannot spawn in your bed and your spawn point gets reset.

### "death"

Fires when you die.

### "health"

Fires when your hp or food change.

### "entitySwingArm" (entity)
### "entityHurt" (entity)
### "entityWake" (entity)
### "entityEat" (entity)
### "entityCrouch" (entity)
### "entityUncrouch" (entity)
### "entityEquipmentChange" (entity)
### "entitySleep" (entity)
### "entitySpawn" (entity)
### "playerCollect" (collector, collected)

An entity picked up an item.

 * `collector` - entity that picked up the item.
 * `collected` - the entity that was the item on the ground.

### "entityGone" (entity)
### "entityMoved" (entity)
### "entityDetach" (entity, vehicle)
### "entityAttach" (entity, vehicle)

An entity is attached to a vehicle, such as a mine cart
or boat.

 * `entity` - the entity hitching a ride
 * `vehicle` - the entity that is the vehicle

### "entityUpdate" (entity)
### "entityEffect" (entity, effect)
### "entityEffectEnd" (entity, effect)
### "playerJoined" (player)
### "playerLeft" (player)

### "blockUpdate" (oldBlock, newBlock)

Fires when a block updates. Both `oldBlock` and `newBlock` provided for
comparison.

Note that `oldBlock` may be `null`.

### "blockUpdate:(x, y, z)" (oldBlock, newBlock)

Fires for a specific point. Both `oldBlock` and `newBlock` provided for
comparison.

Note that `oldBlock` may be `null`.

### "chunkColumnLoad" (point)
### "chunkColumnUnload" (point)

Fires when a chunk has updated. `point` is the coordinates to the corner
of the chunk with the smallest x, y, and z values.

### "soundEffectHeard" (soundName, position, volume, pitch)

Fires when the client hears a sound effect.

 * `soundName`: name of the sound effect
 * `position`: a Vec3 instance where the sound originates
 * `volume`: floating point volume, 1.0 is 100%
 * `pitch`: integer pitch, 63 is 100%

### "noteHeard" (block, instrument, pitch)

Fires when a note block goes off somewhere.

 * `block`: a Block instance, the block at emitted the noise
 * `instrument`:
   - `id`: integer id
   - `name`: one of [`harp`, `doubleBass`, `snareDrum`, `sticks`, `bassDrum`].
 * `pitch`: The pitch of the note (between 0-24 inclusive where 0 is the
   lowest and 24 is the highest). More information about how the pitch values
   correspond to notes in real life are available on the
   [official Minecraft wiki](http://www.minecraftwiki.net/wiki/Note_Block).

### "pistonMove" (block, isPulling, direction)

### "chestLidMove" (block, isOpen)

### "blockBreakProgressObserved" (block, destroyStage)

Fires when the client observes a block in the process of being broken.

 * `block`: a Block instance, the block being broken
 * `destroyStage`: integer corresponding to the destroy progress (0-9)

### "blockBreakProgressEnd" (block)

Fires when the client observes a block stops being broken.
This occurs whether the process was completed or aborted.

 * `block`: a Block instance, the block no longer being broken

### "diggingCompleted" (block)

 * `block` - the block that no longer exists

### "diggingAborted" (block)

 * `block` - the block that still exists

### "move"

Fires when the bot moves. If you want the current position, use
`bot.entity.position` and if you want the previous position, use
`bot.entity.position.minus(bot.entity.velocity)`.

### "mount"

Fires when you mount an entity such as a minecart. To get access
to the entity, use `bot.vehicle`.

To mount an entity, use `mount`.

### "dismount" (vehicle)

Fires when you dismount from an entity.

### "windowOpen" (window)

Fires when you begin using a workbench, chest, brewing stand, etc.

### "windowClose" (window)

Fires when you may no longer work with a workbench, chest, etc.

### "sleep"

Fires when you sleep.

### "wake"

Fires when you wake up.

### "experience"

Fires when `bot.experience.*` has updated.

## Functions

### bot.blockAt(point)

Returns the block at `point` or `null` if that point is not loaded.
See `Block`.

### bot.canDigBlock(block)

Returns whether `block` is diggable and within range.

### bot.recipesFor(itemType, metadata, minResultCount, craftingTable)

Returns a list of `Recipe` instances that you could use to craft `itemType`
with `metadata`.

 * `itemType` - numerical item id of the thing you want to craft
 * `metadata` - the numerical metadata value of the item you want to craft
   `null` matches any metadata.
 * `minResultCount` - based on your current inventory, any recipe from the
   returned list will be able to produce this many items. `null` is an
   alias for `1`.
 * `craftingTable` - a `Block` instance. If `null`, only recipes that can
   be performed in your inventory window will be included in the list.

### bot.recipesAll(itemType, metadata, craftingTable)

The same as bot.recipesFor except that it does not check wether the bot has enough materials for the recipe.

## Methods

### bot.end()

End the connection with the server.

### bot.quit(reason)

Gracefully disconnect from the server with the given reason (defaults to 'disconnect.quitting').

### bot.chat(message)

Sends a publicly broadcast chat message. Breaks up big messages into multiple chat messages as necessary.

### bot.whisper(username, message)

Shortcut for "/tell <username>". All split messages will be whispered to username.

### bot.chatAddPattern(pattern, chatType, description)

Adds a regex pattern to the bot's chat matching. Useful for bukkit servers where the chat format changes a lot.
 * `pattern` - regular expression to match chat
 * `chatType` - the event the bot emits when the pattern matches. Eg: "chat" or "whisper"
 * 'description ' - Optional, describes what the pattern is for

### bot.setSettings(options)

See the `bot.settings` property.

### bot.sleep(bedBlock, [cb])

Sleep in a bed. `bedBlock` should be a `Block` instance which is a bed. `cb` can have an err parameter if the bot cannot sleep.

### bot.wake([cb])

Get out of bed. `cb` can have an err parameter if the bot cannot wake up.

### bot.setControlState(control, state)

 * `control` - one of ['forward', 'back', 'left', 'right', 'jump', 'sprint']
 * `state` - `true` or `false`

### bot.clearControlStates()

Sets all controls to off.

### bot.lookAt(point, [force])

 * `point` - tilts your head so that it is directly facing this point.
 * `force` - See `force` in `bot.look`

### bot.look(yaw, pitch, [force])

Set the direction your head is facing.

 * `yaw` - The number of radians to rotate around the vertical axis, starting
   from due east. Counter clockwise.
 * `pitch` - Number of radians to point up or down. 0 means straight forward.
   pi / 2 means straight up. -pi / 2 means straight down.
 * `force` - If present and true, skips the smooth server-side transition.
   Specify this to true if you need the server to know exactly where you
   are looking, such as for dropping items or shooting arrows. This is not
   needed for client-side calculation such as walking direction.

### bot.updateSign(block, text)

Changes the text on the sign.

### bot.equip(item, destination, [callback])

Equips an item from your inventory.

 * `item` - `Item` instance. See `window.items()`.
 * `destination`
   - `"hand"` - `null` aliases to this
   - `"head"`
   - `"torso"`
   - `"legs"`
   - `"feet"`
 * `callback(error)` - optional. called when you have successfully equipped
   the item or when you learn that you have failed to equip the item.

### bot.unequip(destination, [callback])

Remove an article of equipment.

### bot.tossStack(item, [callback])

 * `item` - the stack of items you wish to toss
 * `callback(error)` - optional, called when tossing is done. if error is
   truthy, you were not able to complete the toss.

### bot.toss(itemType, metadata, count, [callback])

 * `itemType` - numerical id of the item you wish to toss
 * `metadata` - metadata of the item you wish to toss. Use `null`
   to match any metadata
 * `count` - how many you want to toss. `null` is an alias for `1`.
 * `callback(err)` - (optional) called once tossing is complete

### bot.dig(block, [callback])

Begin digging into `block` with the currently equipped item.
See also "diggingCompleted" and "diggingAborted" events.

Note that once you begin digging into a block, you may not
dig any other blocks until the block has been broken, or you call
`bot.stopDigging()`.

 * `block` - the block to start digging into
 * `callback(err)` - (optional) called when the block is broken or you
   are interrupted.

### bot.stopDigging()

### bot.digTime(block)

Tells you how long it will take to dig the block, in milliseconds.

### bot.staticDigTime(block,creative,heldItemType,inWater,onGround)

Tells you how long it will take to dig the block, in milliseconds.
The difference between that function and bot.digTime is that this function is static :
 it doesn't depend on the environment of the bot, you can call it at any time and get the same result.

 * `block` block to dig
 * `creative` game in creative
 * `heldItemType` the id of the held item (or null if nothing is held)
 * `inWater` the bot is in water
 * `onGround` the bot in on the ground

### bot.placeBlock(referenceBlock, faceVector)

 * `referenceBlock` - the block you want to place a new block next to
 * `faceVector` - one of the six cardinal directions, such as `new Vec3(0, 1, 0)` for the top face,
   indicating which face of the `referenceBlock` to place the block against.

The new block will be placed at `referenceBlock.position.plus(faceVector)`.

### bot.activateBlock(block)

Punch a note block, open a door, etc.

### bot.activateItem()

Activates the currently held item. This is how you eat, shoot bows, throw an egg, etc.

### bot.deactivateItem()

Deactivates the currently held item. This is how you release an arrow, stop eating, etc.

### bot.useOn(targetEntity)

Use the currently held item on an `Entity` instance. This is how you apply a saddle and
use shears.

### bot.attack(entity)

Attack a player or a mob.

### bot.mount(entity)

Mount a vehicle. To get back out, use `bot.dismount`.

### bot.dismount()

Dismounts from the vehicle you are in.

### bot.moveVehicle(left,forward)

Moves the vehicle :

 * left can take -1 or 1 : -1 means right, 1 means left
 * forward can take -1 or 1 : -1 means backward, 1 means forward

All the direction are relative to where the bot is looking at

### bot.setQuickBarSlot(slot)

 * `slot` - 0-8 the quick bar slot to select.

### bot.craft(recipe, count, craftingTable, [callback])

 * `recipe` - A `Recipe` instance. See `bot.recipesFor`.
 * `count` - How many times you wish to perform the operation.
   If you want to craft planks into `8` sticks, you would set
   `count` to `2`. `null` is an alias for `1`.
 * `craftingTable` - A `Block` instance, the crafting table you wish to
   use. If the recipe does not require a crafting table, you may use
   `null` for this argument.
 * `callback` - (optional) Called when the crafting is complete and your
   inventory is updated.

### bot.openChest(chestBlock)

Returns a `Chest` instance which represents the chest you are opening.

### bot.openFurnace(furnaceBlock)

Returns a `Furnace` instance which represents the furnace you are opening.

### bot.openDispenser(dispenserBlock)

Returns a `Dispenser` instance which represents the dispenser you are opening.

### bot.openEnchantmentTable(enchantmentTableBlock)

Returns an `EnchantmentTable` instance which represents the enchantment table
you are opening.

## Lower level inventory methods

These are lower level methods for the inventory, they can be useful sometimes but prefer the inventory methods presented above if you can.

### bot.clickWindow(slot, mouseButton, mode, cb)

Click on the current window.

### bot.putSelectedItemRange(start, end, window, slot, cb)

Put the item at `slot` in the specified range.

### bot.putAway(slot, cb)

Put the item at `slot` in the inventory.

### bot.closeWindow(window)

Close the `window`.

### bot.transfer(options, cb)

Transfer some kind of item from one range to an other. `options` is an object containing :

 * `window` : the window where the item will be moved
 * `itemType` : the type of the moved items
 * `metadata` : the metadata of the moved items
 * `sourceStart` and `sourceEnd` : the source range
 * `destStart` and `destEnd` : the dest Range

### bot.openBlock(block, Class)

Open a block, for example a chest.

 * `block` is the block the bot will open
 * `Class` is the type of window that will be opened

### bot.moveSlotItem(sourceSlot, destSlot, cb)

Move an item from `sourceSlot` to `destSlot` in the current window.

### bot.updateHeldItem()

Update `bot.heldItem`.

## bot.creative

This collection of apis is ueseful in creative mode.
Detecting and changing gamemodes is not implemented here,
but it is assumed and often required that the bot be in creative mode for these features to work.

### bot.creative.setInventorySlot(slot, item)

Gives the bot the specified item in the specified inventory slot.

 * `slot` is in inventory window coordinates (where 36 is the first quickbar slot, etc.).
 * `item` can presumably be anything, specified with arbitrary metadata, nbtdata, etc.
    If `item` is `null`, the item at the specified slot is deleted.

If this method changes anything, you can be notified via `bot.inventory.on("windowUpdate")`.

### bot.creative.flyTo(destination, [cb])

Calls `startFlying()` and moves at a constant speed through 3d space in a straight line to the destination.
`destination` is a `Vec3`, and often the `x` and `z` coordinates will end with `.5`.
This operation will not work if there is an obstacle in the way,
so it is advised to fly very short distances at a time.

When the bot arrives at the destination, `cb` is called.

This method does not attempt any path finding.
It is expected that a path finding implementation will use this method to move < 2 blocks at a time.

To resume normal physics, call `stopFlying()`.

### bot.creative.startFlying()

Sets `bot.physics.gravity` to `0`.
To resume normal physics, call `stopFlying()`.

This method is useful if you want to hover while digging the ground below you.
It is not necessary to call this function before calling `flyTo()`.

Note that while flying, `bot.entity.velocity` will not be accurate.

### bot.creative.stopFlying()

Restores `bot.physics.gravity` to it's original value.

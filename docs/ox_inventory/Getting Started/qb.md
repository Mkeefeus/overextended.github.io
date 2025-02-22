# QBCore

:::warning

QBCore support is experimental and cannot work as a "drag and drop" solution due to many incompatibilities.
:::

## Compatibility

Ox Inventory provides a complete suite of tools to replace the built-in items and inventory system from QBCore, and is not intended to be used with resources designed around it.

- Stashes used by qb-inventory and its forks do not work, but can _possibly_ be converted.
- Remove `qb-inventory` (or similar), or qb-core will not function properly.
- Remove `qb-shops` and `qb-weapons`, as they depend on `qb-inventory` and conflict.
- Weapon holstering and draw animations (like in `qb-smallresources`) may break our own methods.

## Qbox Project

Qbox is a fork of QBCore being developed by a team of former contributors and developers on QBCore. The team is focused on improving performance and security, as well as converting resources to support our resources (mainly ox_lib and ox_inventory).

We _strongly_ advise using Qbox as an alternative to QBCore.

- [Qbox Project GitHub](https://github.com/Qbox-project/)
- [Qbox Project Discord](https://discord.gg/AtbwBuJHN5)

## Installation

- Setup [qb-core](https://github.com/qbcore-framework) or [qbox](https://github.com/Qbox-project/qb-core).
- Edit your `server.cfg`.
  - Add `setr inventory:framework "qb"` before starting your resources.
  - Start ox_inventory immediately after qb-core.

### Convert QBCore

If you have existing player data, you will need to convert it to a compatible format.

- Start the server and type `convertinventory qb` into the server console.
- Restart the server once conversion is complete.

## Optional optimisation

All item related functions from Player, such as `Player.Functions.GetItemByName`, have been modified for compatibility purposes; however they are considered deprecated.

The reasoning is fairly simple - there's now additional function references and overhead to consider. Fortunately, the new Inventory functions can be used directly and offer a great deal of improvements over the old ones.

You should read through the functions section for further information, but the following should give you a decent idea.

<Tabs>
<TabItem value="qb" label="QBCore">

```lua
local acetone = Player.Functions.GetItemByName('acetone')
local antifreeze = Player.Functions.GetItemByName('antifreeze')
local sudo = Player.Functions.GetItemByName('sudo')
if acetone?.amount > 2 and antifreeze?.amount > 4 and sudo?.amount > 9 then
    Player.Functions.RemoveItem("acetone", 3)
    Player.Functions.RemoveItem("antifreeze", 5)
    Player.Functions.RemoveItem("sudo", 10)
end
```

</TabItem>
<TabItem value="inventory" label="Inventory">

Add the following code somewhere in your resource to cache the exports metatable.

```lua
local ox_inventory = exports.ox_inventory
```

You will be able to reference any functions exposed through the export.

```lua
local items = ox_inventory:Search(source, 'count', {'acetone', 'antifreeze', 'sudo'})
if items and items.acetone > 2 and items.antifreeze > 4 and items.sudo > 9 then
    ox_inventory:RemoveItem(source, 'acetone', 3)
    ox_inventory:RemoveItem(source, 'antifreeze', 5)
    ox_inventory:RemoveItem(source, 'sudo', 10)
end
```

</TabItem>
</Tabs>

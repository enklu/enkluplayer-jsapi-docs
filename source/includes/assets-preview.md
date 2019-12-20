# Assets (Preview)

```javascript
const assets = require('assets-preview');
```

The Assets API can be used to fetch `AssetData` based on Ids, Names, and Tags.

## Methods

> Get all Assets available to this experience.

```javascript
var all = assets.all();
log.info('Available Assets: ' + all.length);
```

> Get an Asset by a specific Id.

```javascript
var asset = assets.byId('756f21c0-21fc-4fd7-93af-a80750266b43');
if (asset) log.info(asset.AssetName); // Logs 'Octopus'
```

> Get all Assets matching a tag or name.

```javascript
var aquatic = assets.byTag('Aquatic')
log.info('Aquatic Assets: ' + aquatic.length);

var seashells = assets.query('SeaShell');
log.info('Seashell Assets: ' + seashells.length);
```

The returned `AssetData` has the following properties:

**Guid** - The unique Id of this Asset.

**AssetName** - The name of this Asset.

**Description** - The Asset's description.

**Owner** - The Asset's owner.

**Version** - The latest version of the Asset.

**Tags** - The tags placed on this Asset.

**CreatedAt** - The timestamp when the Asset was created.

**UpdatedAt** - The timestamp when the Asset was last updated.

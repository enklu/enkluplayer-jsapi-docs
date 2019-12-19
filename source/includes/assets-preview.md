# Assets (Preview)

```javascript
const assets = require('assets-preview');
```

The Assets API can be used to fetch `AssetData` based on Ids, Names, and Tags.

## Methods

```javascript
// Get all Assets available to this experience.
var all = assets.all();
log.info('Available Assets: ' + all.length);

// Get an Asset by a specific Id.
var asset = assets.byId('756f21c0-21fc-4fd7-93af-a80750266b43');
if (asset) log.info(asset.AssetName); // Logs 'Octopus'

var aquatic = assets.byTag('Aquatic')
log.info('Aquatic Assets: ' + aquatic.length);

var seashells = assets.query('SeaShell');
log.info('Seashell Assets: ' + seashells.length);
```

The returned `AssetData` has the following properties:

<b>Guid</b> - The unique Id of this Asset.

<b>AssetName</b> - The name of this Asset.

<b>Description</b> - The Asset's description.

<b>Owner</b> - The Asset's owner.

<b>Version</b> - The latest version of the Asset.

<b>Tags</b> - The tags placed on this Asset.

<b>CreatedAt</b> - The timestamp when the Asset was created.

<b>UpdatedAt</b> - The timestamp when the Asset was last updated.

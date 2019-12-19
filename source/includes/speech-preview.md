# Speech (Preview)

We have a preview API for Azure's Language Understanding service (LUIS). This allows users to define natural language models via LUIS and recognize those intents in Enklu behaviors.

## Azure Setup

First, setup a LUIS account at the [LUIS portal](https://www.luis.ai/home). The documentation will guide you through creating your own model. Your Azure information can then be entered in the Scene inspector.

[!Speech Inspector](../images/speech.inspector.png)

## API

```javascript
var speech = require('speech-preview');

// speech is not currently available on all platforms
if (speech.isAvailable()) {
    speech.on('onintent', function(result) {
        // an intent was recognized!
    });

    speech.on('onspeech', function(speech) {
        // an intent was not recognized but speech was
    });
}
```

Next, register for events from the `speech-preview` api. Register for either intent events, which are data structures that understand what a user _wants_, or speech events, which provide raw text data when LUIS cannot determine an intent.

## Data Objects

### IntentResult

| *query* | _string_ | The raw recognized text. |
|---|---|---|
| *topScoringIntent* | _TopScoringIntent_ | The intent ranked most likely by LUIS. |
| *entities* |_Array of Entity_ |Predefined entities reference by the intent. |

### TopScoringIntent

|*intent*|_string_|The name of the recognized intent|
|---|---|---|
|*score*|_number_|A number between 0 and 1 indicating LUIS' confidence that this intent was correctly identified.|

### Entity

|*name*|_string_|The string value of the entity.|
|---|---|---|
|*type*|_string_|The type of entity, as defined in the LUIS portal.|
|*startIndex*|_number_|The starting index into the intent query string.|
|*endIndex*|_number_|The end index into the intent query string.| 
|*score*|_number_|A number between 0 and 1 indicating LUIS' confidence that this entity is correctly identified.

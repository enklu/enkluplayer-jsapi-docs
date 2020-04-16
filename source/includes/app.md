# App

In all scripts, there is a global `app` object. This object is useful for manipulating application-wide settings, scenes inside an experience, creating new elements via scripts, and networking.

## Scene Object

> Retrieves ids of all scenes

```javascript
var all = app.scenes.all;
```
> Returns root element of a specific scene

```javascript
var root = app.scenes.root('myScene');
```

The `scenes` object has functions for manipulating multi-scene experiences.

## Element Object

> Retrieves element

```javascript
button = app.elements.byId('specific-id');
```

> Creates element as a child of another

```javascript
var a = element.create(root, 'Button');
var b = element.create(a, 'Button', 'specific-id');
```

> Creates elements from a vine, as a child of `a`

```javascript
var c = element.createFromVine(a, '<Menu><Button /></Menu>');
```

> Destroys an element

```javascript
app.elements.destroy(button);
```

The `elements` object can be used to query all scenes for an element, create elements, or destroy them.

## SAI object (Preview)

> Enables SAI

```javascript
app.sai.enabled = true;
```

> Focuses SAI on an element. An optional callback can be provided that will be invoked when SAI reaches the target element.

```javascript
app.sai.focus(button);
app.sai.focus(button, onComplete);
```

> Removes an exising focus target and tells SAI to follow the player.

```javascript
app.sai.unfocus();
```

> Displays some text next to SAI. An optional timeout can be provided that will clear the text when it expires. If the timeout is set, a callback can also be provided that will be invoked when the timeout expires.

```javascript
var message = "Hello, world.";
app.sai.write(message);
app.sai.write(message, timeoutMs);
app.sai.write(message, timeoutMs, onComplete);
```

> Displays a timed sequence of text. If a callback is provided, it will be invoked after the final text times out.

```javascript
var dialog = {
  script: [
    {
      text: "Hi, I'm SAI.",
      timeoutMs: 2000
    },
    {
      text: "It's great to meet you.",
      timeoutMs: 3000
    }
  ]
};

app.sai.write(dialog);
app.sai.write(dialog, onComplete);
```

> Speaks in plain text or SSML. An optional callback can be provided that will be invoked after speech synthesis playback completes.

```javascript
app.sai.speak(message);
app.sai.speak(message, onComplete);
```

> Teleports SAI directly to an element or back to the player

```javascript
app.sai.teleport(button);
app.sai.teleport();
```

> Plays one of SAI's animations Currently supported values are `Happy`, `Sad`, `Idle`, `Move`, `Pulse`, `Talk`, `Scan`, `Highlight`, and `Work`. 

```javascript
app.sai.animate('Happy');
```
The `sai` object can be used to control the Spatial Artificial Intelligence familiar. Use SAI to communicate essential information about a scene or direct a player's attention toward an important element.


> Changes visibility of SAI

```javascript
app.sai.schema.setBool('visible', false);
```
> Changes font size of SAI text

```javascript
app.sai.schema.setNumber('fontSize', 70f);
```
> Changes the magnet tolerance of sai when it is unfocused. SAI will readjust its position only when the gaze spot is beyond the specified threshold.  

```javascript
app.sai.schema.setNumber('magnetTolerance', 2f);
```

> Changes text alignment of SAI

```javascript
app.sai.schema.setString('text.alignment', 'MidLeft');
```

###SAI Schema

Many aspects of SAI can be adjusted through its schema. Here is a list of all of its schemas and a brief description:

**Booleans**

`visible`: toggles visibility

`attract.enabled`: toggles whether SAI is attracted to camera

`logging`: toggles SAI state change logging

**Numbers**

`fontSize`: sets the font size for SAI's text display

`lineSpacing`: sets the line spacing for SAI's text display 

`text.width`: sets the width for SAI's text display 

`text.height`: sets height for SAI's text display 

`text.transitionDuration`: sets the transition duration between SAI text changes

`attract.delay`: sets how long SAI waits before the user's focus

`attract.pause`: sets how long SAI waits before returning to the waypoint

`springConstant`: sets how fast SAI moves

`magnetTolerance`: sets the threshold distance that when surpassed, SAI repositions itself in front of the camera

**Strings**

`font`: sets the font for SAI's text display 

`lineSpacing`: sets the line spacing for SAI's text display 

`text.horizontalOverflow`: sets the horizontal overflow(*Overflow* or *Wrap*) for SAI's text display 

`text.verticalOverflow`: sets the vertical overflow(*Overflow* or *Wrap*) for SAI's text display 

`text.alignment`: sets the alignment for SAI's text display 

**Colors**

`text.shadow.color`: sets the shadow color for SAI's text display 

**Vectors**

`text.shadow.offset`: sets the shadow offset for SAI's text display 


> Adds button option to sai for users to choose from


##SAI Buttons(Preview)

The `prompt` command can be used to let SAI prompt button options for the player to choose from. The first argument is a callback, the second is the question, the following are chained questions.

`prompt(Callback callback, String Question, String option1, String option2, ...)`

The callback has a payload that indicates which button has been selected.

> Adds button options to sai

```javascript
app.sai.enabled = true;

function enter() {
        app.sai.prompt(onClicked, "How are you?", "Great", "Not so well", "Terrible");
}

//call back function to handle button selection
function onClicked(buttonSelected) {
        switch(buttonSelected) {
                case 1:
                        // first button selected
                        sai.write("Glad to hear that!");
                        break;
                case 2:
                        // second button selected
                        sai.write("Hope your day will brighten up!");
                        break;

                case 3:
                        // third button selected
                        sai.write("Oh no what happened?");
                        break;

        }
}

```

###SAI Buttons Schema

SAI buttons' schema is separate from that of SAI. To make alterations, we use `app.sai.buttonSchema`.
Here is a list of the button schema descriptions and a short description.

**Boolean** 

`visible`: toggles SAI buttons visibility

**Numbers** 

`fontSize`: sets the font size for the text on SAI's buttons

`lineSpacing`: sets the line spacing for the text on SAI's buttons 

`text.height`: sets the height for the text on SAI's buttons 

`text.width`: sets the width for the text on SAI's buttons 

**Strings**

`font`: sets the font for the text on SAI's buttons 

`text.horizontalOverflow`: sets the horizontal overflow (*Overflow* or *Wrap*) for the text on SAI's buttons 

`text.verticalOverflow`: sets the vertical overflow (*Overflow or *Wrap*) for the text on SAI's buttons 

**Color**

`text.shadow.color`: sets the shadow color for the text on SAI's buttons 

**Vector**

`text.shadow.offset`: sets the shadow offset for the text on SAI's buttons 


> Changes sai buttons' text size

```javascript
app.sai.buttonSchema.setNumber('fontSize', 75);
```

> Changes sai buttons' vertical overflow

```javascript
app.sai.buttonSchema.setString('Wrap');
```

> Changes sai buttons' text shadow color

```javascript
app.sai.buttonSchema.setColor('text.shadow.color', col(0,0,0,1));
```

###Notes on vertical and horizontal overflow properties

By default, the horizontal overflow is `Wrap` and the vertical overflow is `Overflow`.
Setting `text.width` will only take effect when the horizontal overflow is set to `Wrap`                                
Assuming that horizontal overflow is wrap:

- Setting vertical overflow to `Overflow`: the buttons' text height will not be affected by `text.height` and will grow according to text length and the number of lines. Their positions will also adjust and make sure to have the same padding from end-to-end.

- Setting verical overflow to `Wrap`: the buttons' text height will limited by `text.height`. The text box's size will be fixed and all buttons will be of same distance from each other.`

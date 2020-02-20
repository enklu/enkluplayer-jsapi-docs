# Score

```javascript
var score = require('score');
```
The score API allows users to define different score types, update scores, as well as define their visual characteristics.

##Creating a score

Every score is attached to a `type` name. The user can choose to specify an `Element` to make marginal scores appear around that `Element`.  The user can also initiate a score without any element so it appears in front of the user, or specify the location of the score when configuring it.

```javascript 
// Initialize score with specified element. Spawned marginal scores will appear around that element
var coinsScore = score.type(this, 'coins');

// Initialize score without element. Spawned marginal scores will appear in front of the camera
var points = score.type(this, 'points);

```
##Configuring a score

Each parameter of the score can be controlled through calling a method of the score. These functions can be chained.

All of the parameters are optional. The default values are the following if there are no specifications:

 Score = 0

 Color = Col4(1, 1, 1, 1)

 Font Size = 60

 Location = null

 Duration = 1s

 Random Range = 0

 Prefix = “”

Suffix = “”

```javascript
var score = require(‘score’);

//declares a score object
var coinsScore = score.type(this, 'coins')

//sets initial score to be 10
.initScore(10)

//sets the color of margin score display to be red
.color(new Col4(1,0,0,1))

//sets margin score display font size to be 90
.fontSize(90)

//sets score display location
.location(new Vec3(3,3,0))

//sets duration of margin score display to be 2 seconds
.duration(2)

//spawns the marginal scores at a random location within the specified range
.randomRange(1)

//adds a '+' sign as a prefix
.prefix('+')

//adds 'pts' as a suffix
.suffix('pts');

```
##Adding score

Scores can be directly added to the Score object declared. They can also be added through the score manager. 

```javascript
//add score directly to score object
var coinsScore = score.type('coins');

coinsScore.award(2);


//add score through score manager
score.award('coins', 2);

```

## Messages on increase/decrease
A global message `score-increase` is dispatched whenever a score increases. A global message `score-decrease` is dispatched whenever a score decreases.



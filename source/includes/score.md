# Score

```javascript
var score = require('score');
```
The score API allows users to define different score types, update scores, as well as define their visual characteristics.

##Creating a Score

```javascript 
// Initialize score with specified element. Spawned marginal scores will appear around that element
var coinsScore = score.type(this, 'coins');

// Initialize score without element. Spawned marginal scores will appear in front of the camera or at the specified location
var points = score.type('points);
```

Every score is attached to a `type` name. The user can choose to specify an `Element` to make marginal scores appear around that `Element`.  The user can also initiate a score without any element so it appears in front of the user, or specify the location of the score when configuring it.

##Configuring a Score.

```javascript
var score = require('score');

//declares a score object
var coinsScore = score.type(this, 'coins')

//sets initial score to be 10
.initScore(10)

//sets the color of margin score display to be red
.color(new Col4(1,0,0,1))

//sets margin score display font size to be 90
.fontSize(90)

//sets score display offset
//When there is an element attached, this is the local offset
//When there is none attached, this is the global offset
.offset(new Vec3(3,3,0))

//sets duration of margin score display to be 2 seconds
.duration(2)

//spawns the marginal scores at a random location within the specified range
.randomRange(1)

//adds a '+' sign as a prefix
.prefix('+')

//adds 'pts' as a suffix
.suffix('pts');
```

Each parameter of the score can be controlled through calling a method of the score. These functions can be chained.

All of the parameters are optional. The default values are the following if there are no specifications:

 **Score**: 0

 **Color**: Col4(1, 1, 1, 1)

 **Font Size**: 60

 **Offset**: null
	if declared with an Element - defaults to vec3(0, 0, 0)

 **Duration**: 1s

 **Random Range**: 0

 **Prefix**: ""

 **Suffix**: ""

##Adding Score 

```javascript
var coinsScore = score.type('coins');

//add score directly to score object
coinsScore.award(2);
 
//add score directly to score object at specified location
coinsScore.award(2, vec3(0, 1, 1));

//add score through score manager
score.award('coins', 3);

//add score directly to score object at specified location
score.award('coins', 3, vec3(0, 1, 1));  

//add score without spawning visualization
coinsScore.awardHidden(10);
score.awardHidden('coins', 8);

//clear score to 0
coinsScore.clearScore();
score.clearScore('coins');
```

Scores can be directly added to the Score object declared. They can also be added through the score manager. Scores can be awarded without showing any visual indication using `awardHidden`.

##Access and Modify Score Parameters

```javascript
const score = require('score');

//access to specific score and make modifications
var coinsScore = score.getType('coins');
coinsScore.FontSize = 100;
log.info(coinsScore.Data.FontSize);     //prints 100

//Current score can also be directly accessed by through the score manager
score.getScore('coins');

```
The score manager has reference to all created scores. All score information can be accessed and modified through the manager.

##Spawned Score Location

>Scores declared with attached Element

```javascript
// Score attached to element
//----------element 1---------
const score = require('score');

//when there is only one mushroom and spawns score around it
var mushroomScore = score.type(this, 'mushroom')
        .initScore(2)
        .offSet(vec3(0, -0.1, 0));

mushroomScore.award(2);     //score appears slightly below element 1's position

//----------element 2---------
const score = require('score');
score.award('mushroom' , 2);  //score appears slightly below element 1 position
```

Score will appear at different locations depending on how you declare them, what offset you set, and what method you use to award scores.

**Scores declared with attached Element** The spawned score's location will be that of the attached Element adjusted with local offset. This can be useful when the attached Element has the potential to move around. 

**Scores declared without attached Element, and without offset** The spawned score will appear directly in fron of the user's field of view. 
*Please not that if the offset is later assigned, the score will be spawned like the type below.*

>No elements attached, no offset specified

```javascript
//----------element 1---------
const score = require('score');

//declare ScoreJs with no element attached
var butterflyScore= score.type('butterfly');

butterflyScore.award(2);     //score appears in front of player's field of view

//----------element 2---------
const score = require('score');
score.award('butterfly', 2);  //score appears in front of player's field of view
```

**Scores declared without attached Element, and with offset** The spawed score will appear at the global offset location.
>No elements attached, offset specified

```javascript
//----------element 1---------
const score = require('score');

//declare ScoreJs with no element attached, but specifies global offset
var planetScore= score.type('planet')
                      .location(vec3(0, 0, 0));


planetScore.award(2);     //score appears at position (0, 0, 0) even if element 1 changes position

//----------element 2---------
const score = require('score');
score.award('planet', 2);  //score appears at position (0, 0, 0) 
```

**Award scores with a specified location** The spawned score will appear at the specified location regardless of how it is declared.

>Award scores at specified location regardless of the score's parameters. Can be combined with other methods or on its own.

```javascript
// Each element spawn score at their corresponding place, but all contribute to same score
//----------element 1---------
const score = require('score');

//declare ScoreJs with no element attached, but specifies location
var waterScore= score.type('water')
                      .offset(this.transform.position);


waterScore.award(2);     //score appears around element 1, award points to water score

//----------element 2---------
const score = require('score');
score.award('water',this.transform.position, 2);  //score appears around element 2 location, 
                                                  //award points to water score

//----------element 3---------
const score = require('score');
score.award('water',this.transform.position, 3);  //score appears around element 3 location,
                                                  //award points to water score 
```

## Messages on Score Change

```javascript
const scoreManager = require('score');

const MSG_SCORE_INCREASE = 'score-increase';

function enter() {
        scoreManager.on(MSG_SCORE_INCREASE, onIncrease);
}

function onIncrease(type, margin) {
        //do sth on coins score increase
        if(type == 'coins') {
        log.info("Coins increased by " + margin);
        }
}

function exit() {
        scoreManager.off(MSG_SCORE_INCREASE, onIncrease);
}
```
A local message `score-increase` is dispatched whenever a score increases. A local message `score-decrease` is dispatched whenever a score decreases. Each of these messages include two payloads: the first one is the type name of the increased/decreased score, and the second one is the marginal score.


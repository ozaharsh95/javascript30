# 30 Whack a Mole:

fun game to hit molesss.

# Learnings :

## Random Time generation and Hole selection :

```javascript
let lastHole;

function randomTime(min,max){

    return Math.floor(Math.random() * (max - min) + min);

}

function randomHole(holes){

    const index = Math.floor(Math.random()*holes.length);
    // if hole's index is repeated we call recursive untill we get unique index
    if(index===lastHole){
        return randomHole(holes);
    }
    lastHole = index;

    return holes[index];
}
```

## Start function :

+ Invoked when user press start button 

```javascript

function startGame(){
    scoreBoard.textContent = 0;     // seting deafault score
    timeOutFlag = false;
    popup();
    // here after 5sec game over ....
    setTimeout(()=>{
        timeOutFlag = true;
    },5000);

}

```

## popup function :

+ it toggle mole up at perticular random time and hole.

+ it recursively called utill game over

```javascript

function popup(){
    const time = randomTime(200,1000);          // geting random time
    const hole = randomHole(holes);             // getting a random hole
    hole.classList.add('up');                   // pop up mole from hole    
    setTimeout(()=>{
        hole.classList.remove('up');            // after that random time mole get back to hole
        if (!timeOutFlag) {
            // popup runs till game is over 
            popup();                            
        }
    },time);
}

```

## Hit to mole && score update:

+ we add 'click' eventListener to mole for hitiing

+ **`event.isTrusted`** 

    + According to MDN, According to MDN, the **isTrusted** property returns **true** when the event was generated by a user action, and **false** when the event was created or modified by a script.

    + user may create fake clicks to create high score by manipulating DOM , those mispractices can be prevented by `isTrusted`.

```javascript

function bonkMole(e){
    if(!e.isTrusted) return;                    
    this.parentNode.classList.remove('up');     // remove mole if we hit 
    score++;                                    // when hit happen -> score++
    scoreBoard.textContent = score;             // score updated to DOM
}

moles.forEach((mole)=>{
    mole.addEventListener('click',bonkMole);
});
```

# References :

+ [Event.isTrusted - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Event/isTrusted)

+ [Arjun khode's blog](https://thesagittariusme.blogspot.com/search/label/JS30)

+ [The great Kelly chiii](https://github.com/KellyCHI22/JavaScript30/blob/main/30-Whack-A-Mole/README.md)

---
layout: page
title: microcomputer final project
description: final project for MIT course 6.2061 (6.1151)
img: assets/img/mc_cover.jpeg
importance: 1
category: school/work
---

<h3>Background</h3>

In Microcomputer Project Laboratory, MIT Course 6.2061 (previously 6.1151), the class ends with a 4 week long independent final project. The only requirements were to use the [PSoC 5 Microcontroller](https://www.infineon.com/cms/en/product/microcontroller/32-bit-psoc-arm-cortex-microcontroller/32-bit-psoc-5-lp-arm-cortex-m3/). Additional hardware components were recommended and available to be used as well. An initial proposal was written and reviewed with course staff, but the project plan was designed and executed independently. 

<hr/>
<h3>Overview</h3>

For my project, a **Connect Four** game was created. The game was programmed in C on the PSoC 5, with game visuals showing up on a TFT screen. The player(s) used a keypad to control the game (from player setup to actual gameplay) with additional light and sound effects. After a quick demo, the hardware components will be summarized, followed by a description of the features in the game. 
The purpose of this page is to summarize the results of the project. To get more details on any section, including implementation process, download the **full report in the Links section** at the bottom. **Source code can also be found there.**

<hr/>
<h3>Demo</h3>

Shown is single player mode, with basic adding and assigning of players. 
For a complete feature description see Game Features below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/XgA4j9Mq-rQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<hr/>
<h3>Hardware Components</h3>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/screen.jpeg" title="tft screen" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <b>TFT Screen</b>
            <p>The TFT screen is the primary visuals for the game, showing things such as the list of players or the state of the game.</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/keypad.jpeg" title="keypad" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <b>Keypad + 74C922</b>
            <p>The keypad, along with its encoder, is the primary method for users to interact with the game.</p>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/potentiometer.jpeg" title="potentiometer + buttons" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <b>Potentiometer + Buttons</b>
            <p>These components are part of the PSoC, and are only used to enter a player’s name during game setup.</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/speaker.jpeg" title="speaker" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <b>Speaker + LM386</b>
            <p>The speaker and amplifier are used as a form of negative feedback, playing an error-like buzzer sound when a user does something wrong.</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/leds.jpeg" title="leds" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <b>LEDs</b>
            <p>The lights are used as additional visuals for the user, lighting up to indicate which player's turn it is.</p>
        </div>
    </div>
</div>

<hr/>
<h3>Game Features</h3>

The game is controlled almost exclusively through the keypad - when referring to a player "choosing" or "selecting" something, it is likely using the numbers on the keypad.

**Note**: In the context of the game, the term “player” can easily be interpreted as a number of things. As a result, this report aims to refer to the person playing the game as the **user**, the names in the system as **players**, and the 1 or 2 players actually playing the game as **P1** and **P2**. 

<p>&nbsp;</p>
<h4>Home Screen</h4>
<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        
        <p>In addition to being a home screen, this is also a “pre-setup” screen where <b>users choose between single and two player mode</b> - pressing 1 on the keypad chooses single player, and pressing 2 is two player.</p>
        <p>Even after entering the setup state, the user can press * on the keypad to return to the home screen and switch a different player mode.</p>
        <p>The screen will also be shown when starting a new game after completed games, as selecting the player mode is a necessary step to determining player assignment and flow of the game.</p>
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.html path="assets/img/home.jpeg" title="home screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<p>&nbsp;</p>
<h4>Setup Screen</h4>
<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        
        <p>Until the user “moves on” from the setup state, we will continue to display the following information:</p>
        <ol>
            <li><b>All players</b> currently in the system (green list)</li>
            <li><b>Players currently assigned</b> to P1 and P2 (yellow highlight)</li>
            <li>Options available (purple boxes)</li>
        </ol>
        <p>The option to assign P2 is only available in two player mode.</p>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/singleplayer.jpeg" title="home screen" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/twoplayer.jpeg" title="home screen" class="img-fluid rounded z-depth-1" %}
    </div>
    
</div>

A user will choose one of the options. If it is something they are able to do, then the program will act accordingly. If not, a buzzer sound will be emitted from the speaker to indicate they have done something incorrect. The screen updates itself depending on changes made by the user.
When starting up the game for the first time, there will be no players and empty assignments (left screen). The only successful action that can be done is adding a new player. Only players in the player list can be assigned - new players always need to be added before being assigned.

<div class="caption">
    <p>A user will choose one of the options. If it is something they are able to do, then the program will act accordingly. If not, a buzzer sound will be emitted from the speaker to indicate they have done something incorrect. The screen updates itself depending on changes made by the user.</p>
    <p>When starting up the game for the first time, there will be no players and empty assignments (left screen). The only successful action that can be done is adding a new player. Only players in the player list can be assigned - new players always need to be added before being assigned.</p>
</div>


The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}
```html
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
```
{% endraw %}

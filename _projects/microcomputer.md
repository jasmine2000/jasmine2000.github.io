---
layout: page
title: microcomputer final project
description: April 2022 - May 2022
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

<h5>Setup - Adding Players</h5>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/keyboard.jpeg" title="keyboard" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>"Keyboard" Screen</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/potentiometer.jpeg" title="potentiometer" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Potentiometer to move across a row <i>(purple)</i></p>
            <p>Button to toggle between rows <i>(yellow)</i></p>
            <p>Button to “press” a key <i>(blue)</i></p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/keyboardname.jpeg" title="keyboard name" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Once full name is entered, user should click "enter" key (bottom right)</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/newplayer.jpeg" title="new player" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>New player will now show up on player list</p>
        </div>
    </div>
</div>

<h5>Setup - Deleting Players</h5>
This command was primarily created because only 9 players can be in the system at a time, so if 9 exist and a new person wants to play, an old one needs to be deleted. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/delete1.jpeg" title="delete1" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Original player list - user presses B to delete a player</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/delete2.jpeg" title="delete2" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>User then selects an actual player to delete (players are numbered)</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/delete3.jpeg" title="delete3" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>The deleted player is "confirmed" because this act cannot be undone</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/delete4.jpeg" title="delete4" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>The modified player list</p>
        </div>
    </div>
</div>

<h5>Setup - Assigning Players</h5>
To choose which players will actually be playing the game, P1 (and P2 in two player) need to be assigned.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assign1.jpeg" title="assign1" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Original player list - user presses 1 or 2 to assign P1 and P2, respectively</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assign2.jpeg" title="assign2" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>User then selects an actual player to be assigned</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assign3.jpeg" title="assign3" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Player screen now reflects assignment</p>
        </div>
    </div>
</div>

<h5>Setup - Confirming Players</h5>
If user(s) are satisfied with player assignment(s), they confirm players, and gameplay begins.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/confirm1.jpeg" title="confirm1" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Original player list - user presses C to confirm players and begin gameplay</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/confirm2.jpeg" title="confirm2" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>User presses C again to confirm assigned players</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/confirm3.jpeg" title="confirm3" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>Gameplay begins</p>
        </div>
    </div>
</div>

If user(s) are satisfied with player assignment(s), they confirm players, and gameplay begins.
<div class="row">
    <div class="col-sm-4 mt-3 mt-md-0">
        <h5>Setup - Help Screen</h5>
        <p>If the user was unclear with any part of the setup process, they can press 0 to display the help screen. The contents of the help screen are a quick summary of the explanation above.</p>
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/setuphelp.jpeg" title="setuphelp" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<p>&nbsp;</p>
<h4>Gameplay</h4>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <p>Players take turn placing chips in columns.</p>
        <p>The game will indicate whose turn it is using both LED lights, along with text at the top of the screen.</p>
        <p>A chip (corresponding to the player’s color) will hover above the board in the column that they have currently selected.</p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/play1.jpeg" title="play1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/play2.jpeg" title="play2" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>The "hovering chip" defaults to the middle column.</p>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/play3.jpeg" title="play3" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>The player is able to change their mind any number of times, and the chip will move to the column they are selecting.</p>
        </div>
    </div>
</div>
<div class="caption">
    <p>Pressing D will drop the piece and switch to the other player's turn.</p>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <h5>Game Ending - Early Exit</h5>
        <p>Players have the opportunity to exit the game early by pressing *.</p>
        <p>If they do so, the game will confirm that they want to exit (yellow highlighted text at the bottom of the screen), and send the player to the leaderboard screen (with no scores changed).</p>
        <p>The leaderboard screen will be shown in the following section.</p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/play3.jpeg" title="play3" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <p>The player is able to change their mind any number of times, and the chip will move to the column they are selecting.</p>
        </div>
    </div>
</div>

<h5>Game Ending - Player Wins</h5>
After a player makes a move that results in a win, a message will display. Then, the screen will change to show the leaderboard, with the most recent win reflected in the scores next to the player names.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/win1.jpeg" title="win1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/win2.jpeg" title="win2" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/win3.jpeg" title="win3" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<p>&nbsp;</p>
<h2>Independent Inquiry</h2>

There was an extension of this class - the Independent Inquiry. For the extension, **a topic (outside the scope of the class) was studied independently throughout the semester, and incorporated into the final project.**

The goal of my independent inquiry study was to get **hands-on experience with machine learning**. Selected chapters from Hands-On Machine Learning by Aurélion Géron were read. This included all of Part I (Fundamentals of Machine Learning) and half of Part II (Neural Networks and Deep Learning). 

The goal was to extend the main project by **allowing users to use their voice to control the column selection in the Connect Four game**. Initially, this appeared to be a problem mostly concerned with training and improving a ML model, although the scope expanded to other areas as well, including signal processing, understanding ModusToolbox compatibility, and more. A PSoC 6 was used for this portion of the project, as the increased memory and computing power was necessary to support a ML model.

The system appears functional (records audio, transforms it, outputs a prediction), but the accuracy is pretty low, and was therefore not integrated into the main project. The high level process of arriving at the current state is summarized below.

For a more in depth discussion, **reference the Independent Inquiry section in the full report**.

<h3>Summary of Independent Inquiry Process</h3>
<ol>
    <li>
        Creating a basic ML model
        <ul>
            <li>Collecting and loading data</li>
            <li>Eliminating outliers</li>
            <li>Training and testing model</li>
        </ul>
    </li>
    <li>
        Iterating on the model
        <ul>
            <li>Trying various preprocessing techniques</li>
        </ul>
    </li>
    <li>
        Modifying model to be compatible with software/PSoC
        <ul>
            <li>Drastically reducing model size</li>
            <li>Replacing/removing layers (newest version of TF was not supported)</li>
        </ul>
    </li>
    <li>
        Writing audio preprocessing code in C (prior experimentation was done in Python)
        <ul>
            <li>Trimming silence on real time recorded audio</li>
            <li>Short Time Fourier Transform to prepare for ML model</li>
        </ul>
    </li>
</ol>

<p>&nbsp;</p>
<h2>Links</h2>
<a href="{{ '6115_final_report.pdf' | prepend: 'assets/pdf/' | relative_url}}" target="_blank" rel="noopener noreferrer">Final Report PDF</a>

[Final Project Source Code](https://github.com/jasmine2000/connect-four)

[Independent Inquiry Source Code](https://github.com/jasmine2000/spoken-digits)
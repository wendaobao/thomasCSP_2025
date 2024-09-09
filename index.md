---
layout: base
title: Student Home 
description: Home Page
Author: Thomas Bao
Image: /images/mario_animation.png
hide: true
---




<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %}

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 7.5);
    }

    startWalkingLeft() {
        this.stopAnimate();
        this.animate(this.obj["WalkL"], -7.5);
    }

    startRunningLeft() {
        this.stopAnimate();
        this.animate(this.obj["Run1L"], -15);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 15);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startPuffingLeft() {
      this.stopAnimate();
      this.animate(this.obj["PuffL"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    startRestingLeft() {
      this.stopAnimate();
      this.animate(this.obj["RestL"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
        if (mario.currentSpeed === 0) {
            mario.startWalking();
        } 
        else if (mario.currentSpeed === 7.5) {
            mario.startRunning();
        }
        else if (mario.currentSpeed < 0){
            mario.startResting();
        }
    } 

    else if (event.key === "ArrowDown") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } 
      else if (mario.currentSpeed >= 0){
        mario.startPuffing();
      }
      else {
        mario.startPuffingLeft();
      }
    } 

    else if (event.key === "ArrowLeft") {
        event.preventDefault();
        if (mario.currentSpeed === 0) {
            mario.startWalkingLeft();
        } 
        else if (mario.currentSpeed === -7.5) {
            mario.startRunningLeft();
        }
        else if (mario.currentSpeed > 0){
            mario.startRestingLeft();
        }
    }

    else if (event.key === "ArrowUp") {
      event.preventDefault();
      if (mario.currentSpeed === 0) {
        mario.startFlipping();
      }
      else if (mario.currentSpeed < 0) {
        mario.startRestingLeft();
      }
      else if (mario.currentSpeed > 0) {
        mario.startResting();
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 7.5) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${.5 * scale})`;
    mario.startResting();
  });

</script>


# My journey starts here.

When setting up the tools, I had trouble navigating through the terminal to install packages such as homebrew and cloning the nighthawk portfolio. To resolve this problem I had to reinstall the nighthawk portfolio and forking it in github first than downloading it to my terminal. To install homebrew succesfully, I asked a friend and chatGPT to help me out and I discovered that I was doing it for the Windows method when I had a Mac. I was able to overcome all these problems and being able to actually work on this project. 



<style>
  .fancy-button {
    font-size: 20px;
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #FF5722; /* Bright orange */
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s ease-in-out; /* Smooth transition for hover/click effects */
  }

  .fancy-button:hover {
    background-color: #FFC107; /* Bright yellow on hover */
  }

  .fancy-button:active {
    background-color: #FFEB3B; /* Even lighter yellow when clicked */
    transform: scale(0.95); /* Shrink effect on click */
  }
</style>

<a href="">
  <button class="fancy-button">Link to nothing</button>
</a>

<a href="https://nighthawkcoders.github.io/teacher_portfolio/">
  <button class="fancy-button">Link to CSP Page</button>
</a>

<a href="https://delnorte.powayusd.com/apps/bell_schedules/">
  <button class="fancy-button">Link to Del Norte Bell Schedule Page</button>
</a>

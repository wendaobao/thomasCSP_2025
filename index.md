---
layout: base
title: Student Home 
description: Home Page
Author: Thomas Bao
Image: /images/mario_animation.png
hide: true
---


<div class="button-container">
  <a href="https://dakshag001.github.io/dakshaggCSP_2025/" target="_blank">
    <button class="button33">
      <svg xmlns="https://dakshag001.github.io/dakshaggCSP_2025/" width="1em" height="1em" viewBox="0 0 1024 1024" stroke-width="0" fill="currentColor" stroke="currentColor" class="icon">
        <path d="M946.5 505L560.1 118.8l-25.9-25.9a31.5 31.5 0 0 0-44.4 0L77.5 505a63.9 63.9 0 0 0-18.8 46c.4 35.2 29.7 63.3 64.9 63.3h42.5V940h691.8V614.3h43.4c17.1 0 33.2-6.7 45.3-18.8a63.6 63.6 0 0 0 18.7-45.3c0-17-6.7-33.1-18.8-45.2zM568 868H456V664h112v204zm217.9-325.7V868H632V640c0-22.1-17.9-40-40-40H432c-22.1 0-40 17.9-40 40v228H238.1V542.3h-96l370-369.7 23.1 23.1L882 542.3h-96.1z"></path>
      </svg>
    </button>
  </a>
  
  <a href="https://dakshag001.github.io/dakshaggCSP_2025/search/" target="_blank">
    <button class="button33">
      <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" aria-hidden="true" viewBox="0 0 24 24" stroke-width="2" fill="none" stroke="currentColor" class="icon">
        <path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" stroke-linejoin="round" stroke-linecap="round"></path>
      </svg>
    </button>
  </a>
  
  <a href="https://dakshag001.github.io/dakshaggCSP_2025/about/" target="_blank">
    <button class="button33">
      <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" stroke-width="0" fill="currentColor" stroke="currentColor" class="icon">
        <path d="M12 2.5a5.5 5.5 0 0 1 3.096 10.047 9.005 9.005 0 0 1 5.9 8.181.75.75 0 1 1-1.499.044 7.5 7.5 0 0 0-14.993 0 .75.75 0 0 1-1.5-.045 9.005 9.005 0 0 1 5.9-8.18A5.5 5.5 0 0 1 12 2.5ZM8 8a4 4 0 1 0 8 0 4 4 0 0 0-8 0Z"></path>
      </svg>
    </button>
  </a>
  
  <a href="https://dakshag001.github.io/dakshaggCSP_2025/blogs/" target="_blank">
    <button class="button33">
     <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="currentColor" class="icon">
    <path d="M3 2C2.44772 2 2 2.44772 2 3V21C2 21.5523 2.44772 22 3 22H21C21.5523 22 22 21.5523 22 21V3C22 2.44772 21.5523 2 21 2H3ZM12 19L5 15H7V10H17V15H19L12 19ZM12 4C16.4183 4 20 7.58172 20 12C20 16.4183 16.4183 20 12 20C7.58172 20 4 16.4183 4 12C4 7.58172 7.58172 4 12 4Z"></path>
    </svg>
  </button>
  </a>
</div>



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

<img src="https://delnorte.powayusd.com/pics/school_logo.png" alt="School Logo" width="200">


<style>
    .paste-button {
        position: relative;
        display: block;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    .button {
        background-color: #88BC4C;
        color: #212121;
        padding: 10px 15px;
        font-size: 15px;
        font-weight: bold;
        border: 2px solid transparent;
        border-radius: 15px;
        cursor: pointer;
    }
    .dropdown-content, .submenu-content {
        display: none;
        font-size: 13px;
        position: absolute;
        z-index: 1;
        min-width: 200px;
        background-color: #212121;
        border: 2px solid #88BC4C;
        border-radius: 0px 15px 15px 15px;
        box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    }
    .dropdown-content a, .submenu-content a {
        color: #88BC4C;
        padding: 8px 10px;
        text-decoration: none;
        display: block;
        transition: 0.1s;
    }
    .dropdown-content a:hover, .submenu-content a:hover {
        background-color: #88BC4C;
        color: #212121;
    }
    .dropdown-content a:focus, .submenu-content a:focus {
        background-color: #212121;
        color: #88BC4C;
    }
    .dropdown-content #top:hover {
        border-radius: 0px 13px 0px 0px;
    }
    .dropdown-content #bottom:hover {
        border-radius: 0px 0px 13px 13px;
    }
    .paste-button:hover button {
        border-radius: 15px 15px 0px 0px;
    }
    .paste-button:hover .dropdown-content {
        display: block;
    }
    /* Submenu styles */
    .submenu {
        position: relative;
    }
    .submenu-content {
        top: 0;
        left: 100%;
        border-radius: 0 15px 15px 15px;
    }
    .submenu-content a:first-child:hover {
        border-radius: 0px 13px 0px 0px;
    }
    .submenu-content a:last-child:hover {
        border-radius: 0px 0px 13px 13px;
    }
    .submenu:hover .submenu-content {
        display: block;
    }
</style>
<div class="paste-button">
  <button class="button">Menu &nbsp; ▼</button>
  <div class="dropdown-content">
    <a id="top" href="https://wendaobao.github.io/thomasCSP_2025/2024/09/09/hackstool_IPYNB_2_.html">Jupyter Notebook</a>
    <div class="submenu">
        <a id="middle" href="http://127.0.0.1:4100/thomasCSP_2025/2024/08/21/sprint1_plan_IPYNB_2_.html">Sprint 1 Main Objectives &nbsp;</a>
        <div class="submenu-content">
            <a href="https://wendaobao.github.io/thomasCSP_2025/snake/">Snake game</a>
            <a href="https://wendaobao.github.io/thomasCSP_2025/calculator/">Calculator</a>
            <a href="https://wendaobao.github.io/thomasCSP_2025/cookieclicker/">Cookie Clicker Game</a>
            <a href="https://wendaobao.github.io/thomasCSP_2025/randomnumber/">Random Number Generator</a>
        </div>
    </div>
    <a id="bottom" href="https://wendaobao.github.io/thomasCSP_2025/about/">About Pages</a>
  </div>
</div>


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


<a href="https://nighthawkcoders.github.io/teacher_portfolio/">
  <button class="fancy-button">Link to CSP Page</button>
</a>

<a href="https://delnorte.powayusd.com/apps/bell_schedules/">
  <button class="fancy-button">Link to Del Norte Bell Schedule Page</button>
</a>

<a href="">
  <button class="fancy-button">Link to Sprint 2 teachings</button>
</a>




<style>
.button-container {
  display: flex;
  background-color: rgba(245, 73, 144);
  width: 250px;
  height: 40px;
  align-items: center;
  justify-content: space-around;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px,
        rgba(245, 73, 144, 0.5) 5px 10px 15px;
}

.button33 {
  outline: 0 !important;
  border: 0 !important;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: transparent;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  transition: all ease-in-out 0.3s;
  cursor: pointer;
}

.button:hover {
  transform: translateY(-3px);
}
.icon {
  font-size: 20px;
}
</style>
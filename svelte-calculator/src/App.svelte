<script>
	import Big from 'big.js';

	let currentInput = "";
	let memory = 0;
	let operator = "";
  
	function handleDigitClick(digit) {
    if (currentInput === memory.toString()) {
      // If the current input is the result, clear everything for a new calculation
      currentInput = "";
      memory = 0;
      operator = "";
    }
    currentInput += digit;
  }
  
	function handleDecimalClick() {
	  if (currentInput.indexOf(".") === -1) {
		// Check if a decimal point already exists in the input
		currentInput += ".";
	  }
	}
  
	function handleOperatorClick(op) {
	  if (currentInput !== "") {
		if (operator && memory !== 0) {
		  handleEqualsClick();
		} else {
		  memory = parseFloat(currentInput);
		}
		operator = op;
		currentInput = "";
	  }
	}
  
	function handleEqualsClick() {
    if (currentInput !== "") {
      switch (operator) {
        case '+':
          memory = Big(memory).plus(Big(currentInput)).toString();
          break;
        case '-':
          memory = Big(memory).minus(Big(currentInput)).toString();
          break;
        case '*':
          memory = Big(memory).times(Big(currentInput)).toString();
          break;
        case '/':
          memory = Big(memory).div(Big(currentInput)).toString();
          break;
        default:
          break;
      }
      // Remove trailing zeros
      currentInput = parseFloat(memory).toString();
      operator = "";
    }
  }
  
	function handleDelete() {
	  currentInput = currentInput.slice(0, -1);
	}
  
	function handleReset() {
	  currentInput = "";
	  memory = 0;
	  operator = "";
	}


///theme switcher

import { onMount } from 'svelte';

  // Possible themes
  const themes = [
    { background: 'green' },
    { background: 'grey' },
    { background: 'white' },
  ];

  // Initial theme state, default to position 0 (green background)
  let currentTheme = 0;

  // Load the user's choice from sessionStorage if available
  if (sessionStorage.getItem('theme')) {
    currentTheme = parseInt(sessionStorage.getItem('theme'), 10);
  }

  // Update the theme when the currentTheme changes
  $: {
    document.body.style.backgroundColor = themes[currentTheme].background;
    sessionStorage.setItem('theme', currentTheme);
  }

  // Function to change the theme
  function changeTheme() {
    currentTheme = (currentTheme + 1) % themes.length;
  }
  // Function to save the currentTheme to session storage
  function saveThemeToStorage() {
    sessionStorage.setItem('theme', currentTheme);
  }

  // Check if there is a saved theme in session storage and load it
  if (sessionStorage.getItem('theme') !== null) {
    currentTheme = parseInt(sessionStorage.getItem('theme'));
  }

  // Save the currentTheme to session storage whenever it changes
  $: saveThemeToStorage();

  </script>
  
  <main>
	<div>
	  <div class="toggle3-switch">
		<input type="radio" id="toggle3radio1" class=radio1 name="toggle3"/>
		<input type="radio" id="toggle3radio2" class=radio2 name="toggle3" />
		<input type="radio" id="toggle3radio3" class=radio3 name="toggle3" checked />
		<div class=topLabelContainer>
		  <label class=labelTop1 for="toggle3radio1">1
			</label><label class=labelTop2 for="toggle3radio2">2
			</label><label class=labelTop3 for="toggle3radio3">3</label>
		</div>
		<span class=labelLeft>THEME
		  </span><span class=sliderContainer>
		  <label class="label1" for="toggle3radio1"></label>
		  <label class="label2" for="toggle3radio2"></label>
		  <label class="label3" for="toggle3radio3"></label>
		  <span class="slider"></span>
		</span>



	<div class="calculator">
	  <div class="display">{currentInput}</div>
	  <div class="row">
		<button on:click={() => handleDigitClick('7')}>7</button>
		<button on:click={() => handleDigitClick('8')}>8</button>
		<button on:click={() => handleDigitClick('9')}>9</button>
		<button on:click={handleDelete}>del</button>
	  </div>
	  <div class="row">
		<button on:click={() => handleDigitClick('4')}>4</button>
		<button on:click={() => handleDigitClick('5')}>5</button>
		<button on:click={() => handleDigitClick('6')}>6</button>
		<button on:click={() => handleOperatorClick('+')}>+</button>
	  </div>
	  <div class="row">
		<button on:click={() => handleDigitClick('1')}>1</button>
		<button on:click={() => handleDigitClick('2')}>2</button>
		<button on:click={() => handleDigitClick('3')}>3</button>
		<button on:click={() => handleOperatorClick('-')}>-</button>
	  </div>
	  <div class="row">
		<button on:click={handleDecimalClick}>.</button>
		<button on:click={() => handleDigitClick('0')}>0</button>
		<button on:click={() => handleOperatorClick('/')}>/</button>
		<button on:click={() => handleOperatorClick('*')}>x</button>
	  </div>
	  <div class="row">
		<button on:click={handleReset}>reset</button>
		<button on:click={handleEqualsClick}>=</button>
	  </div>
	</div>
	<div class="attribution">
		Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
		Coded by <a href="https://github.com/JohnyTheTurtle">JohnyTheTurtle</a>.
	  </div>
  </main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=League+Spartan:wght@500;700&display=swap');
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
  /* ... other styles ... */

  /* Styling for the theme slider */
  body{
  font-family: 'Raleway', sans-serif;
}

.toggle3-switch {
  margin:auto;
  position:relative;
  text-align:center;
  font-size:2em;
}
.radio1,
.radio2,
.radio3 {
  position: absolute;
  /*top: -9999px; - don't use may cause focus/scolling issues*/
  left: -9999px
}
.topLabelContainer{
  margin:auto;
  padding-left:6.5em; /*must match .labelLeft width + padding*/
}
.labelTop1,.labelTop2,.labelTop3{
  width:2em;
  margin:auto;
  padding:0 1px 0 1px;
  display:inline-block;
  text-align:center;
}
.labelLeft{
  width:6em;  /*must match .topLabelContainer margin-left*/
  height:3em;
  padding-right:.5em;
  position:relative;
  display:inline-block; 
  vertical-align:middle;
  text-align:right;
}
.sliderContainer{
  height: 2em;
  width: 6em;
  margin: 2px auto;
  position: relative;
  padding: 2px;
  display: inline-block;
}


.label1,
.label2,
.label3 {
  height: 100%;
  width: 100%;
  padding: 2px;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgb(200, 200, 200)
}
.radio1:checked ~ span .label1,
.radio1:checked ~ span .label3,
.radio2:checked ~ span .label1,
.radio2:checked ~ span .label2,
.radio3:checked ~ span .label2,
.radio3:checked ~ span .label3 {
  /*hide labels */
  position: absolute;
  /*top: -9999px;*/
  left: -9999px
}
.slider {
  width: 2em;
  height: 2em;
  margin: 4px;
  position: absolute;
  display: inline-block;
  pointer-events: none; /*ie fallback may be required */
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color:rgb(100,100,100);
  -webkit-transition: .4s;
  transition: .4s;
}

.radio2:checked ~ span .slider {
  -webkit-transform: translateX(2em);
  -ms-transform: translateX(2em);
  transform: translateX(2em);
}

.radio3:checked ~ span .slider {
  -webkit-transform: translateX(4em);
  -ms-transform: translateX(4em);
  transform: translateX(4em);
}

.content1{
  height:4em;
  width:100%;
  position:relative;
  display:block;
  margin:0 auto;
  padding:1em;
  text-align:center;
  font-size:1.3em;
  background:beige;
}
.content1 p{
  margin:0;
}
.labelTop1,
.radio1:checked ~ .sliderContainer .slider,
.radio1:checked ~ .content1{
  color:rgb(255,0,0);
}
.radio1:checked ~ .topLabelContainer .labelTop1{
  color:white;
  text-shadow: 0 0 1px rgb(255,100,100), 0 0 8px rgb(255,0,0);
}
.radio1:checked ~ .sliderContainer .slider{
  background:rgb(255,0,0);
}
.labelTop2,
.radio2:checked ~ .content1{
  color:rgb(255,0,255);
}
.radio2:checked ~ .topLabelContainer .labelTop2{
  color:white;
  text-shadow: 0 0 1px rgb(255,50,255), 0 0 6px rgb(255,0,255);
}
.radio2:checked ~ .sliderContainer .slider{
  background:rgb(255,0,255);
}
.labelTop3,
.radio3:checked ~ .content1{
  color:rgb(0,0,255);
}
.radio3:checked ~ .topLabelContainer .labelTop3{
  color:white;
  text-shadow: 0 0 1px rgb(100,100,255), 0 0 8px rgb(0,0,255);
}
.radio3:checked ~ .sliderContainer .slider{
  background:rgb(0,0,255);
}

.attribution { font-size: 11px; text-align: center; }
.attribution a { color: hsl(228, 45%, 44%); }

:root{
	/*## Typography*/
	/*### Body Copy*/
	--Font-size-numbers: 32px;

	/*### Theme 1*/
	/*#### Backgrounds*/
	--Very-dark-desaturated-blue-main-background: hsl(222, 26%, 31%);
	--Very-dark-desaturated-blue-toggle-background-keypad-background: hsl(223, 31%, 20%);
	--Very-dark-desaturated-blue-screen-background: hsl(224, 36%, 15%);
	/* Theme 1 */
	--key-background-theme1: hsl(225, 21%, 49%);
	--key-shadow-theme1: hsl(224, 28%, 35%);

	--red-background-theme1: hsl(6, 63%, 50%);
	--red-shadow-theme1: hsl(6, 70%, 34%);

	--orange-background-theme1: hsl(30, 25%, 89%);
	--orange-shadow-theme1: hsl(28, 16%, 65%);

	--text-dark-theme1: hsl(221, 14%, 31%);
	--text-white-theme1: hsl(0, 0%, 100%);

	/* Theme 2 */
	--main-background-theme2: hsl(0, 0%, 90%);
	--toggle-background-theme2: hsl(0, 5%, 81%);
	--screen-background-theme2: hsl(0, 0%, 93%);

	--cyan-background-theme2: hsl(185, 42%, 37%);
	--cyan-shadow-theme2: hsl(185, 58%, 25%);

	--orange-background-theme2: hsl(25, 98%, 40%);
	--orange-shadow-theme2: hsl(25, 99%, 27%);

	--yellow-background-theme2: hsl(45, 7%, 89%);
	--orange-background-theme2: hsl(35, 11%, 61%);

	--text-dark-theme2: hsl(60, 10%, 19%);
	--text-white-theme2: hsl(0, 0%, 100%);

	/* Theme 3 */
	--main-background-theme3: hsl(268, 75%, 9%);
	--toggle-background-theme3: hsl(268, 71%, 12%);
	--screen-background-theme3: hsl(268, 71%, 12%);

	--violet-background-theme3: hsl(281, 89%, 26%);
	--magenta-shadow-theme3: hsl(285, 91%, 52%);

	--cyan-background-theme3: hsl(176, 100%, 44%);
	--cyan-shadow-theme3: hsl(177, 92%, 70%);

	--violet-background-theme3: hsl(268, 47%, 21%);
	--magenta-shadow-theme3: hsl(290, 70%, 36%);

	--text-yellow-theme3: hsl(52, 100%, 62%);
	--text-dark-blue-theme3: hsl(198, 20%, 13%);
	--text-white-theme3: hsl(0, 0%, 100%);
}
</style>
# Javascript-pong
<html>
<canvas id = "gameCanvas" width = "800" height = "600">
</canvas>
<script>
//Things that will need to stay frame to frame
var canvas;
var canvasContext;
var ballX = 50;
var ballY = 50;
var ballSpeedX = 15;
var ballSpeedY = 4;

// Player Score
var player1Score = 0;
var player2Score = 0;
const WINNING_SCORE = 2;

//Winning Screen
var showingWinScreen = false;

// Player Paddles
var paddle1Y = 250;
var paddle2Y = 250;
const PADDLE_HEIGHT = 100;
const PADDLE_THICKNESS = 10;
// How the paddle finds the mouse.
function calculateMousePos(evt) {
	var rect = canvas.getBoundingClientRect();
	var root = document.documentElement;
	var mouseX = evt.clientX - rect.left - root.scrollLeft;
	var mouseY = evt.clientY - rect.top - root.scrollTop;
	return {
		x:mouseX,
		y:mouseY
	};
}
// Evt for Clearing Score
function handleMouseClick(evt) {
	if(showingWinScreen) {
		player1Score = 0;
		player2Score = 0;
		showingWinScreen = false;
	}
}
//Setting up our area
window.onload = function() {
		
		canvas = document.getElementById('gameCanvas');
		canvasContext = canvas.getContext('2d');
		
//Setting the Speed of display
		var framesPerSecond = 30;
		setInterval(function (){
			moveEverything();
			drawEverything();
		}, 1000/framesPerSecond );	
		
// End Event
		canvas.addEventListener('mousedown', handleMouseClick);
		
// The evt
		canvas.addEventListener('mousemove', function(evt) {
			var mousePos = calculateMousePos(evt);
			paddle1Y = mousePos.y - (PADDLE_HEIGHT/2);
		});
}

// If the ball moves off the screen Reset
function ballReset(){
		if (player1Score >= WINNING_SCORE ||
			player2Score >= WINNING_SCORE) {
			player1Score = 0;
			player2Score = 0;
			showingWinScreen = true;
		}
		ballSpeedX = -ballSpeedX;
		ballX = canvas.width/2;
		bally = canvas.height/2;
}
// Movement Right
function computerMovement() {
		var paddle2YCenter = paddle2Y + (PADDLE_HEIGHT/2);
		if(paddle2YCenter < ballY-35) {
				paddle2Y += 3;
			} else if(paddle2YCenter > ballY-35) {
				paddle2Y -= 3; 
			}
}
// Movement Left
function moveEverything (){
		if(showingWinScreen) {
			return;
		}
		computerMovement();
		ballX += ballSpeedX;
		ballY += ballSpeedY;
		// Getting the ball to hit the panel
		if (ballX < 0) {
			if (ballY > paddle1Y &&
				ballY < paddle1Y + PADDLE_HEIGHT) {
					ballSpeedX = -ballSpeedX;
				
					var deltaY =ballY
						-(paddle1Y+PADDLE_HEIGHT/2);
					ballSpeedY = deltaY * .35;
			} else {
				player2Score++; // Must come before reset
				ballReset();
			}
		}
		if (ballX > canvas.width) {
			if (ballY > paddle2Y &&
				ballY < paddle2Y + PADDLE_HEIGHT) {
					ballSpeedX = -ballSpeedX;
					
					var deltaY =ballY
						-(paddle2Y+PADDLE_HEIGHT/2);
					ballSpeedY = deltaY * .35;
			} else {
				player1Score++; // Must come before reset
				ballReset();
			}
		}
		if(ballY < 0) {
			ballSpeedY = -ballSpeedY;
		}
		if(ballY > canvas.height) {
			ballSpeedY = -ballSpeedY;
		}
}
//Net 
function drawNet() {
	for(var i=0; i<canvas.height; i += 40) {
		colorRect (canvas.width/2-1,i,2,20,'white');
		}
}
//Ball & Panel 
function drawEverything(){
		// Draws the page black
		colorRect(0,0, canvas.width, canvas.height, 'black');
		// Win Screen
		if(showingWinScreen) {
			canvasContext.fillStyle = 'white';
			if (player1Score >= WINNING_SCORE) {
				canvasContext.fillText("Left Player Won!", 350, 200);
			} else if(player2Score >= WINNING_SCORE) {
				canvasContext.fillText("Right Player Won!", 350, 200);
			}
				canvasContext.fillText("Click to continue", 350, 400);
				return;
		}
		// Draw Net
		drawNet();
		// Draws the left
		colorRect(0, paddle1Y, PADDLE_THICKNESS, PADDLE_HEIGHT, 'white');
		// Draws the right
		colorRect(canvas.width-PADDLE_THICKNESS, paddle2Y, PADDLE_THICKNESS, PADDLE_HEIGHT, 'white');
		// Draws the Ball
		colorCircle(ballX, ballY, 10, 'white')
		// The score
		canvasContext.fillText(player1Score, 100, 100);
		canvasContext.fillText(player2Score, canvas.width-100, 100);
}
// Draws the ball
function colorCircle(centerX, centerY, radius, drawColor){
		canvasContext.fillStyle = drawColor;
		canvasContext.beginPath();
		canvasContext.arc(centerX, centerY, radius, 0, Math.PI*2, true);
		canvasContext.fill();
}
//Looks
function colorRect(leftX,topY, width, height, drawColor) {
		canvasContext.fillStyle = drawColor;
		canvasContext.fillRect(leftX, topY, width, height);
}

</script

</html>

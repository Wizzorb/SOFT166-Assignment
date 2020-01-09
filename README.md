Password Strength Checker

Welcome to my Password Strength Checker project. This is for my assignment with SOFT166, a module on my Computer Science course.

This is the repo containing the files for the project. Unfortunately, every time I attempted to upload the file, even when zipped, it was
too big to fit in the repo, so I've been saving it to my cloud drive or just locally. I tried to upload the files
in parts, but since some of the files are hidden it wasn't possible in the end. However I have been saving regularly on my other
project repo, so hopefully that could be evidence that I have been using GitHub and not just ignoring it.

At the moment, my project has been done on Visual Studio 2017. I used to use that for past JavaScript projects so it is what I'm most
familiar with. It can be run by opening the .sln file into Visual Studio.

EDIT: Ended up figuring out how to upload it, I had to delete the Packages file and then zip it to reduce the size enough.

# Code

```<!DOCTYPE html>
	<html>
	<head>
		<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
		<meta charset="utf-8" />
		<title>Password Check</title>
	</head>
	<body>
		<b id="Title">Password Strength Checker</b>
		<p id="info1">Insert your password below to test how it holds up against hackers!</p><br />
		<p id="info2">Don't worry, this website doesn't record or save any information... or does it?</p><br />
		<input id="helpBtn" type="button" value="Password Help" onclick="helpBtn_onClick()" />
		<input id="txtPWord" type="text" STYLE="background-color: #ffffff;" />
		<input id="btnCheck" type="button" value="Check" onclick="btnCheck_onClick()" />
		<script src="https://code.jquery.com/jquery-3.4.1.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
		<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
		<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
	</body>
	</html>

	<script>

		var minLength = 8
		var upper = 0
		var numbers = 0
		var special = 0
		var pLength = 0
		var combo = 0
		var strength = 0
		var changeLights

		/*                                                              This next chunk of commented out code consists of what I tried to do to affect the smartlights. I was unsuccessful in sending commands
		 *                                                              from JavaScript to the lights to change them, but I saved the code here commented out to show my attempts at doing so.
		 *                                                              
		 *                                                              The below code was taken from the treasure.js project, I thought it was a way of connecting to the lights API and sending commands,
		 *                                                              but I couldn't figure out how to implement it into the project I'm doing.


		function getLightURI(element)
		{
			var IP = "http://192.168.0.50/api/";
			var username = "stlaB2I6VZ8O80Qepc-1xfmLrHgyTFvB9IGupaQz";
			var lights = "/lights/";
			var URI = IP + username + lights;
			return URI + element.attr("id")+"/";
		}

		function togglelight(element)
		{
			var getState = $.getJSON(getLightURI(element), function (data)
			{
				var state = data["state"]["on"];
				if (state)
				{
					state = false;
				}
				else
				{
					state = true;
				}

				var lightState = {"on" : state};

				$.ajax({
					url: getLightURI(element) + "state/",
					type: "PUT",
					data: JSON.stringify(lightState)
				})
			})
		}

		function lightColour(element)                                   Here I attempted to adapt the code to change the colours, below was the attempt with when they have an extremely secure password,
		{                                                               but I couldn't get it to fit so well
			var getLightState = $.getJSON(getLightURI(element), function (data)
			{
				if (changeLights = 1)
				{
					var colour = data["hue"]["25500"];

					var setColour = { "hue": "25500" };

					$.ajax({
						url: getLightURI(element) + "hue/",
						type: "PUT",
						data: JSON.stringify(setColour)
					})
				}
			}
		}

		 /*                                                              Below here is a huejs API I found online and I tried to implement it, but I didn't fully understand how it worked or how to send 
		 *                                                              commands through this to the lights.

		var hue = jsHue();

		hue.discover().then(bridges => {
			if (bridges.length === 0) {
				console.log('No bridges found. :(');
			}
			else {
				bridges.forEach(b => console.log('Bridge found at IP address %s.', b.internalipaddress));
			}
		}).catch(e => console.log('Error finding bridges', e));

		var bridge = hue.bridge('192.168.0.50');

		// create user account (requires link button to be pressed)
		bridge.createUser('myApp#testdevice').then(data => {
			// extract bridge-generated username from returned data
			var username = data[0].success.username;

			console.log('New username:', username);

			// instantiate user object with username
			var user = bridge.user(username);
		});
		*/

		function btnCheck_onClick() {
			//Declaring the variables

			if (txtPWord.value.length > minLength) {
				for (i = 0; i < txtPWord.value.length; i++) {
					if (txtPWord.value[i].match(/[A-Z]/g)) {                                              //Checks if there are upper case letters in the password
						upper = upper + 1;
					}
					if (txtPWord.value[i].match(/[0-9]/g)) {                                              //Checks if there are numbers in the password
						numbers = numbers + 1;
					}
					if (txtPWord.value[i].match(/(.*[!,@,#,$,%,^,&,*,?,_,~])/g)) {                         //Checks if there are special characters in the password
						special = special + 1;
					}

					pLength = txtPWord.value.length - minLength;                                          //The length of a password contributes to its' security, so this variable saves how long the password is above the minimum necessary

					if (upper > 0 && numbers > 0 && special > 0) {                                  //Calculates the strength from a combo of all three additions
						combo = 6;
					}
					else if ((upper && numbers) || (upper && special) || (numbers && special)) {    //Calculates the strength from partial combos
						combo = 3;
					}
				}

				strength = upper + numbers + special + combo + (pLength / 2);                       //Calculates the strength of a password

				if (strength >= 12) {
					info1.innerText = "Outstandingly safe!";
					document.getElementById("txtPWord").style.backgroundColor = "#00AF08";          //Changes colour to dark green due to very safe password
					//changeLights = 1;
				} else if (strength >= 9) {
					info1.innerText = "Safe";
					document.getElementById("txtPWord").style.backgroundColor = "#83FF00";          //Changes colour to light green for safe password
					//changeLights = 2;
				} else if (strength >= 6) {
					info1.innerText = "Somewhat secure";
					document.getElementById("txtPWord").style.backgroundColor = "#FFFB00";          //Changes colour to yellow for slightly safe password
					//changeLights = 3
				} else if (strength >= 3) {
					info1.innerText = "Unsafe";
					document.getElementById("txtPWord").style.backgroundColor = "#FFAE00";          //Changes colour to orange for unsafe password
					//changeLights = 4;
				} else {
					info1.innerText = "Vulnerable";
					document.getElementById("txtPWord").style.backgroundColor = "#FF0000";          //Changes colour to red for vulnerable password
					//changeLights = 5;
				}
				//togglelight($(changeLights));
			} else {
				//Most security experts recommend 8 characters long as the barest minimum length for a password, so I decided to reflect that in this if statement to check if it's above that amount
				info1.innerText = "Your password is very short, it will be very easy to crack this. Think about making it at least 8 characters long.";
				document.getElementById("txtPWord").style.backgroundColor = "grey";
			}
		}

		function helpBtn_onClick() {
			window.alert("Passwords are very important to our digital safety. Having a strong password can make a huge difference to how likely it is to guess or crack with programs. Having a variety of upper and lowercase letters, numbers and special characters will increase how safe it is. Also, 8 characters is the bare minimum recommended by security professionals.");
		}
	</script>```
	

MotionCAPTCHA is a jQuery CAPTCHA plugin that requires users to sketch the shape they see in the canvas in order to dubmit the form.

### (Try the demo)[http://josscrowcroft.com/demos/motioncaptcha/]

## Important Introduction of sorts

### Proof of concept ... for now.

Please be aware that MotionCAPTCHA was pretty much designed initially to be a proof of concept - as it is right now, I can't see people actually using it in production - BUT, as the roadmap below hints, I have a solid plan for how to turn this into an actually usable plugin, involving some very elegant progressive enhancement and clever server-side processing.

I'm hoping people will be interested enough in this as a concept for me to devote a few weekends to turning it into a serious CAPTCHA alternative :o)

Check out **How It Works** below to see how the basic thing works right now, as of v0.1.

### No IE Support yet

Going to try to fix it up for the next version, but either way, see the roadmap for info.


## How It Works

So, in future, we will have progressive enhancement where the form uses a standard PHP (or similar weapon of choice) CAPTCHA script, and on page load, the plugin switches it for MotionCAPTCHA only if the browser is grown up enough.

For now, here's how it works (if you need a step-by-step guide, there's a **How-To** below):

* You manually disable your form, by emptying the form's `action` attribute, and placing its value in a hidden `<input>` with a specific ID. You should also put `disabled="disabled"` on the submit button, for added points.
* You add a few HTML lines to your form to initialise the MotionCAPTCHA canvas, and add the plugin's scripts to your page.
* The user draws the shape and, if it checks out, the plugin inserts the form's `action` into the `<form>` tag. The user can submit the form, happy days.

I know this is going to be unpopular in its current state, because you're making it impossible for people to submit the form without JavaScript or Canvas support - BUT, who gives a crap, it's fun. And in future, it'll degrade gracefully, etc etc.


## The Technology / Credits

The MotionCAPTCHA plugin combines one of the brushes ('Ribbon') and a lot of the logic from [Mr Doob's Harmony canvas experiment](http://mrdoob.com/projects/harmony/) with gesture recognition algorithms and vector shapes based entirely on the [$1 Unistroke Gesture Recognizer](http://depts.washington.edu/aimgroup/proj/dollar/) by Jacob O. Wobbrock, Ph.D. and Andrew D. Wilson, Ph.D., and the later [Protractor algorithm](http://www.yangl.org/pdf/protractor-chi2010.pdf) improvement by Yang Li, Ph.D. 

The idea originally came from [Paul Irish's blog](http://www.paulirish.com), where he combined the Ribbon brush and the Unistroke recognizer on the background of his site - you get a pretty special easter-egg if you draw a star. Great song.

In my book, all of these people are ace - I just borrowed from them and spliced their works into one unholy mess (actually it's fairly tidy, though I'm hoping people will suggest performance improvements.)


## Quick Roadmap:

### v0.2
* Try to fix up IE support via excanvas or similar library
* Add a real 'new shape' button (and internal shape-switcharoo function)

### v1.0
* a) Combine with a simple PHP CAPTCHA and server-side checking script, for progressive enhancement. By default, form is submitted with regular CAPTCHA. On page load, if browser supports JavaScript and HTML5 Canvas, regular CAPTCHA is switched for mighty MotionCAPTCHA, and that would be checked on the server instead.
* b) See (a).


## How To Use

* Add the plugin scripts: (I'm using jQuery 1.6 from the google API, but you could load it locally - and MotionCAPTCHA is supported down to jQuery 1.4):

	<!--[if IE]><script type="text/javascript" src="excanvas.js"></script><![endif]-->
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.6/jquery.min.js"></script>
	<script src="jquery.motionCaptcha.0.1.min.js"></script>
	<link href="jquery.motionCaptcha.0.1.css"></script>

* Code the form as usual, with a unique ID (eg. '#mycoolform') and set the form action to blank (or '#') - eg:

	<form action="#" id="mycoolform" method="[get/post]">

* Add this placeholder <div> element to your form (NB. use <fieldset>s if you need it to validate) containing the blank canvas:

	<div id="mc">
		<p>Please draw the shape in the box to submit the form:</p>
		<canvas id="mc-canvas"></canvas>
	</div>

* Disable the submit button, eg:

	<input type="submit" disabled="disabled" value="Submit Form" />

* Add a hidden input to the form, with the form action as its value. Give it either a unique id, or the id 'mc-action', like so:

	<input type="hidden" id="fairly-unique-id" value="submitform.php" />

* Call the plugin on the form element. If you used any other ID than 'mc-action' for the hidden input, you'll just need to pass it to the plugin, like this:
	
	$('#form-id').motioncaptcha({
		action: '#fairly-unique-id'
	});
	
	// Or, if you just used 'mc-action' as the hidden input's ID:
	$('#form-id').motioncaptcha();


* Other options are available (defaults are shown):

	$('#form-id').motioncaptcha({
		// Basics:
		action: '#mc-action',        // the ID of the input containing the form action
		divId: '#mc',                // if you use an ID other than '#mc' for the placeholder, pass it in here
		cssClass: '.mc-active',      // this CSS class is applied to the 'mc' div when the plugin is active
		canvasId: '#mc-canvas',      // the ID of the MotionCAPTCHA canvas element
		
		// An array of shape names that you want MotionCAPTCHA to use:
		shapes: ['triangle', 'x', 'rectangle', 'circle', 'check', 'caret', 'zigzag', 'arrow', 'leftbracket', 'rightbracket', 'v', 'delete', 'star', 'pigtail'],
		
		// These messages are displayed inside the canvas after a user finishes drawing:
		errorMsg: 'Please try again.',
		successMsg: 'Captcha passed!',
		
		// This message is displayed if the user's browser doesn't support canvas:
		noCanvasMsg: "Your browser doesn't support <canvas> - try Chrome, FF4, Safari or IE9."
		
		// This could be any HTML string (eg. '<label>Draw this shit yo:</label>'):
		label: '<p>Please draw the shape in the box to submit the form:</p>'
	});
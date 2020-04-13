## Recreating the logo of the Eurovision Song Contest 2020 with CSS

#### Live demo on [CodePen](https://codepen.io/Co_is_tired_of_his_username_being_taken/pen/PowpmVe)

I found the concept behind the logo of the 2020 Eurovision Song Contest really interesting and creative, so much that I wanted to give a go at recreating it using CSS and maybe tweak it a little bit (why was the Australian flag represented like that anyway).

I was aware CSS had come a pretty long way over the last decade but I'm not exactly a frontend expert sadly - so I needed help.

I first tried playing with basic shapes and looked at what I could do with cones. That wasn't exactly a success. I left it there and though I'd maybe come back to it another day.

A week or so later I had another idea and started picturing the logo as a piechart, where each "flag slice" would be a section of the chart.

Like I said, I'm by no means a CSS expert. Needless to say I had no clue how to make a piechart with CSS. Luckily, others did and were kind enough to write tutorials for it. The first thing I found on Google was [this brilliant article](https://codeburst.io/how-to-pure-css-pie-charts-w-css-variables-38287aea161e) and I didn't need to look any further. Without going into the article's details (check it for yourself!), this is what I managed to get after a little while:

![First iteration. Not quite the joyful colorfest yet.](https://github.com/corentindautreme/esc-countryslices/blob/master/screenshots/001.png)

Here's the code so far. The grey background is simply here for visibility purposes.

```
<style type="text/css">
	.circle {
		width: 540px;
		height: 540px;
		margin: 150px auto;
		position: relative;
		overflow: hidden;
		border-radius: 100%;
		background: #d0d0d0;
	}

	.slice {
		width: 100%;
		height: 100%;
		position: absolute;
		overflow: hidden;
		transform: translate(0, -50%) rotate(90deg) rotate(calc(var(--offset, 0) * 1deg));
		transform-origin: 50% 100%;
	}

	.slice:before {
		width: 100%;
		height: 100%;
		background: #000;
		content: '';
		position: absolute;
		transform: translate(0, 100%) rotate(calc(var(--value, 45) * 1deg));
		transform-origin: 50% 0;
	}
</style>

 <body>
 	<div class="circle">
		<div class="slice" style="--offset: 0; --value: 6;"></div>
		<div class="slice" style="--offset: 16; --value: 18;"></div>
	</div>
</body>
```

Next step - colors! Thankfully, the design used on the Eurovision logo was simple enough to reproduce - each slice is made of exactly 3 parts of equal size.
Before adding the colors, let's take a step back and remove the `overflow: hidden` on both the .slice and .circle classes. If you haven't read the css piechart article I've linked earlier (and you should!), this is what happens behind the scenes - we use the :before pseudo element and rotate it just enough to make it fit in the actual slice element. Hiding the overflow helps making it look clean and sharp.

![Behind the scenes](https://github.com/corentindautreme/esc-countryslices/blob/master/screenshots/002.png)

Now onto the colors. I'm not gonna lie, it took me a few tries (and some scribbling on a nearby piece of paper) to get this right. The idea was to use a [radial-gradient](https://developer.mozilla.org/fr/docs/Web/CSS/radial-gradient) and get it to originate from the right place on the :before pseudo element to get it to look right. After a way too long period of trial and error (my brain gets easily confused with this kind of stuff), hurray! It works! I tweaked the opacity of the .slice and its :before just for the sake of this screenshot. The intersaction of the .slice (the transparent grey sqaure) and its :before is the resutling slice.

![Colors!](https://github.com/corentindautreme/esc-countryslices/blob/master/screenshots/003.png)

![Cleaning up](https://github.com/corentindautreme/esc-countryslices/blob/master/screenshots/004.png)

And here's the css used for the background:

```
background: radial-gradient(circle at 50% 0%,
	blue 0,
	blue 16%,
	white 16%,
	white 33%,
	red 33%,
	red 100%
);
```

There's only one thing left now and it's not the fun part: extracting the colors used on the logo and recreate & place all of the slices on our piechart. Well, noone was going to do that for me sadly, but there's no reason you should do it all over again - so if anyone's interested, here they are (I took each slice clockwise):

```
#0750c6 #fff #fc0000
#01aa5a #fff #fc0000
#ffc832 #000 #fc0000
#0750c6 #fff #fc0000
#0750c6 #ffc832 #0750c6
#000 #0750c6 #fc0000
#1ac0f8 #fff #fc0000
#1ac0f8 #fff #ffc832
#01aa5a #fc0000 #fc0000
#01aa5a #fff #ff8c32
#fff #fff #fc0000
#fff #1ac0f8 #fff
#0750c6 #fff #0750c6
#fff #ff8c32 #fff
#fff #fc0000 #0750c6
#fff #0750c6 #fc0000
#0750c6 #fff #000
#0750c6 #fff #fc0000
#01aa5a #ffc832 #fc0000
#fc0000 #ffc832 #fc0000
#be0000 #fff #be0000
#ffc832 #ffc832 #1ac0f8
#fc0000 #000 #fc0000
#fc0000 #01aa5a #fc0000
#1ac0f8 #ffc832 #fc0000
#fff #01aa5a #fc0000
#ffc832 #0750c6 #fc0000
#0750c6 #fff #fc0000
#01aa5a #fc0000 #1ac0f8
#01aa5a #fff #1ac0f8
#fc0000 #fff #0750c6
```

You may notice the same colors are being re-used which may my job slightly easier.

And that's pretty much it! The rest was all about guessing the right size and position of everything. You can check the final version of the code right here on Github, or on [CodePen](https://codepen.io/Co_is_tired_of_his_username_being_taken/pen/PowpmVe).
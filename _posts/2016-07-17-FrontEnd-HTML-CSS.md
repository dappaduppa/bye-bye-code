---
layout: post
title:  "FrontEnd HTML CSS"
date:   2016-07-17 14:28:46 +0900
categories: HTML, CSS
---

```css

html element based:
------------------

p {
     text-align: center;
     /* This is a single-line comment */
     color: red;
     
     /* This is
		a multi-line
		comment */ 
} 


html element with class based:
-----------------------------
p.center {
     text-align: center;
     color: red;
}


html "id" based:
----------------
#centerme {
     text-align: center;
     color: red;
}


group selector:
---------------

h1, h2, p {
     text-align: center;
    color: red;
}

-------------------------------------------------------------------
external stylesheet:
-------------------
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>

	mystyle.css:
	-----------
	
	body {
    	background-color: lightblue;
	}

	h1 {
	    color: navy;
	    margin-left: 20px;
	    /*
	    Note: Do not add a space between the property value and the unit (such as margin-left: 20 px;). The correct way 			is: margin-left: 20px;
	    */
	    
	}
	
-------------------------------------------------------------------
Internal style sheet
-------------------
	<head>
		<style>
			body {
			    background-color: linen;
			}

			 h1 {
			    color: maroon;
			    margin-left: 40px;
			} 
		</style>
	</head>

-------------------------------------------------------------------
Inline style sheet
-------------------

	<h1 style="color:blue;margin-left:30px;">This is a heading</h1>
	

Multiple Style Sheets
---------------------
If some properties have been defined for the same selector (element) in different style sheets, the value from the last read style sheet will be used



Colors in CSS are most often specified by:
" a valid color name - like "red" ( "Red" is the same as "red" or "RED". )
		HTML and CSS supports 140 standard color names..
		https://www.w3schools.com/colors/colors_names.asp
" an RGB value - like "rgb(255, 0, 0)"
" a HEX value - like "#ff0000"


rgb

FF --> 255

#000000 --> black
#FFFFFF --> white



CSS background properties:
" background-color
" background-image
" background-repeat
" background-attachment
" background-position


body {
    background-color: lightblue;
} 
body {
    background-image: url("paper.gif");
    /*
    By default, the background-image property repeats an image both horizontally and vertically.
    */
}

body {
     background-image: url("gradient_bg.png");
     background-repeat: repeat-x; /* repeated only horizontally  */
     /* To repeat an image vertically, set background-repeat: repeat-y; */
     /* background-repeat: no-repeat; not to repeat*/
     /*  background-position: right top; */ /* positioning the bg position */
     /* background-attachment: fixed; */
     /*  To specify that the background image should be fixed (will not scroll with the rest of the page),
      use the background-attachment property: */
} 


/* To shorten the code, it is also possible to specify all the background properties in one single property. 
This is called a shorthand property.
The shorthand property for background is background:
*/

body {
    background: #ffffff url("img_tree.png") no-repeat right top;
}

```

https://www.w3schools.com/css/css_border.asp


---------------------------------------------------------------------------



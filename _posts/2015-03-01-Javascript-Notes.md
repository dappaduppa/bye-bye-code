---
layout: post
title: "Javascript notes" 
date:   2015-03-01 10:56:46 +0900
categories: Javascript
---

## classic method, only one method can be attached to event.
```javascript
window.document.getElementById("btn1").onclick = function() {
    alert("clicked")
}


// modern approach //TODO naming

window.document.querySelector("#btn1").addEventListener( "click", function(e) {
        alert("clicked");
        e.stopPropogation(); // stop bubble (or) capturing
        e.preventDefault(); // prevent the default browser event
    }
    // , false-> bubbling, and true=> capturing
);


in the classic method,
    return false;
        does two things

    1) it stop bubble/capturing
    2) prevent default browser events.
```


----------------
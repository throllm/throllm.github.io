---
layout: post
title:  "Github Pages with jQuery and JavaScript?"
jquery: true
---
Is it possible to use JavaScript and jQuery on GitHub pages? Here is an example.

- include all necessary files and functions into your custom header file
- define the corresponding code in the page file 

```
<link rel="stylesheet" href="//code.jquery.com/ui/1.13.2/themes/base/jquery-ui.css">
<script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
<script src="https://code.jquery.com/ui/1.13.2/jquery-ui.js"></script>
<script>
<script>
  $( function() {
    $( "#selectable" ).selectable();
  } ); 
  </script>
$().ready(function(){
    $("#text").html("Hello JavaScript");
  }); 
</script>
```
```
<div id="text"></div>
<ol id="selectable">
  <li class="ui-widget-content">Item 1</li>
  <li class="ui-widget-content">Item 2</li>
  <li class="ui-widget-content">Item 3</li>
  <li class="ui-widget-content">Item 4</li>
  <li class="ui-widget-content">Item 5</li>
  <li class="ui-widget-content">Item 6</li>
  <li class="ui-widget-content">Item 7</li>
</ol>
```
Here is the result:

##### Set HTML contents
<div id="text"></div>

##### jQuery Selectable
[jQuery Selectable][jqueryui-selectable]
<ol id="selectable">
  <li class="ui-widget-content">Item 1</li>
  <li class="ui-widget-content">Item 2</li>
  <li class="ui-widget-content">Item 3</li>
  <li class="ui-widget-content">Item 4</li>
  <li class="ui-widget-content">Item 5</li>
  <li class="ui-widget-content">Item 6</li>
  <li class="ui-widget-content">Item 7</li>
</ol>

[jqueryui-selectable]:https://jqueryui.com/selectable/
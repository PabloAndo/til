# MANIPULATING THE DOM

Most common way to start manipulating the DOM is:  
`$('.red-box').fadeOut(2000);`  
The dollar sign can be changed by 'jQuery', it works the same.  
Then, CSS selector. In the example is a class.  
CSS SELECTORS:

- . : classes
- \# : id's

## Methods

- fadeOut(milliseconds) : makes the component disappear after some seconds.
- fadeIn(milliseconds) : reveals again the component
- fadeTo(1000, 0.5) : fades the component until certain opacity. Making fade to opacity to 0, will not make the other components move.
- fadeToggle(milliseconds) : toggle the state of the component (if its hidden it will be shown in the end and the other way around) **ATTENTION :** it only fades or shows until the initial opacity
- slideUp(milliseconds) : fade or show an element. the effect goes from bottom to top. It's handy fow showing **FORMS**
- slideDown(milliseconds) : the same as before but from top to bottom.
- animate : example  
   `$('.blue-box').animate( { 'margin-left': '+=200px' }, 1000, 'linear' );`  
   changes the CSS properties. 1000 is the time in milliseconds, linear is how it animates it.

  Multiple properties can be changed at once: `$(function() { $('.blue-box').animate( { 'margin-left': '200px', opacity: '0', height: '50px', width: '50px', 'margin-top': '25px' }, 1000 ); });`

- delay(milliseconds) : to introduce a delway between animations. Example: `$('.green-box'.delay(1000).fadeIn(1000)`

Another way to chain animations is using callbacks functions. Even though soon will be callback hell.
Example:

```javascript
$(function() {
  $('.red-box').fadeTo(1000, 0, function() {
    $('.green-box').fadeTo(1000, 0, function() {
      $('.blue-box').fadeTo(1000, 0);
    });
  });
});
```

## Element Selectors

Cheat sheet: <https://www.w3schools.com/jquery/jquery_ref_selectors.asp>

```javascript
$(function() {
  $('p').css('background-color', 'rgba(180,180, 30, 0.8'); // elements
  $('.red-box').css('background-color', 'rgba(180,180, 30, 0.8'); // classes
  $('#list').css('background-color', 'rgba(180,180, 30, 0.8'); // ids
  $('input[type="text"]').css('background-color', 'rgba(180,180, 30, 0.8'); // selection for inputs
  $('h2, p, input').css('background-color', 'rgba(180,180, 30, 0.8'); // multiple elements at a time
  $('p:first').css('background-color', 'rgba(180,180, 30, 0.8'); // only first p element
  $('list:last').css('background-color', 'rgba(180,180, 30, 0.8'); // only last list element
  $('li:odd').css('background-color', 'rgba(180,180, 30, 0.8'); // one selected, one not...

  $('input:password').css('background-color', 'rgba(180,180, 30, 0.8'); // jquery selector
});
```

## jQuery selector for TRAVERSAL

- Selecting all the "li" elements and its children: `$('#list') .find('li') .css('background-color', 'rgba(180,180, 30, 0.8');`

- Selecting only the direct children and just that: `$('#list') .children('li') .css('background-color', 'rgba(180,180, 30, 0.8');`

- Selecting the parents: `parents()`
- Selecting only the first parent: `parent()`
- Selecting the siblings: `siblings("p")`
  - `siblings(":header")`
- Select the next element: `$('#list').next().css('background-color', 'rgba(180,180, 30, 0.8');`
- Select the previous element: `$('#list').prev().css('background-color', 'rgba(180,180, 30, 0.8');`

## Filtering Methods

- Filter the even elements: `$('#list').children("li").filter(":even").css('background-color', 'rgba(180,180, 30, 0.8');`

- Filter by index:

  ```javascript
  $('li')
    .filter(function(index) {
      return index % 3 === 0;
    })
    .css('background-color', 'rgba(180,180, 30, 0.8');
  ```

- First element: `$("li").first().css()`
- Last element: `$("li").last().css()`
- Targeting the nth element: `$("li").equal(3).css()` or `$("li").equal(-2).css()`
- Targeting not the first: `$("li").not(":first").css()`
- Other ways to use "not": `$("li").not("#list ul li").css()`

**FILTER is positive condition.  
NOT is negative condition.**

## Adding new elements to the DOM

```javascript
$(function() {
  // 1) Appending elements to existing elements.
  // -> This adds elements as the last children of a given element

  // Append a new list item to the end of the first sub list
  $('ul ul:first').append('<li>New sub item</li>');

  // Other way to achieve the same effect
  $('<li>Another new sub item</li>').appendTo('ul ul:first');

  // 2) Prepending elements to existing elements.
  // -> This adds elements as the first children of a given element

  // Prepend a new list item to the start of the first sub list
  $('ul ul:first').prepend('<li>First sub item</li>');

  // Other way to achieve the same effect
  $('<li>Very first sub item</li>').prependTo('ul ul:first');

  // 3) Adding elements as siblings of existing elements

  // Add a new elements after an existing one (as next sibling)
  $('.red-box').after("<div class='red-box'>New Red</div>");

  // Add a new elements before an existing one (as previous sibling)
  $('.blue-box').before("<div class='blue-box'>New Blue</div>");

  // You can also use a callback function to construct more complex
  // elements to add to the DOM
  $('.green-box').after(function() {
    // Maybe more complex string of new element(s)
    return "<div class='green-box'>New Green</div>";
  });

  // 4) Add existing elements to other existing elements
  // -> This will move the added element, so the original vanishes

  // For instance, add the #list after the first paragraph (removes it from its
  // original position).
  // In case it's added to multiple elements, jQuery must clone the added element.
  $('p:first').after($('#list'));
});
```

## Replacing Elements and content

```javascript
$(function() {
  // Replace each selected item with another one.
  // For instance, replace all <p> tags with another one.
  $('p:first').replaceWith("<p>I've been replaced.</p>");

  // Again, to construct more complex elements, you may want
  // to do that in a callback function:
  $('p:last').replaceWith(function() {
    return '<p>Complex new paragraph...</p>';
  });

  // Also, you can again replace an element with another existing
  // element which will remove it from its original position.
  $('.red-box').replaceWith($('.blue-box'));

  // You can also replace multiple items at the same time.
  $('li:first, li:last').replaceWith('<li>First or last list item</li>');
});
```

## Removing Elements and Content

````javascript
$(function () {

  // You can remove an existing element simply with remove().
  // For instance, the first main item:
  $("li:first").remove();

  // If you want to reuse/re-display the element later, you should
  // use detach(). This also keeps all attached event handlers alive.
  // First, this removes the element from the DOM:
  var detached = $("p:first").detach();
  // Next, this will re-attach the element to the DOM (at another position):
  $("#list").after(detached);

  // To remove all elements *inside* a given elements, you can use empty().
  // The element itself remains in the DOM, so in this case the <form>.
  $("form").empty();

});```
````

## Manipulating Attributes and Properties

```javascript
$(function() {
  // 1) Attributes with attr()

  // To read the current value of an attribute, use attr() with only
  // one argument: the name of the attribute you want to read.
  var link = $('#link');
  // Retrieve the value of the href attribute of the <a> tag
  // This is logged into the console. To see it, press F12 in Firefox/Chrome
  // to open the Developer Tools and click on the Console tab.
  console.log(link.attr('href'));
  // Log the value of the title attribute
  console.log(link.attr('title'));

  // To set the value of an attribute, you call the attr() function with two
  // arguments: first, the name of the attribute you want to change, and second
  // the new value:
  link.attr('href', 'http://petersommerhoff.com');

  // 2) Properties with prop()

  // To read the _current_ value of a checkbox for instance, you must use the
  // prop() function:
  var radioButton = $('input[type=radio]:first');
  console.log(radioButton.prop('checked')); // false
  console.log(radioButton.attr('checked')); // undefined

  // 3) Values with val()
  // -> Read or set the value attribute of input elements

  // To retrieve the value of an input element, simply call val()
  var email = $('input[type=email]').val();
  console.log(email); // i@mine.me
  var number = $('input[type=range]').val();
  console.log(number); // 3

  // To set a new value, pass the new value as an argument to val():
  $('input[type=text]').val('Peter Sommerhoff');
  $('input[type=range]').val(9);

  // If you have multiple elements selected, using val() will implicitly
  // always call first() beforehand, so only change the first input element:
  var value = $('input').val();
  console.log(value); // Peter Sommerhoff
});
```

## Changing the CSS Properties

```javascript
$(function() {
  // To retrieve or set the value of a CSS property, you can use css().
  // This works similar to attr() and prop(): pass one argument to read
  // or two arguments to set a new value.

  // For instance, you can achieve the same as with hide() like this:
  $('p:first').css('display', 'none');

  // Let's read the width property of .red-box:
  var redBox = $('.red-box');
  console.log(redBox.css('width')); // 80px
  // To get the width or height without unit, you can use width() and height():
  console.log(redBox.width()); // 80

  // You can change any CSS property in this simple way.
  redBox.css('background-color', '#AA5500');
  $('p').css('font-size', '20px');

  // You can even change the value of a property relative to its previous value
  redBox.css('width', '+=20px');
  redBox.css('height', '-=20px');

  // Also, you can retrieve multiple values by passing an array as the argument
  var properties = $('p').css(['font-size', 'line-height', 'color']);
  // The result is an object containing all values
  console.log(properties);
  console.log(properties['font-size']); // 20px

  // In line with jQuery's other function, you can also use a callback function
  // instead of passing in a new value directly.
  // This example also demonstrates that jQuery automatically uses the correct
  // vendor prefix for the CSS property depending on the browser, which is
  // -moz-user-select in this case.
  // You can see that in the Developer Tools (F12 in browser) in the Inspector tab.
  redBox.css('user-select', function() {
    // [some logic here...]
    return 'none';
  });
});
```

## Adding CSS Classes

```javascript
$(function() {
  // Additional CSS classes can be added to an element via addClass()

  // For instance, add the class .fancy-link to all anchor tags.
  // Note that you *do not* add a dot in front of the class name here.
  $('a').addClass('fancy-link');

  // You can also add multiple classes at once.
  $('p:first').addClass('large emphasize');

  // To attach a class based on the index of the element in your selected
  // set of elements, you can use a callback function.
  $('li li').addClass(function(index) {
    // This adds classes .item-0, .item-1, ... to the list's sub-items.
    $(this).addClass('item-' + index);
  });

  // You can even use two parameters for the callback function, the index
  // and the current class of the element.
  $('div').addClass(function(index, currentClasses) {
    console.log(currentClasses); // String containing all classes

    // Add .red-box if the current classes contain "dummy"
    if (currentClasses.indexOf('dummy') !== -1) {
      return 'red-box';
    }
  });

  // You can again chain these calls to switch one class for another.
  $('.green-box')
    .removeClass('green-box')
    .addClass('blue-box');

  // To quickly toggle one particular CSS class, use toggleClass().
  $('li li:first').toggleClass('emphasize');
});
```

## Changing the Data of an element

```javascript
$(function() {
  // jQuery allows you to attach arbitrary data to any element, represented as
  // HTML attributes prefixed with "data-", e.g. "data-images".
  // Here, we'll attach data about all available images to the gallery itself.

  // Select the <img> inside the gallery to manipulate it later
  var gallery = $('.gallery');

  // Initialize an array of all images for the gallery
  var images = [
    'images/laptop-mobile_small.jpg',
    'images/laptop-on-table_small.jpg',
    'images/people-office-group-team_small.jpg'
  ];

  // To attach data, use the data() function and pass in two arguments: first,
  // the key for the data, and second the actual data/value.
  gallery.data('availableImages', images);
  // To retrieve the data, again use just one argument: the key.
  console.log(gallery.data('availableImages')); // Array[...]

  gallery.data('name', 'The Amazing Gallery');
  console.log(gallery.data('name')); // The Amazing Gallery

  // Remove data just as easily using removeData().
  gallery.removeData('name');
  console.log(gallery.data('name')); // undefined

  // If you attach data directly in HTML via an attribute prefixed with "data-",
  // you can read that automatically from jQuery.
  var data = $('p:first').data('mydata');
  console.log(data); // Data coming from HTML attribute
});
```

In the HTML we can prefix to retrieve the data, for instance:

```html
<p data-mydata="Some data here"></p>
```

The prefix data is interpreted by jQuery.

## Retreiving and Changing the Content of an Element

```javascript
$(function() {
  // 1) Pure text content: text()

  // To read or change the textual content of an element, you use text().
  var p = $('p:first');
  console.log(p.text()); // Lorem ipsum...

  p.text("Now it's a very short paragraph.");
  console.log(p.text()); // Now it's a very short paragraph.

  // 2) HTML content: html()

  // In contrast to text(), html() allows you to retrieve the content of an
  // element including the HTML code of its content/children.
  var p2 = $('p').eq(1); // Second <p> tag
  console.log(p2.text()); // Lorem ipsum...
  console.log(p2.html()); // <span>Lorem</span> ipsum ...

  // Similarly, you can set the HTML content of an element with html()
  // whereas text() would escape the HTML code to make it show as text.
  $('li ul')
    .eq(0)
    .text('<li>Not an item</li>');
  $('li ul')
    .eq(1)
    .html('<li>Correct item</li>');

  // You can use this in a way that appends content to an element.
  p2.text(p2.text() + ' This is an additional sentence.');
  p2.html(p2.html() + " <span class='emphasize'>And another emphasized one.</span>");
});
```

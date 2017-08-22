# **Autocomplete-Typing**

Project 6 out of 30 mini projects written from scratch using only HTML5, CSS3 and ES6 vanillaJS.

This project consists in creating a type-ahead/auto-complete form, that suggests results on user input. The final result looks like this:

![alt text](https://github.com/itaouil95/Autocomplete-Typing/blob/master/autocomplete.png)


### HTML

The structure of the project is really easy going. A simple form containing an input text where the user will insert the US state of city being searched and an unordered list where we will display the list of cities or states.

```HTML
<form class="search-form">

  <input type="text" class="search" placeholder="City or State"></input>
  <ul class="suggestions">
    <li>Filter by city</li>
    <li>or state</li>
  </ul>

</form>
```

### CSS

The next step is to style the text search and the global tags (as I like to call them). Moreover, we centre the form and set the dimensions of the input search and form.

```CSS
html {
  box-sizing: border-box;
  background-color: #ffc600;
  font-family: 'Open Sans', sans-serif;
  font-weight: 400;
  font-size: 20px;
}

form {
  max-width: 400px;
  margin: 50px auto;
}

input.search {
  width: 120%;
  padding: 20px;
  text-align: center;
  font-size: 40px;
  border: 10px solid #F7F7F7;
  border-radius: 5px;
  z-index: 2;
  position: relative;
  left: -12%;
  top: 10px;
}
```

Now it is time for the suggestions, where we are going to display the data regarding the search.

```CSS
.suggestions {
  margin: 0px;
  padding: 0px;
  position: relative;
  left: 5%;
}

.suggestions li {
  list-style: none;
  background-color: white;
  text-transform: capitalize;
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #D8D8D8;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.14);
  padding: 20px;
}

.suggestions li:nth-child(even) {
  transform: perspective(100px) rotateX(3deg) translateY(2px) scale(1.001);
  background: linear-gradient(to bottom, #ffffff 0%, #EFEFEF 100%);
}

.suggestions li:nth-child(odd) {
  transform: perspective(100px) rotateX(-3deg) translateY(3px);
  background: linear-gradient(to top, #ffffff 0%, #EFEFEF 100%);
}
```

The interesting part here is the transformation of the list items using the perspective, rotations and translation animations.


### JavaScript

And here is the JavaScript part where we make use of *fetch* web api to fetch asynchronously across the browser and of the latest ES6 to write some nice and clean JavaScript.

```JavaScript
// Some references
const cities = [];
const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';

// Number with commas
function numberWithCommas(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}

// Find matches on the dataset
function findMatches(wordToMatch, cities) {
  const regex = new RegExp(wordToMatch, 'gi');
  return cities.filter(place => place.city.match(regex) || place.state.match(regex));
}

// Render matches on page
function renderMatches() {
  const matches = findMatches(this.value, cities);
  const html = matches.map(match => {
    const regex = new RegExp(this.value, 'gi');
    const cityName = match.city.replace(regex, `<span class='hl'>${this.value}</span>`);
    const stateName = match.state.replace(regex, `<span class='hl'>${this.value}</span>`);
    return `
      <li>
        <span>${cityName}, ${stateName}</span>
        <span>${numberWithCommas(math.population)}</span>
      </li>
    `
  }).join('');

  // Inject html in the DOM
  suggestions.innerHTML = html;
}

// DOM references
const searchInput = document.querySelector('.search');
const suggestions = document.querySelector('.suggestions');

// Fetch data (return a Promise)
fetch(endpoint)
  .then(response => response.json())
  .then(data => cities.push(...data)); // deconstruct the array (spread operator)

// Search Input event listener
searchInput.addEventListener('keyup', renderMatches);
```

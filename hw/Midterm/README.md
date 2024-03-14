# Midterm Presentation: "not Michelin Guide"

## Restaurant Data

Intially, I tried to use JSON file to save restaurant data and call in each HTML file. Because of CORS error, I decided to create an array of restaurant data in the HTML files (`./index.html` & `./restaurants/index.html`).

```
const cuisines = [
  {
    name: "American",
    restaurants: [
      {
        name: "Juliana's Pizza",
        dish_name: "Margherita Pizza",
        dish_img: "./img/julianas.webp",
        description: "A classic thin-crust Italian pizza from Naples",
        short_address: "Brooklyn, NY",
        full_address: "19 Old Fulton St, Brooklyn, NY 11201",
        gmap_link:
          "https://www.google.com/maps...",
        cuisine: "American",
      },
      ...
    ]
  },
  ...
]
```

## Rendering restaurant cards

```
<main>
  <div id="main_container"></div>
</main>
<script>
const mainContainer = document.getElementById("main_container")
const cuisineHTML = cuisines
  .map((cuisine) => {
    const restaurantCards = cuisine.restaurants
      .map(
        (restaurant) => `
          <a class="card" href="./restaurants/index.html?restaurant=${restaurant.name}">
            <img src="${restaurant.dish_img}" class="card_img" alt="" />
            <div class="card_content">
              <p class="card_title">${restaurant.dish_name}</p>
              <p class="card_desc">${restaurant.description}</p>
              <p class="location">${restaurant.name} - ${restaurant.short_address}</p>
            </div>
          </a>
        `
      )
      .join("")

    // restaurantCards are inside the corresponding cuisine section
    return `
      <div id="cuisine_container" class="cuisine_container">
        <h2 class="cuisine_title">${cuisine.name}</h2>
        <div class="card_container">
          ${restaurantCards}
        </div>
      </div>
    `
  })
  .join("")

<!-- ADDING INNERHTML CREATED FROM THIS SCRIPT TO THE HTML ELEMENT -->
mainContainer.innerHTML += cuisineHTML
</script>
```

## Restaurant Page

For the restaurant page, I only created one file, "restaurant.html." Since the file has all the data of each restaurant and reads the URL Parameter, "restaurant", the file only renders the corresponding restaurant information.

```
<!-- URL PARAMETER -->
const urlParams = new URLSearchParams(window.location.search)
const restaurantParam = urlParams.get("restaurant")

<!-- RESTAURANTS DATA -->
const restaurants = {
  "Juliana's Pizza": {
    name: "Juliana's Pizza",
    dish_name: "Margherita Pizza",
    dish_img: "../img/julianas.webp",
    description: "A classic thin-crust Italian pizza from Naples",
    short_address: "Brooklyn, NY",
    full_address: "19 Old Fulton St, Brooklyn, NY 11201",
    gmap_link: "...",
    cuisine: "American",
  },
  ...
}

<!-- MATCHING THE RESTAURANT PARAM AND THE RESTAURANTS DATA -->
const restaurant = restaurants[restaurantParam]

<!-- FIND THE HTML ELEMENT -->
const restaurantContainer = document.getElementById(
  "restaurant_container"
)
const pageTitle = document.getElementById("page_title")

<!-- CREATE AN INNER HTML ELEMENT -->
if (restaurant) {
  pageTitle.innerHTML = `${restaurant.name}`
  restaurantContainer.innerHTML = `
    <div class="text_container">
      <p class="title">${restaurant.name} <a class="address" href=${restaurant.gmap_link} target="_blank">${restaurant.full_address}</a></p>
      <p class="subtitle">Highlight: ${restaurant.dish_name}</p>
      <p class="description">${restaurant.description}</p>
    </div>
    <div class="content">
      <img src=${restaurant.dish_img} class="dish_img" />
    </div>
  `
} else {
  restaurantContainer.innerHTML = "<p>Restaurant Not Found</p>"
}
```

## Next Steps

- Implementing Google Maps API to make the website more interactive
  - For Midterm, I decided not to implement it because writing an API key in the plain html file is not secure and I don't want to expose it to the public.
- Use React :)

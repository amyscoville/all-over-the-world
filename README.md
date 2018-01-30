# All Over The World - React/Redux with API Integration

## Objectives

1) Build an educational website with a child-friendly interface that helps users become familiar with countries around the world.

2) Meet the following assignment requirements:
  * Online API integration
  * Single Page Application with 3 page views using React Router
  * Responsive

### Homepage
![homepage](public/images/home.png)

### Countries
When component mounts, a request is sent to the server for a full list of countries, and the page displays all country names in Link components.

![list of countries](public/images/countries.png)

### Individual Country Page
When component mounts, a request is sent to `https://restcountries.eu/rest/v2/alpha/:alpha2code` using the 2-letter country code. Country-specific response data is manipulated in JavaScript and displayed on the page.

![alt text](public/images/country.png)

## Technologies Used
* React
* JavaScript
* HTML
* CSS
* React Router
* AJAX
* Redux
* Axios + Thunk

## Code Snippets

Although Redux is generally unnecessary for applications of this size, I chose to use Redux and Thunk here to solidify my understanding of the concepts in preparation for building larger applications in the future. API requests are made in Redux action creators, applying Thunk as middleware:

```
export function getCountries() {
    return function(dispatch) {
        axios.get(countriesUrl)
            .then(response => {
                dispatch({
                    type: 'GET_COUNTRIES',
                    countries: response.data
                })
            })
            .catch(err => {
                console.error(err);
            })
    }
}
```

Functions making API requests in Redux are made available as props through the React-Redux `connect()` method. They are called from individual components in the life cycle method `componentDidMount()`:

```
componentDidMount() {
        this.props.getCountries();
    }
```

Data about languages and currencies for each country comes in an array of objects. This posed a challenge when it came to displaying the language and currency names on the individual country page, as the sentence structure needed to change based on the length of the array. I used JavaScript to manipulate the data and return nicely formed sentences:

```
toCurrencyString(currencies, countryName) {
    let currStr = `People in ${countryName} use `;
    if (currencies.length === 1) {
        currStr += `the ${currencies[0].name} to buy things.`;
    } else if(currencies.length === 2) {
        currStr += `the ${currencies[0].name} and the ${currencies[1].name} to buy things.`;
    } else if(currencies.length > 2) {
        for(let i = 0; i < currencies.length; i++) {
            if (i === currencies.length - 1) {
                currStr += ` and the ${currencies[i].name} to buy things.`;
            } else if (i < currencies.length - 1) {
                currStr += `the ${currencies[i].name}, `;
            }
        }
    } 
    return currStr;
}
```

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

---
layout: post
title:      "React-Redux Project"
date:       2021-06-17 16:36:13 -0400
permalink:  temp_blog_for_react
---


With the react-redux project some of the most fun and challenging hurdles to climb were learning to work with react-router and learning how to destructure components.
# Routing

Understaind react-router really enables you to make a felxible application in a relatively clean format. 
Learning how to do this dynamically helps even more.

In my project I used the following code in my App.js file:

```
import

...

import {BrowserRouter as Router, Switch, Route} from 'react-router-dom'


 <Router>
  //         <div>
  //           <NavBar locations={this.props.locations}/>
  //             <Switch>
  //               <Route path="/locations/:id" render={routerProps => <Location location={this.props.locations.find(loc => loc.id === routerProps.match.params.id )}/>}/>
  //               <Route exact path='/' component={Locations} />
  //             </Switch>
  //         </div>
  //       </Router>
	
...

```

Router is a component imported ad BrowserRouter  from 'react-router-dom'.  It is necessary to contain links and routes within a div in this component.

My NavBar is linked to this complonent:

```
import React from 'react'
import {NavLink} from 'react-router-dom'

const NavBar = (props) => {
    const locationLinks = props.locations.map((location, key) => <NavLink className={'loc-link'} key={key} to={`/locations/${location.id}`}>{' '}{location.attributes.name}</NavLink>)
    return(
            <div className="navbar">
                <NavLink to='/'>Location Home</NavLink>
                {locationLinks}
            </div>
    )
}

export default NavBar 
```

The NavBar component contains <NavLinks/> which very conveniently keep track of whether the route to the NavLink is active.  This is handy because it allows you to write code in css to highlight the link that you  are currently using (so the user knows where they are on the the website). I found it best to use ```  exact to="url" ``` because the base URL in nested routes will be considered active as well without the "exact" added. 

I used rather simplistic code, but this included using NavLink's active class and I created dynamic nav links so the user can easily navigate to any location they created in the app. 

With the dynamic links I had an error in the NavBar that made it so these particular links weren't rendering. The error was:

```
VM231:1 Uncaught (in promise) SyntaxError: Unexpected token < in JSON at position 0
```

 It seemed to suggest that the error was occurring in App.js (the container component for NavBar), but alterations to the code in that file didn't seem to change anything. I eventually found that I was missing an the s in to='/locations/${location.id}' in locationLinks.
 
#  Destructuring
 
 Destructuring was a particularly fun challenge. I had decided to use my  rail json serializer structure as the redux state structure for my app so fetching from my own api was easy, what wasn't easy was parsing the data from an external api.  To handle this, i thought it best to destructure the data received from the fetch call and create an object that resembeled my already existing structure.
 
 The following is an example of an object returned from an unaltered fetch rsponse:
 
 ```
venue: {
    categories:[{
        icon: {
            prefix: "https://ss3.4sqi.net/img/categories_v2/food/caribbean_", 
            suffix: ".png"
        },
        id: "4bf58dd8d48988d144941735",
        name: "Caribbean Restaurant",
        pluralName: "Caribbean Restaurants",
        primary: true,
        shortName: "Caribbean",
        // __proto__: Object,
        // length: 1 
        },
        // {__proto__: []}
    ];
    id: "4b3ff7cff964a52034b325e3";
    location: {
        address: "3903 Branch Ave";
        cc: "US";
        city: "Temple Hills";
        country: "United States";
        formattedAddress: ["3903 Branch Ave", "Temple Hills, MD 20748", "United States"];
        labeledLatLngs: [
            // {…}, {…}
        ];
        lat: 38.836069805402474;
        lng: -76.9454409891463;
        postalCode: "20748";
        state: "MD"
        // __proto__: Object
    };
    name: "Tropicana Eateries"   
    photos: {
        // count: 0, groups: Array(0)
    }
    // __proto__: Object
}
 ```
 
 the format I wanted was 
 
 ```object: {id: id, attributes: {...atributes} } ```
 
 to destructure this in the format i desired I implented this code:
 
 ```
 items.map(thisPlace => {
        
                    const{venue:{
                        id, name, 
                        // categories: {icon: {prefix, suffix} ={prefix: 'undefined', suffix: 'undefined'},} = {icon: 'no icon'}, 
                        categories: [{icon: {prefix, suffix}}],
                        location: {address, city, state, postalCode, country} = {address: 'undefined'}
                        
                    }}= thisPlace

                    let iconUrl = prefix+'88'+suffix
                    // 88  is the size of the icon.  There are 4 sizes available -> 32, 44, 64, 88 
                    // see this link: https://stackoverflow.com/questions/24377797/get-the-icon-of-a-foursquare-category-from-its-id

                    let item ={id: id, attributes: {icon_url: iconUrl, name: name, street: address, city: city, state: state, zip: postalCode, country: country, location_id: locationId
										return item
}})
 ```
 
Particular challenges for this destructuring task was learning how to destructure deeply nested objects and utlizing mixed destructuring techniques to access each desired point.

For example:

To destructure 

```
venue: {
    categories: {
        icon: {
            prefix: "https://ss3.4sqi.net/img/categories_v2/food/caribbean_", 
            suffix: ".png"
        }
}
```

you need create nested brackets like so:

```
const{
categories: {icon: {previx, suffix},}
}=venue


```

and if that value is undefined (as was happening in my response as icon was within an array) you can replace a value like so:

```

categories: {icon: {prefix, suffix},} = {icon: 'no icon'}

```

For deeply nested mixed objects such this one:


```
venue: {
    categories:[{
        icon: {
            prefix: "https://ss3.4sqi.net/img/categories_v2/food/caribbean_", 
            suffix: ".png"
        }]
}
```

Mixed  destruction is needed like so:

```

const{
categories: [{icon: {prefix, suffix}}]

}
```


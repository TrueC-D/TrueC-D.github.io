---
layout: post
title:      "JavaScript; Dynamically Creating Filters"
date:       2021-03-29 05:19:35 +0000
permalink:  javascript_dynamically_creating_filters
---


The first major hurdle of my JavaScript journey seems to be encompassed by Flatiron's Dog CEO challenge. The first few challenges were pretty easy;  I had to pull images and dog names from two JSON documents on the web and have them render on the Direct Object Model (DOM) in their respective places. The places these elements had to be appended to were identified by IDs.  The third challenge in this lab was to add an event listener to the items rendered by javascript. This was a semi difficult learning curve as the way I initially wrote my code didn't allow for the initial javascript to populate before the event listener was called. The last challenge was to dynamically display dog names, filtering breed names by selecting the first letter in the drop down menu on the webpage. This dynamic filtering challenge proved to be my biggest hurdle.

When creating my code,  I tried to be creative in how I called fetch.  I wanted my code to be as DRY as possible so that rather than writing the code for fetch multiple times for each JSON document and calling fetch in my event listener, I called fetch in the functions that my event listener called. 

For Example: 

```
document.addEventListener('DOMContentLoaded', function(){renderDogImages(); renderDogBreeds();})

function fetchDogs(urlToUse,whatToReturn){
    fetch(urlToUse).then(
        function(response){
            return response.json()
        }).then(whatToReturn)
}

function renderDogImages(){
    const urlToUse = 'https://dog.ceo/api/breeds/image/random/4'
    const whatToReturn = function(json){
        for(const arrayElement of json.message){
            let image = document.createElement('img'); 
            image.setAttribute('src', arrayElement); 
            document.getElementById('dog-image-container').appendChild(image);
        }
    }
    fetchDogs(urlToUse, whatToReturn)
}

```


As a result, this made it so I had to be careful of where my functions were placed in relation to the fetch conditions so that any event listeners I created would try to attach themselves to elements after the fetched information was rendered rather than before ( I can't attach an event listener if the object doesn't exist yet).This was solved by putting event listeners in functions within the calleback function for .fetch() 's .then() method rather than adding the event listener outside of that.

The following function did not work when placed at the bottom of my code (outside of the functions):

```

for(const listItem of document.getElementsByTagName('li')) {listItem.addEventListener('click', colorChange)}

```

This did work:

```

function renderDogBreeds(){

    const urlToUse = 'https://dog.ceo/api/breeds/list/all

        const whatToReturn = function(json){
          
            for(const objectKey in json.message){
                // addDogBreed(objectKey)
                let dogBreed = document.createElement('li'); 
                dogBreed.innerText = objectKey;
                dogBreedUl.appendChild(dogBreed);
                dogBreed.addEventListener('click', colorChange)
            };

    fetchDogs(urlToUse, whatToReturn)
}
```

To dynamically populate dog breeds for the last challenge, what proved to be the hardest to figure out was how to filter through a collection (like an array, but not the same), and how to remove all the list items.

This was complicated because when grabbing the elements in the DOM, since a collection which is not technically an array, .map and .filter did not work.

The key to both removing and filtering the elements was to store the elements elsewhere in a way that they could still retrieved and modified. 

```
const lis = document.getElementsByTagName('li');
const allDogs = Object.values(lis);
```

The above constants allowed me to do just that.  By storing the values of the object rather than just the text, I was able to remove and and add back in list Items to the DOM, except that I had to use this code in two different places.

I called the allDogs constant that is shown above when I wanted to filter through what dogs to add to the page. Because the the list I needed to filter through didn't change as it needed to represent  a master list, it didn't matter if the stored collection didn't match the collection displayed on the DOM as long as I was able to render the information I needed from it.  This wasn't the case when removing the current list items on the DOM.  What was displayed in the DOM changes every time a new selection on the dropdown menu is made.  To remove the collection on displayed on the DOM I needed to redefine "allDogs" each time then call it.  To solve for this I created a duplicate constant called "currentList" which was displayed in the "removeDogBreeds" function and would be rewritten every time the list needed to be cleared.


```
   function removeDogBreeds(){
                let currentList = Object.values(document.getElementsByTagName('li'))
                currentList.map(dog => dog.remove())
            };
```

My final code for renderDogBreeds() ended up looking like this:


```
function colorChange(event) {
    event.target.style.color = '#7B68EE'
}


function renderDogBreeds(){
  

    const dogBreedUl = document.getElementById('dog-breeds')

    const urlToUse = 'https://dog.ceo/api/breeds/list/all'


        const whatToReturn = function(json){
            const breedDropdown = document.getElementById('breed-dropdown');
            for(const objectKey in json.message){
                let dogBreed = document.createElement('li'); 
                dogBreed.innerText = objectKey;
                dogBreedUl.appendChild(dogBreed);
                dogBreed.addEventListener('click', colorChange)
            };

            function removeDogBreeds(){
                let currentList = Object.values(document.getElementsByTagName('li'))
                currentList.map(dog => dog.remove()
            };
            
            const lis = document.getElementsByTagName('li');
            const allDogs = Object.values(lis);
            breedDropdown.onchange = function(){
                removeDogBreeds();
                const filteredByLetter = allDogs.filter(item => item.innerText.startsWith(breedDropdown.value));
                for(const dog of filteredByLetter){
                    let dogBreed = document.createElement('li'); 
                    dogBreed.innerText = dog.innerText;
                    dogBreedUl.appendChild(dogBreed);
                    dogBreed.addEventListener('click', colorChange);
                }
            }
        }
    fetchDogs(urlToUse, whatToReturn)
}

```

---
layout: post
title:  "Medical Cabinet"
date:   2020-06-06 14:34:25
categories: machine-learning medical
tags: MachineLearning Medical
image: assets/article_images/2020-06-06-Medcabinet/medcab.jpg
image2: /assets/article_images/2014-11-30-mediator_features/night-track-mobile.JPG


---
#Mediator Medcabinet Project

Examples for different formats and css features

#Header Formats
#Header1
##Header2

#Blockquotes
>Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus

#Lists
##orderd lists
1. one
2. two
3. three

##unorderd lists
- Apple
- Banana
- Plum

#Links
This is an [example link](http://example.com/ "With a Title").

#Combinations
>Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus
>
> - Apple
> - Banana
> - Plum

# Med Cabinet
This project was made in order to recommend the closest Cannabis product desirable to the users' taste. This was done on a cross-functional team.
our role as Data Engineers was to provide 2 API routes for the Web Developement team.
One endpoint to show user all of the Cannabis strains and their tastes and effects.
After user chooses a strain, that would be sent to use using another endpoint, we would then use the ML model that was provided to us (pickled) from the ML team, and feed the user inputs to it, then return the result back to the Web Developement team.
The data was all scraped from weed Weedmaps using their [API](https://developer.weedmaps.com/)
These are the endpoint:

# APIs:
Base URL is https://weed-data-bw.herokuapp.com

## POST [/model](https://weed-data-bw.herokuapp.com/model)

### Request:
JSON object in the following format:

```js
{
    "Flavors": ["Blueberry", "Apple", "Skunk"],
    "Effects": ["Focused", "Happy", "Relaxed"]
}
```
It has to have 2 keys "Flavors" and "Effects" in it. Each of their values are an array which contain at most 3 strings.

### Response:
JSON: list of dictionaries, each with 4 keys:</br>
{
    "Name" -> string,
    "Description" -> string,
    "Flavors" -> list of 3 strings,
    "Effects" -> list of 3 strings
]
```js
[
    {
        "Name": "Agent Tangie",
        "Description": "Agent Tangie is a ...",
        "Flavors": [
            "Cirtus",
            "Diesel",
            "Earthy"
        ],
        "Effects": [
            "Energetic",
            "Focused",
            "Giggly"
        ]
    },
    {
        "Name":...,
        ...
    },
    ...
]
```
<hr>

## GET [/strains](https://weed-data-bw.herokuapp.com/strains)

### Request:
Request's body is empty

### Response:
JSON: list of a dictionaries which each contain 59 keys ("name", "description", <57 one-hot-encoded features>) there also is an endpoint for a not one-hot-encoded version ([/web_layout_strains](https://weed-data-bw.herokuapp.com/web_layout_strains)):</br>
```js
[
    {
        "name": "24K Gold",
        "description": "24K Gold, also known as...",
        "Ammonia": 0,
        "Apple": 0,
        "Apricot": 0,
        "Berry": 1,
        "Blue Cheese": 0,
        <'other 52 one-hot-encoded features'>
    },
    {
        "name": ...,
        ...
    },
    ...
]
```

<hr>

## GET [/web_layout_strains](https://weed-data-bw.herokuapp.com/web_layout_strains)

### Request:
Request's body is empty

### Response:
JSON: list of a dictionaries which each contain 4 keys ("name", "description", "flavors", "effects") there also is an endpoint for a one-hot-encoded version ([/strains](https://weed-data-bw.herokuapp.com/strains)):</br>
```js
[
    {
        "name": "24K Gold",
        "description": "24K Gold, also known as...",
        "flavors": [
            "Blueberry",
            "Mango",
            "Spicy/Herbal"
        ],
        "effects": [
            "Creative",
            "Giggly",
            "Relaxed"
        ]
    },
    {
        "name": ...,
        ...
    },
    ...
]
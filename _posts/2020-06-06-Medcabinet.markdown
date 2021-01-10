---
layout: post
title:  "Medical Cabinet"
date:   2020-06-06 14:34:25
categories: machine-learning medical
tags: MachineLearning Medical
image: /assets/article_images/2020-06-06-Medcabinet/medcab.jpg
image2: /assets/article_images/2014-11-30-mediator_features/night-track-mobile.JPG
ThumbnailVideo: https://file-examples-com.github.io/uploads/2017/04/file_example_MP4_480_1_5MG.mp4

---
# Med Cabinet
This project was made in order to recommend the closest Cannabis product desirable to the users' taste. This was done on a cross-functional team.
our role as Data Engineers was to provide 2 API routes for the Web Developement team.
# How it works
First, in order to create the model, we scraped the data from [weedmaps](https://weedmaps.com/).
The data was all scraped from weed Weedmaps using their [developer API](https://developer.weedmaps.com/).

This is what the final cleaned table looks like

| name                  	| 24K Gold, also...     	| Blueberry 	| Cheese 	| Chestnut 	|
|-----------------------	|-----------------------	|-----------	|--------	|----------	|
| 24K Gold              	| 303 OG Kush is...     	| 1         	| 0      	| 0        	|
| 303 OG Kush           	| While the original... 	| 0         	| 0      	| 1        	|
| 3 Kings (Three Kings) 	| 3X Crazy, also...     	| 1         	| 1      	| 0        	|

You can take a look at the complete scraped data in [this link](https://github.com/Build-Week-Med-Cabinet-2-MP/bw-med-cabinet-2-ml/blob/master/data/CLEAN_WMS_2020_05_24.csv)

Then we needed to create a model to recommend 

One endpoint to show user all of the Cannabis strains and their tastes and effects.
After user chooses a strain, that would be sent to use using another endpoint, we would then use the ML model that was provided to us (pickled) from the ML team, and feed the user inputs to it, then return the result back to the Web Developement team.

These are the endpoint:

# APIs:
Base URL is https://weed-data-bw.herokuapp.com(https://weed-data-bw.herokuapp.com) which there you can see github icon as a link, directing to our complete repo of the Front-End, Back-end,
marketing and Data Engineering repos.
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
```
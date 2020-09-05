---
title: "COVID-19 Train Escort Management App for the Indian Railways"
date: 2020-5-27
categories:
- Project
tags:
- Flask
- COVID-19
cover: '/images/traincheck.png'
thumbnail: '/images/traincheck.png'
---

My dad works for the Indian Railways, and when special trains started running to transport migrant labourers during India's response to the COVID-19, he discussed the logistical issue of managing police escorts on these trains and coordinating with different government agencies. 

<!--more-->

I designed a basic application to help him out with the usecases he mentioned and and coded it overnight in a 4 hour sprint. Admittedly, not very polished or tight on security, the web-app delivered on what was required. He presented it to his office, and within hours of coding the web-app, it was deployed online and used by thousands of Indian Railway employees all over the country.

Ever since the COVID-19 pandemic began, I've been actively looking for ways to contribute to the fight against it. Before working on this web-app, I contributed to a project by PHFI to predict the number of hospital beds required in different districts based on current patient numbers and trends in different countries. This web-app though has had an admittedly more direct impact.

You can find the web-app deployed here : [traincheck.pythonanywhere.com](http://traincheck.pythonanywhere.com)

The frontend is vanilla JS with HTML and CSS, while the backend is a simple Flask server with an SQLite database.

The code for the project is open sourced and available [here, on GitHub](https://github.com/kaushalvivek/train-escort-app).
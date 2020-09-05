---
title: "Containment Zone Management and Prediction - South Delhi"
date: 2020-6-27
categories:
- Project
tags:
- Freelance
- Flask
- COVID-19
cover: '/images/cont1.png'
thumbnail: '/images/cont3.png'
---

Around a couple of weeks ago, I found out that the district administration of South Delhi, like governments all over the world, are struggling to find a strong technical solution to contain the COVID-19 pandemic. Especially in Delhi, with cases surging, it was becoming increasingly difficult to create containment zones manually by looking at addresses of patients when thousands of cases are coming in everyday.

I came up with a better way of doing that, and with the permission of the District Magistrate of South Delhi, coded and deployed my solution built on their database. The solution was well received by those in charge of containment zones and is now live.

In this article, I'll briefly explain my approach.

<!--more-->

## The Problem

![image](/images/cont2.png)

With the volume at which cases were, and still are, coming in, it's becoming virtually impossible to create containment zones by looking at addresses alone. What's needed is a technological solution that suggests containment zone on it's own, based on pre-defined parameters.

## The Solution

![image](/images/cont3.png)

I designed an approach using Google Maps' geocoding API and k-means clustering that largely does the following:
- it looks at all the confirmed cases from the last 7 days -- scraping this data from a live Google Sheet
- converts all the addresses into geocodes -- pairs of longitude and latitude -- using Google Maps' geocodiing API
- plots these cases on a map -- using Google Maps' dynamic JS map API
- uses k-means clustering to create n clusters among these map plots -- the value n is decided on the basis of maximization of the number of reported clusters that meet containment criteria
- a suggested containment zone is defined as a region where 3 or more than 3 cases are in a region such that they are less than 100m away from each other
- the proposed clusters are termed 'suggested containment zones' and plotted on a map, again using Google Maps' dynamic JS map API
- in addition to this, using a sheet of existing containment and mini-containment zones, features hae been added to plot and post review-reminders for the said zones.
- the application is built in python, and uses flask to serve web-pages
- the full app has been hosted on pythonanywhere -- a great platform for python-based app deployment that allows for custom domains and cron-jobs/script scheduing out-of-box in its free tier.

## Resources used:
- [Google Maps API](https://developers.google.com/maps/documentation)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [```gspread``` - Sheets API Python Wrapper](https://github.com/burnash/gspread)
- [```googlemaps``` - Maps API Python Wrapper](https://github.com/googlemaps/google-maps-services-python)
- [PythonAnywhere - free web hosting](http://pythonanywhere.com/)

The codebase is currently proprietary, it would be released under an open-source license once the deployment contract expires.
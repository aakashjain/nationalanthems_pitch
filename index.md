---
title       : National Anthems
subtitle    : Music Artist Popularity by Country
author      : Aakash Jain
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## What it does

National Anthems is a webapp that shows you the most popular music artists in
a country, giving you a glimpse into regional trends.

It uses data from Last.FM, a widely used social network for music 
lovers that also keeps track of what its users listen to.

It is made in R with the Shiny framework, as part of a project for the
Developing Data Products course on Coursera.

---

## Scraping the Last.FM API

Last.FM exposes records through a web API that can be very simply scraped.
For example, to get data for the top 5 artists among US users:


```r
require(httr)
require(knitr)
request <- POST('http://ws.audioscrobbler.com/2.0/',
              body=list(method='geo.getTopArtists',
              api_key='4d1e099188a7028fad88d7bd0ac7353d',
              country='United States', limit=5, format='json'),
              encode='form')
stop_for_status(request)
data <- content(request)
```

---

Details for the top artist:


```r
data$topartists$artist[[1]]$name
```

```
## [1] "Kanye West"
```

```r
data$topartists$artist[[1]]$listeners
```

```
## [1] "9453"
```

After some reshaping, the rCharts package plots the data:



```r
require(rCharts)
p <- dPlot(y='Artist', x='Listeners', groups='Artist',
              data=data, type='bar')
p$xAxis(type='addMeasureAxis', outputFormat='#,')
p$yAxis(type='addCategoryAxis')
```

---

If the plot does not appear, please refresh the page. This is a problem with the way dimple plots are rendered.

<iframe src=' assets/fig/unnamed-chunk-5.html ' scrolling='no' frameBorder='0' seamless class='rChart dimple ' id=iframe- chart3f913bd6c8af ></iframe> <style>iframe.rChart{ width: 100%; height: 400px;}</style>

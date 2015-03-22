---
title       : CapiTool
subtitle    : 
author      : WillahScott
job         : WiDo Stuff
framework   : revealjs      # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : default       # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

<style>
    .reveal h2 {
        color: orange;
        text-align: center;
        padding-bottom: 15px;
        font-family: Tahoma, sans-serif;
        font-style: italic
    }
    .reveal h3 {
        color: lightblue;
        text-align: center;
        padding-bottom: 25px;
        font-family: Tahoma, sans-serif;
        font-style: normal
    }
    .extraSpace {
        padding-top: 25px !important;        
    } 
    .author {
        padding-top: 70px !important;
        color: gray;
        text-align: right;
        font-family: Tahoma, sans-serif;
        font-variant: small-caps;
        font-style: italic;
    }
    .author2 {
        padding-top: 10px !important;
        color: gray;
        text-align: right;
        font-family: Tahoma, sans-serif;
        font-variant: small-caps;
        font-style: italic;
        font-size: 0.8em !important;
    }
    .smallText {
        font-family: Tahoma, sans-serif;
        font-size: 0.6em !important;
        text-align: left;
    }
    .smallText2 {
        padding-top: 15px !important;        
        font-family: Tahoma, sans-serif;
        font-size: 0.6em !important;
        text-align: left;
    }
    .smallNote {
        padding-top: 20px !important;
        color: grey;
        text-align: left;
        font-family: Tahoma, sans-serif;
        font-size: 0.6em !important;
    }
    ul.in {
        list-style-type: square;
        font-size: 0.7em !important;
        text-align: left;
    }
    ul.smallIn {
        list-style-type: square;
        font-size: 0.65em !important;
    }
</style>

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

<h2>CapiTool</h2>
  
Capital under stress scenarios projection tool

<p class="author">by <span style="color:white; font-weight:bold">WillahScott</span> @ WiDo Stuff</p>

---

### *CapiTool* Overview

<p style="color: orange; font-style: italic; font-size: 1em">Regulatory capital requirements (for financial institutions) simulation under different user-defined macoeconomic scenarios</p>

<a href="http://willahscott.shinyapps.io/CapiToolApp" target="_blank">
    <img src="img/CapiTool.gif" alt="CapiTool Overview" style="width:600px; height:400px">
</a>

<p class="smallNote">The app is available through shinyapps.io. Click image to redirect.</p>

--- 

<h3><i>CapiTool</i> Usage</h3>

<h4>Inputs</h4>

<ul class="in">
    <li><span style="color: grey; font-weight:bold">Macroeconomic variables</span></li>
    <ul class="smallIn">
        <li><span style="color: grey; font-weight:bold">Variables:</span> GDP - Unemployment - Consumer Price Index - House Prices - Rates</li>
        <li><span style="color: grey; font-weight:bold">Scenarios:</span> <span style="color:green">Base</span> - <span style="color:orange">Adverse</span> - <span style="color:red">Severely Adverse</span> - <span style="color:darkred">Extreme Recession</span></li>
    </ul>
    <li><span style="color: grey; font-weight:bold">Projection Window:</span> 2015 - 2016 - 2017</li>
    <li><span style="color: grey; font-weight:bold">Product Breakdown:</span> Mortgage - Consumer Loan - Credit Card - Lease </li>
</ul>

<h4 class="extraSpace">Outputs</h4>
<ul class="in">
    <li>Total projected capital requirements</li>
    <li>Projected capital requirements by product</li>
</ul>

<p class="smallNote">Basic usage information may be found under the <i>About</i> tab in the application.</p>

<script>
$('h4').addClass('fragment')
$('ul.in').addClass('fragment')
$('ul.smallIn').addClass('fragment')
$('p.smallNote').addClass('fragment')
</script>

---

### Stress Testing Regulation

<p class="smallText">The <span style="color: grey; font-weight: bold">Capital Requirements Directive (CRD IV)</span> establishes the amount of capital a bank must have allocated, given as a set of functions of the estimated probability of default (for performing products), severity and exposure, one function for each different product.</p>

<p class="smallText2">The Stress Test exercise regulation put forth by the EBA requires a <span style="color: grey; font-weight: bold">3 year projection</span> (or US Stress Test, CCAR establishes 9 quarters projection) of regulatory capital. <span style="color: orange; font-variant: small-caps">CapiTool</span> allows up to 3 year projection.</p>

<p class="smallText2">The (European) Stress Test exercise <span style="color: grey; font-weight: bold">requires two scenarios</span> set forth by the EBA, as well as any scenarios required by the national regulators. For the US CCAR exercise, 3 exercises are necessary, as well as two internal scenarios which the institution has to create.</p>

<p class="smallText2"><span style="color: orange; font-variant: small-caps">CapiTool</span> stresses the different factors (mainly probability of default and severity) and simulates the projected capital requirements under the scenario defined by the user.</p>


---

### R Implementation

<p class="smallText">The app is implemented in R through RStudio's shiny package, which needs a <span style="font-family: "Courier New", monospace; color: grey">ui.R</span> and <span style="font-family: "Courier New", monospace; color: grey">server.R</span>. Additionally all further calculations take place in the <span style="font-family: "Courier New", monospace; color: grey">simulation.R</span>.</p>

<h4 class="extraSpace">Data</h4>

<p class="smallText"><span style="color: orange; font-variant: small-caps">CapiTool</span> uses two loan databases:</p>

<p style="color: grey; font-family: Tahoma, sans-serif; font-size: 0.8em">Performing portfolio</p>
<pre class="prettyprint lang-r">
    source("simulation.R")
    table(performing.df$Products)
</pre>

```
## 
## Consumer Loan   Credit Card         Lease      Mortgage 
##           322           665           176           395
```

<p style="color: grey; font-family: Tahoma, sans-serif; font-size: 0.8em">Defaulted portfolio</p>
<pre class="prettyprint lang-r">
    table(defaulted.df$Products)
</pre>

```
## 
## Consumer Loan   Credit Card         Lease      Mortgage 
##           310            31           171           157
```

<p class="author2">by <span style="color:white; font-weight:bold">WillahScott</span> @ WiDo Stuff</p>

<script>
$('p.author2').addClass('fragment')
</script>

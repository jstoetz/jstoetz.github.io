---
layout: collection
title:  "Setting Up Your R Environment"
---

## Setup R and R Studio

## Setup Git and github

## Building Your First R File

### 1.1 Install Required Packages

The formula below checks to see if the required packages are and then installs

```
p.packages <- c(‘rvest’,‘xml2’)

lapply(p.packages, function(x)(
		if (!( x %in%  installed.packages())) { 
		install.packages(x)
		}
	)
)

lapply(p.packages, function(x)(
		library(x)
	)
)
```

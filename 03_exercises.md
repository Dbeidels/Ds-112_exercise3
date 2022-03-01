---
title: 'Weekly Exercises #3'
author: "Daniel Beidelschies"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday dog breed data
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')

# Tidy Tuesday data for challenge problem
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>% 
  group_by(vegetable,wday(date)) %>% 
  summarize(wt_lbs = sum(weight*0.00220462))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["wday(date)"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["wt_lbs"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"7","3":"0.34392072"},{"1":"asparagus","2":"7","3":"0.04409240"},{"1":"basil","2":"2","3":"0.06613860"},{"1":"basil","2":"3","3":"0.11023100"},{"1":"basil","2":"5","3":"0.02645544"},{"1":"basil","2":"6","3":"0.46737944"},{"1":"basil","2":"7","3":"0.41005932"},{"1":"beans","2":"1","3":"1.91361016"},{"1":"beans","2":"2","3":"6.50803824"},{"1":"beans","2":"3","3":"4.38719380"},{"1":"beans","2":"4","3":"4.08295624"},{"1":"beans","2":"5","3":"3.39291018"},{"1":"beans","2":"6","3":"1.52559704"},{"1":"beans","2":"7","3":"4.70906832"},{"1":"beets","2":"1","3":"0.32187452"},{"1":"beets","2":"2","3":"0.67240910"},{"1":"beets","2":"3","3":"0.15873264"},{"1":"beets","2":"4","3":"0.18298346"},{"1":"beets","2":"5","3":"11.89172028"},{"1":"beets","2":"6","3":"0.02425082"},{"1":"beets","2":"7","3":"0.37919464"},{"1":"broccoli","2":"1","3":"1.25883802"},{"1":"broccoli","2":"2","3":"0.82011864"},{"1":"broccoli","2":"4","3":"0.70768302"},{"1":"broccoli","2":"6","3":"0.16534650"},{"1":"carrots","2":"1","3":"2.93655384"},{"1":"carrots","2":"2","3":"0.87082490"},{"1":"carrots","2":"3","3":"0.35273920"},{"1":"carrots","2":"4","3":"5.56225626"},{"1":"carrots","2":"5","3":"2.67420406"},{"1":"carrots","2":"6","3":"2.13848140"},{"1":"carrots","2":"7","3":"2.33028334"},{"1":"chives","2":"4","3":"0.01763696"},{"1":"cilantro","2":"3","3":"0.00440924"},{"1":"cilantro","2":"6","3":"0.07275246"},{"1":"cilantro","2":"7","3":"0.03747854"},{"1":"corn","2":"1","3":"1.45725382"},{"1":"corn","2":"2","3":"0.75838928"},{"1":"corn","2":"3","3":"0.72752460"},{"1":"corn","2":"4","3":"5.30211110"},{"1":"corn","2":"6","3":"3.44802568"},{"1":"corn","2":"7","3":"1.31615814"},{"1":"cucumbers","2":"1","3":"3.10410496"},{"1":"cucumbers","2":"2","3":"4.77520692"},{"1":"cucumbers","2":"3","3":"10.04645334"},{"1":"cucumbers","2":"4","3":"5.30652034"},{"1":"cucumbers","2":"5","3":"3.30693000"},{"1":"cucumbers","2":"6","3":"7.42956940"},{"1":"cucumbers","2":"7","3":"9.64080326"},{"1":"edamame","2":"3","3":"1.40213832"},{"1":"edamame","2":"7","3":"4.68922674"},{"1":"hot peppers","2":"2","3":"1.25883802"},{"1":"hot peppers","2":"3","3":"0.14109568"},{"1":"hot peppers","2":"4","3":"0.06834322"},{"1":"jalapeño","2":"1","3":"0.26234978"},{"1":"jalapeño","2":"2","3":"5.55343778"},{"1":"jalapeño","2":"3","3":"0.54895038"},{"1":"jalapeño","2":"4","3":"0.48060716"},{"1":"jalapeño","2":"5","3":"0.22487124"},{"1":"jalapeño","2":"6","3":"1.29411194"},{"1":"jalapeño","2":"7","3":"1.50796008"},{"1":"kale","2":"1","3":"0.82673250"},{"1":"kale","2":"2","3":"2.06793356"},{"1":"kale","2":"3","3":"0.28219136"},{"1":"kale","2":"4","3":"0.61729360"},{"1":"kale","2":"5","3":"0.27998674"},{"1":"kale","2":"6","3":"0.38139926"},{"1":"kale","2":"7","3":"1.49032312"},{"1":"kohlrabi","2":"5","3":"0.42108242"},{"1":"lettuce","2":"1","3":"1.46607230"},{"1":"lettuce","2":"2","3":"2.45815130"},{"1":"lettuce","2":"3","3":"0.91712192"},{"1":"lettuce","2":"4","3":"1.18608556"},{"1":"lettuce","2":"5","3":"2.45153744"},{"1":"lettuce","2":"6","3":"1.80117454"},{"1":"lettuce","2":"7","3":"1.31615814"},{"1":"onions","2":"1","3":"0.26014516"},{"1":"onions","2":"2","3":"0.50926722"},{"1":"onions","2":"3","3":"0.70768302"},{"1":"onions","2":"5","3":"0.60186126"},{"1":"onions","2":"6","3":"0.07275246"},{"1":"onions","2":"7","3":"1.91361016"},{"1":"peas","2":"1","3":"2.05691046"},{"1":"peas","2":"2","3":"4.63411124"},{"1":"peas","2":"3","3":"2.06793356"},{"1":"peas","2":"4","3":"1.08026380"},{"1":"peas","2":"5","3":"3.39731942"},{"1":"peas","2":"6","3":"0.93696350"},{"1":"peas","2":"7","3":"2.85277828"},{"1":"peppers","2":"1","3":"0.50265336"},{"1":"peppers","2":"2","3":"2.52649452"},{"1":"peppers","2":"3","3":"1.44402610"},{"1":"peppers","2":"4","3":"2.44271896"},{"1":"peppers","2":"5","3":"0.70988764"},{"1":"peppers","2":"6","3":"0.33510224"},{"1":"peppers","2":"7","3":"1.38229674"},{"1":"potatoes","2":"2","3":"0.97003280"},{"1":"potatoes","2":"4","3":"4.57017726"},{"1":"potatoes","2":"5","3":"11.85203712"},{"1":"potatoes","2":"6","3":"3.74124014"},{"1":"potatoes","2":"7","3":"2.80207202"},{"1":"pumpkins","2":"2","3":"30.11951844"},{"1":"pumpkins","2":"3","3":"31.85675900"},{"1":"pumpkins","2":"7","3":"92.68883866"},{"1":"radish","2":"1","3":"0.08157094"},{"1":"radish","2":"2","3":"0.19621118"},{"1":"radish","2":"3","3":"0.09479866"},{"1":"radish","2":"5","3":"0.14770954"},{"1":"radish","2":"6","3":"0.19400656"},{"1":"radish","2":"7","3":"0.23148510"},{"1":"raspberries","2":"2","3":"0.13007258"},{"1":"raspberries","2":"3","3":"0.33510224"},{"1":"raspberries","2":"5","3":"0.28880522"},{"1":"raspberries","2":"6","3":"0.57099658"},{"1":"raspberries","2":"7","3":"0.53351804"},{"1":"rutabaga","2":"1","3":"19.26396956"},{"1":"rutabaga","2":"6","3":"3.57809826"},{"1":"rutabaga","2":"7","3":"6.89825598"},{"1":"spinach","2":"1","3":"0.48722102"},{"1":"spinach","2":"2","3":"0.14770954"},{"1":"spinach","2":"3","3":"0.49603950"},{"1":"spinach","2":"4","3":"0.21384814"},{"1":"spinach","2":"5","3":"0.23368972"},{"1":"spinach","2":"6","3":"0.19621118"},{"1":"spinach","2":"7","3":"0.26014516"},{"1":"squash","2":"2","3":"24.33459556"},{"1":"squash","2":"3","3":"18.46810174"},{"1":"squash","2":"7","3":"56.22221924"},{"1":"strawberries","2":"1","3":"0.08157094"},{"1":"strawberries","2":"2","3":"0.47840254"},{"1":"strawberries","2":"5","3":"0.08818480"},{"1":"strawberries","2":"6","3":"0.48722102"},{"1":"strawberries","2":"7","3":"0.16975574"},{"1":"Swiss chard","2":"1","3":"1.24781492"},{"1":"Swiss chard","2":"2","3":"1.07364994"},{"1":"Swiss chard","2":"3","3":"0.07054784"},{"1":"Swiss chard","2":"4","3":"0.90830344"},{"1":"Swiss chard","2":"5","3":"2.23107544"},{"1":"Swiss chard","2":"6","3":"0.61729360"},{"1":"Swiss chard","2":"7","3":"0.73413846"},{"1":"tomatoes","2":"1","3":"75.60964752"},{"1":"tomatoes","2":"2","3":"11.49268406"},{"1":"tomatoes","2":"3","3":"48.75076206"},{"1":"tomatoes","2":"4","3":"58.26590198"},{"1":"tomatoes","2":"5","3":"34.51773534"},{"1":"tomatoes","2":"6","3":"85.07628580"},{"1":"tomatoes","2":"7","3":"35.12621046"},{"1":"zucchini","2":"1","3":"12.23564100"},{"1":"zucchini","2":"2","3":"12.19595784"},{"1":"zucchini","2":"3","3":"16.46851140"},{"1":"zucchini","2":"4","3":"2.04147812"},{"1":"zucchini","2":"5","3":"34.63017096"},{"1":"zucchini","2":"6","3":"18.72163304"},{"1":"zucchini","2":"7","3":"3.41495638"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  group_by(variety) %>% 
  summarize(total_weight_lbs = sum(weight*0.00220462)) %>% 
  left_join(garden_planting,by = "variety")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]},{"label":["total_weight_lbs"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[3],"type":["chr"],"align":["left"]},{"label":["vegetable"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"Amish Paste","2":"65.67342518","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Amish Paste","2":"65.67342518","3":"N","4":"tomatoes","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Better Boy","2":"34.00846812","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Better Boy","2":"34.00846812","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Big Beef","2":"24.99377694","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Black Krim","2":"15.80712540","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Blue (saved)","2":"41.52401770","3":"A","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Blue (saved)","2":"41.52401770","3":"B","4":"squash","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Bolero","2":"8.29157582","3":"H","4":"carrots","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Bolero","2":"8.29157582","3":"L","4":"carrots","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Bonny Best","2":"24.92322910","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Brandywine","2":"15.64618814","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Bush Bush Slender","2":"22.12997556","3":"M","4":"beans","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Bush Bush Slender","2":"22.12997556","3":"D","4":"beans","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"Catalina","2":"2.03486426","3":"H","4":"spinach","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Catalina","2":"2.03486426","3":"E","4":"spinach","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Cherokee Purple","2":"15.71232674","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Chinese Red Noodle","2":"0.78484472","3":"K","4":"beans","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"Chinese Red Noodle","2":"0.78484472","3":"L","4":"beans","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"potD","4":"cilantro","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"E","4":"cilantro","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Cinderella's Carraige","2":"32.87308882","3":"B","4":"pumpkins","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Classic Slenderette","2":"3.60455370","3":"E","4":"beans","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"Crispy Colors Duo","2":"0.42108242","3":"front","4":"kohlrabi","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"delicata","2":"10.49840044","3":"K","4":"squash","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"Delicious Duo","2":"0.75398004","3":"P","4":"onions","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"Dorinny Sweet","2":"11.40670388","3":"A","4":"corn","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Dragon","2":"4.10500244","3":"H","4":"carrots","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Dragon","2":"4.10500244","3":"L","4":"carrots","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"6.09136506","3":"O","4":"edamame","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Farmer's Market Blend","2":"3.80296950","3":"C","4":"lettuce","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Farmer's Market Blend","2":"3.80296950","3":"L","4":"lettuce","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Garden Party Mix","2":"0.94578198","3":"C","4":"radish","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Garden Party Mix","2":"0.94578198","3":"G","4":"radish","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Garden Party Mix","2":"0.94578198","3":"H","4":"radish","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"giant","2":"9.87228836","3":"L","4":"jalapeño","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"Golden Bantam","2":"1.60275874","3":"B","4":"corn","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Gourmet Golden","2":"7.02171470","3":"H","4":"beets","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"grape","2":"32.39468628","3":"O","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"green","2":"5.69232884","3":"K","4":"peppers","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"green","2":"5.69232884","3":"O","4":"peppers","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"greens","2":"0.37258078","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Heirloom Lacinto","2":"5.94586014","3":"P","4":"kale","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Heirloom Lacinto","2":"5.94586014","3":"front","4":"kale","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Improved Helenor","2":"29.74032380","3":"E","4":"rudabaga","5":"30","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Isle of Naxos","2":"1.08026380","3":"potB","4":"basil","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Jet Star","2":"15.02448530","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"King Midas","2":"4.09618396","3":"H","4":"carrots","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"King Midas","2":"4.09618396","3":"L","4":"carrots","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"leaves","2":"0.22266662","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Lettuce Mixture","2":"4.74875148","3":"G","4":"lettuce","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Long Keeping Rainbow","2":"3.31133924","3":"H","4":"onions","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"Magnolia Blossom","2":"7.45822946","3":"B","4":"peas","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"Main Crop Bravado","2":"2.13186754","3":"D","4":"broccoli","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"Main Crop Bravado","2":"2.13186754","3":"I","4":"broccoli","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"Mortgage Lifter","2":"26.32536742","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"Mortgage Lifter","2":"26.32536742","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"mustard greens","2":"0.05070626","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Neon Glow","2":"6.88282364","3":"M","4":"Swiss chard","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"New England Sugar","2":"44.85960776","3":"K","4":"pumpkins","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"Old German","2":"26.71778978","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"perrenial","2":"3.18126666","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"pickling","2":"43.60958822","3":"L","4":"cucumbers","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"purple","2":"3.00930630","3":"D","4":"potatoes","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"red","2":"4.43349082","3":"I","4":"potatoes","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"Red Kuri","2":"22.73183682","3":"A","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Red Kuri","2":"22.73183682","3":"B","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Red Kuri","2":"22.73183682","3":"side","4":"squash","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"reseed","2":"0.09920790","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Romanesco","2":"99.70834874","3":"D","4":"zucchini","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"Russet","2":"9.09185288","3":"D","4":"potatoes","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"saved","2":"76.93241952","3":"B","4":"pumpkins","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Super Sugar Snap","2":"9.56805080","3":"A","4":"peas","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"Sweet Merlin","2":"6.38678414","3":"H","4":"beets","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Tatsoi","2":"2.89466606","3":"P","4":"lettuce","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"thai","2":"0.14770954","3":"potB","4":"hot peppers","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"unknown","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"variety","2":"4.97141810","3":"potA","4":"peppers","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"variety","2":"4.97141810","3":"potA","4":"peppers","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"variety","2":"4.97141810","3":"potC","4":"hot peppers","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"variety","2":"4.97141810","3":"potD","4":"peppers","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"N","4":"tomatoes","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"J","4":"tomatoes","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"front","4":"tomatoes","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"O","4":"tomatoes","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"Waltham Butternut","2":"24.27066158","3":"A","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Waltham Butternut","2":"24.27066158","3":"K","4":"squash","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"yellow","2":"7.40090934","3":"I","4":"potatoes","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"yellow","2":"7.40090934","3":"I","4":"potatoes","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"Yod Fah","2":"0.82011864","3":"P","4":"broccoli","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

* Some of the varieties of different plots have the same weight. We could fix this by trying a different type of join.

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
* We could assume price in garden_spending is price per pound and and join the two charts by vegetable and variety and make a new variable the mutliplies the price by the total weight to see how much you have saved.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(variety, date) %>% 
  summarize(total_weight_lbs = sum(weight*0.00220462)) %>% 
  arrange(total_weight_lbs, date) %>% 
  ggplot(aes(total_weight_lbs, fct_reorder(variety,total_weight_lbs)))+
  geom_col()
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(variety = str_to_lower(variety), length_variety_name = str_length(variety)) %>%  
  distinct(vegetable,variety,length_variety_name)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["length_variety_name"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"lettuce","2":"reseed","3":"6"},{"1":"radish","2":"garden party mix","3":"16"},{"1":"lettuce","2":"farmer's market blend","3":"21"},{"1":"spinach","2":"catalina","3":"8"},{"1":"beets","2":"leaves","3":"6"},{"1":"kale","2":"heirloom lacinto","3":"16"},{"1":"peas","2":"magnolia blossom","3":"16"},{"1":"peas","2":"super sugar snap","3":"16"},{"1":"chives","2":"perrenial","3":"9"},{"1":"strawberries","2":"perrenial","3":"9"},{"1":"lettuce","2":"tatsoi","3":"6"},{"1":"asparagus","2":"asparagus","3":"9"},{"1":"Swiss chard","2":"neon glow","3":"9"},{"1":"cilantro","2":"cilantro","3":"8"},{"1":"basil","2":"isle of naxos","3":"13"},{"1":"lettuce","2":"mustard greens","3":"14"},{"1":"raspberries","2":"perrenial","3":"9"},{"1":"zucchini","2":"romanesco","3":"9"},{"1":"beans","2":"bush bush slender","3":"17"},{"1":"beets","2":"gourmet golden","3":"14"},{"1":"beets","2":"sweet merlin","3":"12"},{"1":"cucumbers","2":"pickling","3":"8"},{"1":"tomatoes","2":"grape","3":"5"},{"1":"onions","2":"delicious duo","3":"13"},{"1":"jalapeño","2":"giant","3":"5"},{"1":"hot peppers","2":"thai","3":"4"},{"1":"hot peppers","2":"variety","3":"7"},{"1":"onions","2":"long keeping rainbow","3":"20"},{"1":"tomatoes","2":"big beef","3":"8"},{"1":"tomatoes","2":"bonny best","3":"10"},{"1":"lettuce","2":"lettuce mixture","3":"15"},{"1":"carrots","2":"king midas","3":"10"},{"1":"tomatoes","2":"cherokee purple","3":"15"},{"1":"tomatoes","2":"better boy","3":"10"},{"1":"peppers","2":"variety","3":"7"},{"1":"carrots","2":"dragon","3":"6"},{"1":"tomatoes","2":"amish paste","3":"11"},{"1":"tomatoes","2":"mortgage lifter","3":"15"},{"1":"broccoli","2":"yod fah","3":"7"},{"1":"tomatoes","2":"old german","3":"10"},{"1":"tomatoes","2":"jet star","3":"8"},{"1":"carrots","2":"bolero","3":"6"},{"1":"tomatoes","2":"brandywine","3":"10"},{"1":"tomatoes","2":"black krim","3":"10"},{"1":"tomatoes","2":"volunteers","3":"10"},{"1":"peppers","2":"green","3":"5"},{"1":"beans","2":"classic slenderette","3":"19"},{"1":"potatoes","2":"purple","3":"6"},{"1":"potatoes","2":"yellow","3":"6"},{"1":"beans","2":"chinese red noodle","3":"18"},{"1":"edamame","2":"edamame","3":"7"},{"1":"corn","2":"dorinny sweet","3":"13"},{"1":"corn","2":"golden bantam","3":"13"},{"1":"carrots","2":"greens","3":"6"},{"1":"pumpkins","2":"saved","3":"5"},{"1":"squash","2":"blue (saved)","3":"12"},{"1":"pumpkins","2":"cinderella's carraige","3":"21"},{"1":"broccoli","2":"main crop bravado","3":"17"},{"1":"potatoes","2":"russet","3":"6"},{"1":"kohlrabi","2":"crispy colors duo","3":"17"},{"1":"squash","2":"delicata","3":"8"},{"1":"squash","2":"waltham butternut","3":"17"},{"1":"squash","2":"red kuri","3":"8"},{"1":"pumpkins","2":"new england sugar","3":"17"},{"1":"apple","2":"unknown","3":"7"},{"1":"potatoes","2":"red","3":"3"},{"1":"rutabaga","2":"improved helenor","3":"16"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  filter(str_detect(variety,"er")|str_detect(variety,"ar"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]}],"data":[{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-11","4":"67","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-13","4":"53","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-13","4":"19","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-17","4":"48","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-06-18","4":"40","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-18","4":"47","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-19","4":"39","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-19","4":"38","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-20","4":"22","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-20","4":"16","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-20","4":"148","5":"grams"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-21","4":"37","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-21","4":"95","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-22","4":"52","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-22","4":"40","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-06-22","4":"19","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-22","4":"18","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-23","4":"165","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-24","4":"34","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-24","4":"122","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-25","4":"30","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-06-26","4":"17","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-26","4":"425","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-27","4":"52","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-28","4":"793","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-28","4":"111","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-29","4":"58","5":"grams"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-29","4":"561","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-06-29","4":"30","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-29","4":"82","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-01","4":"60","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-02","4":"743","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-03","4":"217","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-03","4":"88","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-04","4":"285","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-04","4":"147","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-06","4":"48","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-07","4":"67","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-07","4":"43","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-07","4":"13","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-08","4":"75","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-08","4":"178","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-08","4":"39","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-09","4":"61","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-09","4":"131","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-09","4":"140","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-09","4":"69","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-10","4":"61","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-11","4":"60","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-11","4":"77","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-11","4":"79","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-11","4":"105","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-11","4":"701","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-12","4":"89","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-12","4":"83","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-13","4":"50","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-13","4":"85","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-13","4":"53","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-13","4":"40","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-13","4":"443","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-14","4":"526","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-14","4":"152","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-15","4":"743","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-16","4":"61","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-17","4":"88","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-18","4":"77","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-18","4":"172","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-18","4":"660","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-19","4":"37","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-20","4":"336","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-20","4":"519","5":"grams"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-21","4":"21","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-22","4":"351","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-23","4":"129","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-24","4":"100","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-24","4":"32","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-24","4":"93","5":"grams"},{"1":"peppers","2":"variety","3":"2020-07-24","4":"68","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-27","4":"728","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-27","4":"113","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-07-27","4":"29","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-27","4":"49","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-27","4":"39","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-28","4":"312","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-29","4":"442","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-29","4":"240","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-29","4":"305","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-30","4":"101","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-31","4":"307","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-07-31","4":"633","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-31","4":"290","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-31","4":"592","5":"grams"},{"1":"strawberries","2":"perrenial","3":"2020-07-31","4":"23","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-01","4":"619","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-02","4":"336","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-02","4":"211","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-03","4":"308","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-03","4":"572","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-04","4":"339","5":"grams"},{"1":"peppers","2":"variety","3":"2020-08-04","4":"270","5":"grams"},{"1":"peppers","2":"variety","3":"2020-08-04","4":"192","5":"grams"},{"1":"hot peppers","2":"variety","3":"2020-08-04","4":"40","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-05","4":"781","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-05","4":"67","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-05","4":"234","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-06","4":"303","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-08-06","4":"164","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-07","4":"233","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-07","4":"364","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-07","4":"1045","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-07","4":"562","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-08-07","4":"255","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-08","4":"184","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-08","4":"122","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-08","4":"545","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-09","4":"591","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-09","4":"1102","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-09","4":"308","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-09","4":"54","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-10","4":"216","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-10","4":"241","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-08-10","4":"221","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-11","4":"160","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-11","4":"755","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-11","4":"245","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-11","4":"802","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-11","4":"354","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-13","4":"468","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-13","4":"122","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-13","4":"727","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-08-13","4":"198","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"2020-08-13","4":"2209","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-14","4":"238","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-14","4":"181","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-14","4":"490","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-16","4":"328","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-16","4":"599","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-16","4":"238","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-16","4":"693","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-17","4":"764","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-17","4":"306","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-17","4":"350","5":"grams"},{"1":"peppers","2":"variety","3":"2020-08-18","4":"112","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-18","4":"225","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-18","4":"608","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-18","4":"148","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-18","4":"105","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-19","4":"872","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-19","4":"615","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-19","4":"306","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-20","4":"333","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-20","4":"360","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-20","4":"230","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-20","4":"328","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-20","4":"287","5":"grams"},{"1":"peppers","2":"variety","3":"2020-08-20","4":"70","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-21","4":"1601","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-21","4":"243","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-21","4":"562","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-23","4":"801","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-23","4":"704","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-25","4":"578","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-25","4":"115","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-25","4":"186","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-25","4":"320","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-25","4":"488","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-25","4":"1026","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-26","4":"666","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-26","4":"593","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-26","4":"309","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-26","4":"261","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-08-28","4":"29","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-29","4":"737","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-29","4":"1097","5":"grams"},{"1":"peppers","2":"variety","3":"2020-08-29","4":"627","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-08-29","4":"888","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-29","4":"566","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-08-30","4":"861","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-30","4":"599","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-08-30","4":"822","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-30","4":"589","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-30","4":"393","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-30","4":"752","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-01","4":"1953","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-09-01","4":"160","5":"grams"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-09-01","4":"805","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-09-01","4":"201","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-09-01","4":"773","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-01","4":"1202","5":"grams"},{"1":"peppers","2":"variety","3":"2020-09-02","4":"60","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-03","4":"610","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-04","4":"2899","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-04","4":"1234","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-09-04","4":"1178","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-04","4":"255","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-06","4":"2377","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-09-10","4":"674","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-10","4":"1392","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-10","4":"316","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-09-10","4":"754","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-10","4":"413","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-15","4":"725","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-09-17","4":"168","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-17","4":"212","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-18","4":"670","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-09-18","4":"1631","5":"grams"},{"1":"raspberries","2":"perrenial","3":"2020-09-18","4":"137","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-19","4":"2934","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1655","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1927","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1558","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1183","5":"grams"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-19","4":"1311","5":"grams"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-19","4":"6250","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1028","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1131","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1302","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1570","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1359","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1608","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"2277","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1743","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"2931","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-21","4":"95","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-09-25","4":"1823","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-25","4":"2006","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-25","4":"1239","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-25","4":"1978","5":"grams"},{"1":"peppers","2":"variety","3":"2020-09-25","4":"84","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"2020-09-27","4":"94","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"2020-09-27","4":"81","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-09-27","4":"449","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-30","4":"494","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-09-30","4":"70","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-30","4":"327","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-03","4":"213","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"2020-10-03","4":"346","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-07","4":"254","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-10-07","4":"363","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-10-07","4":"64","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-10-10","4":"1977","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-11","4":"200","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-10-11","4":"898","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-10-11","4":"526","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-10-11","4":"230","5":"grams"},{"1":"peppers","2":"variety","3":"2020-10-11","4":"84","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-10-12","4":"709","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"2020-10-12","4":"2143","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-10-12","4":"2990","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"2020-10-12","4":"1300","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-14","4":"859","5":"grams"},{"1":"tomatoes","2":"Old German","3":"2020-10-14","4":"484","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-10-14","4":"219","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"2020-10-14","4":"646","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"2020-10-14","4":"2838","5":"grams"},{"1":"carrots","2":"Bolero","3":"2020-10-14","4":"1500","5":"grams"},{"1":"peppers","2":"variety","3":"2020-10-14","4":"89","5":"grams"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){width="30%"}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){width="30%"}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usual, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(sdate)) + 
  geom_density()
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
* We see in this graph the number of times people rented out a bicycle at a date of time and we see that October to middle of November is the most popular time to rent out a bike.  
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(time = (hour(sdate)) + (minute(sdate)/60)) %>% 
  ggplot(aes(time))+
  geom_density()
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
In this graph we see the most popular time of day to rent out a bicycle where we 8-9 am and 4-5pm are the most popular times to rent out a bike.   
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(day = wday(sdate)) %>% 
  ggplot(aes(y = day))+
  geom_bar()+
  scale_y_discrete(
    limits = c("Sunday","Monday","Tuesday", "Wednesday","Thursday","Friday","Saturday"),
    breaks = c("Sunday","Monday","Tuesday", "Wednesday","Thursday","Friday","Saturday"),  
    labels = c("Sunday","Monday","Tuesday", "Wednesday","Thursday","Friday","Saturday"))
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
*  We see that the most popular days of the week is Monday and Friday.
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>% 
  mutate(time = (hour(sdate)) + (minute(sdate)/60), day = wday(sdate)) %>% 
  ggplot(aes(time))+
  geom_density()+
  facet_wrap(~day)
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
* The weekdays have two peaks at 8-9am and 4-5pm while the weekends have only one peak.   
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(time = (hour(sdate)) + (minute(sdate)/60), day = wday(sdate)) %>% 
  ggplot(aes(time,fill=client, alpha=.5))+
  geom_density()+
  facet_wrap(~day)
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>% 
  mutate(time = (hour(sdate)) + (minute(sdate)/60), day = wday(sdate)) %>% 
  ggplot(aes(time,fill=client, alpha=.5))+
  geom_density( position = position_stack())+
  facet_wrap(~day)
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
* I would say it's worse in terms of telling a story because it does not show the real relationship between clients as the casual clients clearly differ in times of the registered clients  
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(time = (hour(sdate)) + (minute(sdate)/60), day = wday(sdate), isWeekend = ifelse(day == 1 | day== 7, "Weekend","Weekday")) %>% 
  ggplot(aes(time,fill=client, alpha=.5))+
  geom_density()+
  facet_wrap(~isWeekend)
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>% 
  mutate(time = (hour(sdate)) + (minute(sdate)/60), day = wday(sdate), isWeekend = ifelse(day == 1 | day== 7, "Weekend","Weekday")) %>% 
  ggplot(aes(time,fill=isWeekend, alpha=.5))+
  geom_density()+
  facet_wrap(~client)
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
* I would say the first one is better aesthetically and as well in interpretation.  
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Stations %>% 
  mutate(sstation = name) %>% 
  full_join(Trips, 
            by = "sstation") %>% 
  group_by(name, lat, long) %>% 
  count() %>% 
  ggplot(aes(lat,long,color=n))+
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Stations %>% 
  mutate(sstation = name) %>% 
  full_join(Trips, 
            by = "sstation") %>% 
  group_by(name, lat, long) %>% 
  count() %>% 
  ggplot(aes(lat,long,color=n))+
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).

  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.



  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest numer, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  

  
  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.
  

  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**

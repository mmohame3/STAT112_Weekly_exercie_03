---
title: 'Weekly Exercises #3'
author: "Mohamed Abdi Mohamed"
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
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.5     v dplyr   1.0.3
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
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

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   state = col_character(),
##   variable = col_character(),
##   year = col_double(),
##   raw = col_double(),
##   inf_adj = col_double(),
##   inf_adj_perchild = col_double()
## )
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
  select(vegetable, weight, date) %>% 
  mutate(day = wday(date, label = TRUE)) %>% 
  group_by(vegetable, day) %>% 
  summarise(tot_wt_lbs = sum(weight * 0.00220462)) 
```

```
## `summarise()` has grouped output by 'vegetable'. You can override using the `.groups` argument.
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["day"],"name":[2],"type":["ord"],"align":["right"]},{"label":["tot_wt_lbs"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"Sat","3":"0.34392072"},{"1":"asparagus","2":"Sat","3":"0.04409240"},{"1":"basil","2":"Mon","3":"0.06613860"},{"1":"basil","2":"Tue","3":"0.11023100"},{"1":"basil","2":"Thu","3":"0.02645544"},{"1":"basil","2":"Fri","3":"0.46737944"},{"1":"basil","2":"Sat","3":"0.41005932"},{"1":"beans","2":"Sun","3":"1.91361016"},{"1":"beans","2":"Mon","3":"6.50803824"},{"1":"beans","2":"Tue","3":"4.38719380"},{"1":"beans","2":"Wed","3":"4.08295624"},{"1":"beans","2":"Thu","3":"3.39291018"},{"1":"beans","2":"Fri","3":"1.52559704"},{"1":"beans","2":"Sat","3":"4.70906832"},{"1":"beets","2":"Sun","3":"0.32187452"},{"1":"beets","2":"Mon","3":"0.67240910"},{"1":"beets","2":"Tue","3":"0.15873264"},{"1":"beets","2":"Wed","3":"0.18298346"},{"1":"beets","2":"Thu","3":"11.89172028"},{"1":"beets","2":"Fri","3":"0.02425082"},{"1":"beets","2":"Sat","3":"0.37919464"},{"1":"broccoli","2":"Sun","3":"1.25883802"},{"1":"broccoli","2":"Mon","3":"0.82011864"},{"1":"broccoli","2":"Wed","3":"0.70768302"},{"1":"broccoli","2":"Fri","3":"0.16534650"},{"1":"carrots","2":"Sun","3":"2.93655384"},{"1":"carrots","2":"Mon","3":"0.87082490"},{"1":"carrots","2":"Tue","3":"0.35273920"},{"1":"carrots","2":"Wed","3":"5.56225626"},{"1":"carrots","2":"Thu","3":"2.67420406"},{"1":"carrots","2":"Fri","3":"2.13848140"},{"1":"carrots","2":"Sat","3":"2.33028334"},{"1":"chives","2":"Wed","3":"0.01763696"},{"1":"cilantro","2":"Tue","3":"0.00440924"},{"1":"cilantro","2":"Fri","3":"0.07275246"},{"1":"cilantro","2":"Sat","3":"0.03747854"},{"1":"corn","2":"Sun","3":"1.45725382"},{"1":"corn","2":"Mon","3":"0.75838928"},{"1":"corn","2":"Tue","3":"0.72752460"},{"1":"corn","2":"Wed","3":"5.30211110"},{"1":"corn","2":"Fri","3":"3.44802568"},{"1":"corn","2":"Sat","3":"1.31615814"},{"1":"cucumbers","2":"Sun","3":"3.10410496"},{"1":"cucumbers","2":"Mon","3":"4.77520692"},{"1":"cucumbers","2":"Tue","3":"10.04645334"},{"1":"cucumbers","2":"Wed","3":"5.30652034"},{"1":"cucumbers","2":"Thu","3":"3.30693000"},{"1":"cucumbers","2":"Fri","3":"7.42956940"},{"1":"cucumbers","2":"Sat","3":"9.64080326"},{"1":"edamame","2":"Tue","3":"1.40213832"},{"1":"edamame","2":"Sat","3":"4.68922674"},{"1":"hot peppers","2":"Mon","3":"1.25883802"},{"1":"hot peppers","2":"Tue","3":"0.14109568"},{"1":"hot peppers","2":"Wed","3":"0.06834322"},{"1":"jalapeño","2":"Sun","3":"0.26234978"},{"1":"jalapeño","2":"Mon","3":"5.55343778"},{"1":"jalapeño","2":"Tue","3":"0.54895038"},{"1":"jalapeño","2":"Wed","3":"0.48060716"},{"1":"jalapeño","2":"Thu","3":"0.22487124"},{"1":"jalapeño","2":"Fri","3":"1.29411194"},{"1":"jalapeño","2":"Sat","3":"1.50796008"},{"1":"kale","2":"Sun","3":"0.82673250"},{"1":"kale","2":"Mon","3":"2.06793356"},{"1":"kale","2":"Tue","3":"0.28219136"},{"1":"kale","2":"Wed","3":"0.61729360"},{"1":"kale","2":"Thu","3":"0.27998674"},{"1":"kale","2":"Fri","3":"0.38139926"},{"1":"kale","2":"Sat","3":"1.49032312"},{"1":"kohlrabi","2":"Thu","3":"0.42108242"},{"1":"lettuce","2":"Sun","3":"1.46607230"},{"1":"lettuce","2":"Mon","3":"2.45815130"},{"1":"lettuce","2":"Tue","3":"0.91712192"},{"1":"lettuce","2":"Wed","3":"1.18608556"},{"1":"lettuce","2":"Thu","3":"2.45153744"},{"1":"lettuce","2":"Fri","3":"1.80117454"},{"1":"lettuce","2":"Sat","3":"1.31615814"},{"1":"onions","2":"Sun","3":"0.26014516"},{"1":"onions","2":"Mon","3":"0.50926722"},{"1":"onions","2":"Tue","3":"0.70768302"},{"1":"onions","2":"Thu","3":"0.60186126"},{"1":"onions","2":"Fri","3":"0.07275246"},{"1":"onions","2":"Sat","3":"1.91361016"},{"1":"peas","2":"Sun","3":"2.05691046"},{"1":"peas","2":"Mon","3":"4.63411124"},{"1":"peas","2":"Tue","3":"2.06793356"},{"1":"peas","2":"Wed","3":"1.08026380"},{"1":"peas","2":"Thu","3":"3.39731942"},{"1":"peas","2":"Fri","3":"0.93696350"},{"1":"peas","2":"Sat","3":"2.85277828"},{"1":"peppers","2":"Sun","3":"0.50265336"},{"1":"peppers","2":"Mon","3":"2.52649452"},{"1":"peppers","2":"Tue","3":"1.44402610"},{"1":"peppers","2":"Wed","3":"2.44271896"},{"1":"peppers","2":"Thu","3":"0.70988764"},{"1":"peppers","2":"Fri","3":"0.33510224"},{"1":"peppers","2":"Sat","3":"1.38229674"},{"1":"potatoes","2":"Mon","3":"0.97003280"},{"1":"potatoes","2":"Wed","3":"4.57017726"},{"1":"potatoes","2":"Thu","3":"11.85203712"},{"1":"potatoes","2":"Fri","3":"3.74124014"},{"1":"potatoes","2":"Sat","3":"2.80207202"},{"1":"pumpkins","2":"Mon","3":"30.11951844"},{"1":"pumpkins","2":"Tue","3":"31.85675900"},{"1":"pumpkins","2":"Sat","3":"92.68883866"},{"1":"radish","2":"Sun","3":"0.08157094"},{"1":"radish","2":"Mon","3":"0.19621118"},{"1":"radish","2":"Tue","3":"0.09479866"},{"1":"radish","2":"Thu","3":"0.14770954"},{"1":"radish","2":"Fri","3":"0.19400656"},{"1":"radish","2":"Sat","3":"0.23148510"},{"1":"raspberries","2":"Mon","3":"0.13007258"},{"1":"raspberries","2":"Tue","3":"0.33510224"},{"1":"raspberries","2":"Thu","3":"0.28880522"},{"1":"raspberries","2":"Fri","3":"0.57099658"},{"1":"raspberries","2":"Sat","3":"0.53351804"},{"1":"rutabaga","2":"Sun","3":"19.26396956"},{"1":"rutabaga","2":"Fri","3":"3.57809826"},{"1":"rutabaga","2":"Sat","3":"6.89825598"},{"1":"spinach","2":"Sun","3":"0.48722102"},{"1":"spinach","2":"Mon","3":"0.14770954"},{"1":"spinach","2":"Tue","3":"0.49603950"},{"1":"spinach","2":"Wed","3":"0.21384814"},{"1":"spinach","2":"Thu","3":"0.23368972"},{"1":"spinach","2":"Fri","3":"0.19621118"},{"1":"spinach","2":"Sat","3":"0.26014516"},{"1":"squash","2":"Mon","3":"24.33459556"},{"1":"squash","2":"Tue","3":"18.46810174"},{"1":"squash","2":"Sat","3":"56.22221924"},{"1":"strawberries","2":"Sun","3":"0.08157094"},{"1":"strawberries","2":"Mon","3":"0.47840254"},{"1":"strawberries","2":"Thu","3":"0.08818480"},{"1":"strawberries","2":"Fri","3":"0.48722102"},{"1":"strawberries","2":"Sat","3":"0.16975574"},{"1":"Swiss chard","2":"Sun","3":"1.24781492"},{"1":"Swiss chard","2":"Mon","3":"1.07364994"},{"1":"Swiss chard","2":"Tue","3":"0.07054784"},{"1":"Swiss chard","2":"Wed","3":"0.90830344"},{"1":"Swiss chard","2":"Thu","3":"2.23107544"},{"1":"Swiss chard","2":"Fri","3":"0.61729360"},{"1":"Swiss chard","2":"Sat","3":"0.73413846"},{"1":"tomatoes","2":"Sun","3":"75.60964752"},{"1":"tomatoes","2":"Mon","3":"11.49268406"},{"1":"tomatoes","2":"Tue","3":"48.75076206"},{"1":"tomatoes","2":"Wed","3":"58.26590198"},{"1":"tomatoes","2":"Thu","3":"34.51773534"},{"1":"tomatoes","2":"Fri","3":"85.07628580"},{"1":"tomatoes","2":"Sat","3":"35.12621046"},{"1":"zucchini","2":"Sun","3":"12.23564100"},{"1":"zucchini","2":"Mon","3":"12.19595784"},{"1":"zucchini","2":"Tue","3":"16.46851140"},{"1":"zucchini","2":"Wed","3":"2.04147812"},{"1":"zucchini","2":"Thu","3":"34.63017096"},{"1":"zucchini","2":"Fri","3":"18.72163304"},{"1":"zucchini","2":"Sat","3":"3.41495638"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  group_by(vegetable) %>% 
  summarise(tot_wt_lbs = sum(weight * 0.00220462)) %>% 
  left_join(garden_planting, by = "vegetable")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["tot_wt_lbs"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[3],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"basil","2":"1.08026380","3":"potB","4":"Isle of Naxos","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"26.51937398","3":"M","4":"Bush Bush Slender","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"26.51937398","3":"D","4":"Bush Bush Slender","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"26.51937398","3":"K","4":"Chinese Red Noodle","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"26.51937398","3":"L","4":"Chinese Red Noodle","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"26.51937398","3":"E","4":"Classic Slenderette","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"13.63116546","3":"H","4":"Gourmet Golden","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"13.63116546","3":"H","4":"Sweet Merlin","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"2.95198618","3":"P","4":"Yod Fah","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"broccoli","2":"2.95198618","3":"D","4":"Main Crop Bravado","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"2.95198618","3":"I","4":"Main Crop Bravado","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"H","4":"Bolero","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"H","4":"King Midas","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"H","4":"Dragon","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"L","4":"Bolero","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"L","4":"King Midas","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"L","4":"Dragon","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"chives","2":"0.01763696","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"potD","4":"cilantro","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"E","4":"cilantro","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"13.00946262","3":"A","4":"Dorinny Sweet","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"13.00946262","3":"B","4":"Golden Bantam","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"43.60958822","3":"L","4":"pickling","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"6.09136506","3":"O","4":"edamame","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"1.46827692","3":"potB","4":"thai","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"1.46827692","3":"potC","4":"variety","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"9.87228836","3":"L","4":"giant","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"5.94586014","3":"P","4":"Heirloom Lacinto","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"5.94586014","3":"front","4":"Heirloom Lacinto","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"0.42108242","3":"front","4":"Crispy Colors Duo","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"C","4":"Farmer's Market Blend","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"P","4":"Tatsoi","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"L","4":"Farmer's Market Blend","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"G","4":"Lettuce Mixture","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"onions","2":"4.06531928","3":"H","4":"Long Keeping Rainbow","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"4.06531928","3":"P","4":"Delicious Duo","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"17.02628026","3":"A","4":"Super Sugar Snap","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"17.02628026","3":"B","4":"Magnolia Blossom","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"K","4":"green","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"O","4":"green","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"potA","4":"variety","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"potA","4":"variety","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"potD","4":"variety","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"D","4":"purple","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"D","4":"Russet","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"I","4":"yellow","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"I","4":"yellow","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"I","4":"red","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"A","4":"Cinderalla's Carraige","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"B","4":"Cinderella's Carraige","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"B","4":"saved","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"side","4":"Big Max","5":"6","6":"2020-05-24","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"side","4":"Cinderalla's Carraige","5":"6","6":"2020-05-24","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"K","4":"New England Sugar","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"radish","2":"0.94578198","3":"C","4":"Garden Party Mix","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"0.94578198","3":"G","4":"Garden Party Mix","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"0.94578198","3":"H","4":"Garden Party Mix","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"raspberries","2":"1.85849466","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"rutabaga","2":"29.74032380","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"spinach","2":"2.03486426","3":"H","4":"Catalina","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"2.03486426","3":"E","4":"Catalina","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Butternut (saved)","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Red Kuri","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Blue (saved)","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Waltham Butternut","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"B","4":"Red Kuri","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"B","4":"Blue (saved)","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"K","4":"Waltham Butternut","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"K","4":"delicata","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"side","4":"Red Kuri","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"strawberries","2":"1.30513504","3":"F","4":"perennial","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"6.88282364","3":"M","4":"Neon Glow","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Mortgage Lifter","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Old German","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Better Boy","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Amish Paste","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Cherokee Purple","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Brandywine","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Bonny Best","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Amish Paste","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Big Beef","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Mortgage Lifter","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Black Krim","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Jet Star","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Better Boy","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"O","4":"grape","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"volunteers","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"volunteers","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"front","4":"volunteers","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"O","4":"volunteers","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"99.70834874","3":"D","4":"Romanesco","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
 
 > We see NA values. This is because the left_join output all cases from garden_harvest even if it doesn't have a match on garden_planting. To fix this, we can use the inner_join()
 

```r
garden_harvest %>% 
  group_by(vegetable) %>% 
  summarise(tot_wt_lbs = sum(weight * 0.00220462)) %>% 
  inner_join(garden_planting, by = "vegetable")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["tot_wt_lbs"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[3],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"basil","2":"1.0802638","3":"potB","4":"Isle of Naxos","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"26.5193740","3":"M","4":"Bush Bush Slender","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"26.5193740","3":"D","4":"Bush Bush Slender","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"26.5193740","3":"K","4":"Chinese Red Noodle","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"26.5193740","3":"L","4":"Chinese Red Noodle","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"26.5193740","3":"E","4":"Classic Slenderette","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"13.6311655","3":"H","4":"Gourmet Golden","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"13.6311655","3":"H","4":"Sweet Merlin","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"2.9519862","3":"P","4":"Yod Fah","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"broccoli","2":"2.9519862","3":"D","4":"Main Crop Bravado","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"2.9519862","3":"I","4":"Main Crop Bravado","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"carrots","2":"16.8653430","3":"H","4":"Bolero","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.8653430","3":"H","4":"King Midas","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.8653430","3":"H","4":"Dragon","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.8653430","3":"L","4":"Bolero","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.8653430","3":"L","4":"King Midas","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.8653430","3":"L","4":"Dragon","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cilantro","2":"0.1146402","3":"potD","4":"cilantro","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"0.1146402","3":"E","4":"cilantro","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"13.0094626","3":"A","4":"Dorinny Sweet","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"13.0094626","3":"B","4":"Golden Bantam","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"43.6095882","3":"L","4":"pickling","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"6.0913651","3":"O","4":"edamame","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"1.4682769","3":"potB","4":"thai","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"1.4682769","3":"potC","4":"variety","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"9.8722884","3":"L","4":"giant","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"5.9458601","3":"P","4":"Heirloom Lacinto","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"5.9458601","3":"front","4":"Heirloom Lacinto","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"0.4210824","3":"front","4":"Crispy Colors Duo","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.5963012","3":"C","4":"Farmer's Market Blend","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.5963012","3":"P","4":"Tatsoi","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.5963012","3":"L","4":"Farmer's Market Blend","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.5963012","3":"G","4":"Lettuce Mixture","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"onions","2":"4.0653193","3":"H","4":"Long Keeping Rainbow","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"4.0653193","3":"P","4":"Delicious Duo","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"17.0262803","3":"A","4":"Super Sugar Snap","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"17.0262803","3":"B","4":"Magnolia Blossom","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.3431796","3":"K","4":"green","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.3431796","3":"O","4":"green","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.3431796","3":"potA","4":"variety","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.3431796","3":"potA","4":"variety","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.3431796","3":"potD","4":"variety","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"23.9355593","3":"D","4":"purple","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.9355593","3":"D","4":"Russet","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.9355593","3":"I","4":"yellow","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.9355593","3":"I","4":"yellow","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"potatoes","2":"23.9355593","3":"I","4":"red","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"pumpkins","2":"154.6651161","3":"A","4":"Cinderalla's Carraige","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.6651161","3":"B","4":"Cinderella's Carraige","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.6651161","3":"B","4":"saved","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.6651161","3":"side","4":"Big Max","5":"6","6":"2020-05-24","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.6651161","3":"side","4":"Cinderalla's Carraige","5":"6","6":"2020-05-24","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.6651161","3":"K","4":"New England Sugar","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"radish","2":"0.9457820","3":"C","4":"Garden Party Mix","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"0.9457820","3":"G","4":"Garden Party Mix","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"0.9457820","3":"H","4":"Garden Party Mix","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"spinach","2":"2.0348643","3":"H","4":"Catalina","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"2.0348643","3":"E","4":"Catalina","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"99.0249165","3":"A","4":"Butternut (saved)","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"A","4":"Red Kuri","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"A","4":"Blue (saved)","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"A","4":"Waltham Butternut","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"B","4":"Red Kuri","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"B","4":"Blue (saved)","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"K","4":"Waltham Butternut","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"K","4":"delicata","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"99.0249165","3":"side","4":"Red Kuri","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"strawberries","2":"1.3051350","3":"F","4":"perennial","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"6.8828236","3":"M","4":"Neon Glow","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Mortgage Lifter","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Old German","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Better Boy","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Amish Paste","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Cherokee Purple","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Brandywine","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"Bonny Best","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"Amish Paste","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"Big Beef","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"Mortgage Lifter","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"Black Krim","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"Jet Star","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"Better Boy","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"O","4":"grape","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"N","4":"volunteers","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"J","4":"volunteers","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"front","4":"volunteers","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.8392272","3":"O","4":"volunteers","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"99.7083487","3":"D","4":"Romanesco","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
 

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.



  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  


  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.




## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){300px}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){300px}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  


  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**

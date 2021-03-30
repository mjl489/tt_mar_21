# tt_mar_21
My tidytuesday
#load the libraries

library(tidytuesdayR)
library(tidyverse)
library(dplyr)
library(stringr)

# load data
tuesdata <- tidytuesdayR::tt_load('2021-03-30')

#I wanted to look at how the shades varied within common descriptors. I decided to work with (tuesdata$allShades) to explore this
# move this into a separate object


shades <- tuesdata$allShades
view(shades)

#apply labels, skimmed the data and picked 12 terms that looked interestin in the name category and made them labels
#first time using ifelse with grepl in mutate. Useful.

shades <- mutate(shades, colcat = ifelse(grepl("Espresso", name), "Espresso", ifelse(grepl("Beige", name), "Beige", ifelse(grepl("Ivory", name), "Ivory",ifelse(grepl("Caramel", name), "Caramel",ifelse(grepl("Olive", name), "Olive",ifelse(grepl("Walnut", name), "Walnut",ifelse(grepl("Nude", name), "Nude",ifelse(grepl("Tan", name), "Tan",ifelse(grepl("Chocolate", name), "Chocolate",ifelse(grepl("Cream", name), "Cream",ifelse(grepl("Mocha", name), "Mocha", ifelse(grepl("Sand", name), "Sand","Other")))))))))))))

#remove 'other'. There's probably a way to do this with piping
# I had tried, 
# plot <- filter(shades, colcat != "Other") %>%  ggplot(.data, aes(x = lightness, y = sat, color = hex)) + geom_point() + scale_color_identity() + facet_wrap(~brand) + xlab("Lightness") + ylab("Saturation") + theme_minimal()
#but kept getting error Mapping should be created with `aes() or `aes_()`
#so used the clumsier option below

p1dat <- filter(shades, colcat != "Other")

#check it has worked
view(p1dat)

#plot whats in a name. Scale colour identity using the hex was an interesting thing for me!
p1 <- ggplot(p1dat, aes(x = lightness, y = sat, color = hex)) + geom_point() + scale_color_identity() + facet_wrap(~colcat) + xlab("Lightness") + ylab("Saturation") + theme_classic() + labs(title = "What's in a name?",subtitle = "The range of foundation colours seen for common names", caption = "@wannabehawkeye") +  theme(plot.background = element_rect(fill = "#F3D7D1")) + theme(panel.background = element_rect(fill = "#A2A2A2", colour = "#A2A2A2",size = 2, linetype = "solid")) 
p1

![image](https://user-images.githubusercontent.com/81650490/113048913-f2d57a80-919a-11eb-9221-936538d0a818.png)

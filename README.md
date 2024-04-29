## About this Dataset

This data set, Video Game Sales, comes from vgchartz.com. It examines data from more than 16,500 games. It looks at games and their genre, sales rank, platform, published year, publisher, and the number of sales made in various locations. The reason I chose to analyze this dataset is because I am interested in video games and I think it is interesting to see what types of games do better and why.

``` {r dataset}
vgs <- read.csv("C:\\Users\\phomo\\Documents\\data115\\vgsales.csv")

library(tidyverse)
library(ggplot2)
```

## Processing the Data

### Cleaning the Data

To clean the data I opened up the file in Excel. I noticed that there were some cells that included N/A. I wanted to make sure that this wouldn't interfere with my data, so I used the COUNTIF function on the columns to see how many there were. In the "Year" column that reports what year each game was published, there were 271 cells marked "N/A." In the "Publisher" column that reports who the publisher of each game is, there were 58 cells marked "N/A." Out of 16,600 rows, these numbers were not concerning to me. The missing values in the "Year" column, which is larger than the amount in the "Publisher" column, only make up roughly 1.6% of the data in the column. Because this amount is so insignificant, I didn't take out these rows (especially since there is still valuable information like the genre, rank, name, and platform included in them). I also checked for repeats of games or ranks but was not able to find any that were mistakes. The games that were repeated were only repeated because they were released on another platform.

### Getting the Data Ready

``` {r class}
vgs$Year <-as.numeric(vgs$Year)

vgstop <- read.csv("C:\\Users\\phomo\\Documents\\data115\\top1000.csv")
vgstop$Year <-as.numeric(vgstop$Year)
```

The Year column was automatically classed as character values, so I changed it to numeric values. I also added a file that only has the top 1,000 ranked video games so I can look at the top games more easily. I did the same process of changing the Year classification.

## The Big Question

The main question I want to answer is "How does a video game's genre affect its sales?" This dataset works well in answering this question because it includes the genre of each game and how much it sold globally and in specific locations. I will also explore more around this question by looking into whether specific genres do better in some locations, years, or platforms over others. This dataset offers many areas to look into regarding game genres and their success. 

## The Analysis

### Sales VS Genre

``` {r global}
ggplot(vgs, aes(x= Genre, y = Global_Sales))+geom_bar(stat = "identity", fill = "mediumpurple4")+labs(title="Video Game Global Sales by Genre",x="Genre",y="Global Sales")


```

The Video Game Global Sales by Genre barplot shows that the genres that have sold the most globally were Action and Sports. The three that did the worst globally were Adventure, Puzzle, and Strategy.

```{r specifics}
ggplot(vgs, aes(x= Genre, y = NA_Sales))+geom_bar(stat = "identity", fill = "mediumorchid1")+labs(title="Video Game North American Sales by Genre",x="Genre",y="North American Sales")

ggplot(vgs, aes(x= Genre, y = EU_Sales))+geom_bar(stat = "identity", fill = "mediumorchid2")+labs(title="Video Game European Sales by Genre",x="Genre",y="European Sales")

ggplot(vgs, aes(x= Genre, y = JP_Sales))+geom_bar(stat = "identity", fill = "mediumorchid3")+labs(title="Video Game Japanese Sales by Genre",x="Genre",y="Japanese Sales")

ggplot(vgs, aes(x= Genre, y = Other_Sales))+geom_bar(stat = "identity", fill = "mediumorchid4")+labs(title="Video Game Other Country Sales by Genre",x="Genre",y="Other Country Sales")

```

All but one of these four barplots are nearly identical to the Video Game Global Sales by Genre barplot. The Video Game Japanese Sales by Genre barplot shows that the genre that sold the most was Role-Playing. All of the other genres are in the medium-low sales range.

### Clusters

```{r rank}
ggplot(vgstop, aes(x = Year, y = Rank, color = Genre)) + geom_point()+labs(title = "Top 1,000 Video Games by Published Year and Genre", x= "Published Year", y = "Rank")
```

The Top 1,000 Video Games by Published Year and Genre scatterplot shows us two important things. First, The majority of games receiving higher rankings were published around 2010. It is important to note though that the number of games being published each year heavily increased in the 2000s, so naturally, more games will be ranked higher. The second important thing to note is that there aren't any clear clusters past 1990. Pre-1990, we can slightly see clusters. Sports were ranked relatively low, and Platform games were ranked high. 

### Genres Through the Years

```{r yeargenre}
ggplot(vgs, aes(x = Year, y = Genre, color = Rank)) +
  geom_line(aes(group = Genre), size = 2) +
  labs(title = "Video Games by Year and Rank", x = "Year", y = "Rank")+ scale_color_gradient2(low = "green", high = "red", mid = "pink", midpoint = 8500, limit = c(0,17000))
```

The Video Games by Year and Rank graph shows us how every game genre progressed in rankings through the years. All genres generally started off in higher ranks through the 1980s, but then became lower ranked in the 1990s. This difference is likely due to the amount of video games being published each year. Since there were fewer video games published in the 1980s, each game was able to get a higher ranking. Something to note is that Strategy games only became more popular (as in anywhere in the rankings) in the 1990s. It is also interesting to see that simulation games were published in 2020.


```{r yeargenretop}
ggplot(vgstop, aes(x = Year, y = Genre, color = Rank)) +
  geom_line(aes(group = Genre), size = 2) +
  labs(title = "Top 1,000 Video Games by Year and Rank", x = "Year", y = "Rank")+ scale_color_gradient2(low = "green", high = "red", mid = "pink", midpoint = 500, limit = c(0,1000))


```

The Top 1,000 Video Games by Year and Rank graph only shows the top 1,000 ranked video games. This graph gives us a little more of a zoomed-in picture of the top video games. Looking at Strategy, we can see that it was in the top 1,000 for a short amount of time. It was not highly sought after after the 2010s. Simulation games started to get a little more popular in 2010 but then started to go down. Most of the genres had an even chance in the rankings after the 2000s, but only Action, Sports, and Shooter games were able to stay in the top 1,000 games by the end. The shortest games on here are the Strategy and Puzzle games. This is similar to the Video Game Global Sales by Genre barplot, but this graph shows the popularity of recent genres. For example, there was still a pretty big divide between Action and Sports games on the other barplot, but this graph tells us that recently they have been in the top 1,000 in close amounts. It also shows that the Sports Genre started to get a little higher in ranking by the latest year on the graph. 

### Platforms and Genre

```{r platform}
unique(vgs$Platform)
pnumtop <-vgstop %>% count(Platform)
print(pnumtop)

```

There are 31 different platforms, so I only examined the most occurring five platforms in the top 1,000 ranked video games. This will allow me to examine the best-performing platforms and their best-performing games. The top five platforms were PS2 with 164 games, X360 with 130 games, PS3 with 120 games, PS with 82 games, and Wii with 78 games.

```{r specificplatforms}

ps2top <- vgstop %>% filter(Platform == "PS2")

PS3top <- vgstop %>% filter(Platform == "PS3")

X360top <- vgstop %>% filter(Platform == "X360")

PStop <- vgstop %>% filter(Platform == "PS")

wiitop <- vgstop %>% filter(Platform == "Wii")

```

I split up each of these five platforms to be able to look at them separately and with more details.

```{r platformgraphs}

ggplot(ps2top, aes(x = Genre, y = Global_Sales))+ geom_boxplot()+ labs(title = "PS2 Genre VS Global Sales", x= "Genre", y= "Global Sales")

ggplot(PS3top, aes(x = Genre, y = Global_Sales))+ geom_boxplot()+ labs(title = "PS3 Genre VS Global Sales", x= "Genre", y= "Global Sales")

ggplot(X360top, aes(x = Genre, y = Global_Sales))+ geom_boxplot()+ labs(title = "X360 Genre VS Global Sales", x= "Genre", y= "Global Sales")

ggplot(PStop, aes(x = Genre, y = Global_Sales))+ geom_boxplot()+ labs(title = "PS Genre VS Global Sales", x= "Genre", y= "Global Sales")

ggplot(wiitop, aes(x = Genre, y = Global_Sales))+ geom_boxplot()+ labs(title = "Wii Genre VS Global Sales", x= "Genre", y= "Global Sales")

```

These graphs show us a more detailed explanation of what genres sell. The PS2, PS3, and X360 Genre VS Global Sales graphs tell us that for those platforms the Action genre has many outliers that might be skewing the global sales results. These graphs also have Shooter games doing relatively well with  fewer outliers. The PS Genre VS Global Sales graph tells us that for the PS the genre is not a huge factor for global sales. Many games on the PS are making more sales than they would on other platforms. The Wii Genre VS Global Sales graph tells us that the Sports genre makes the most global sales. This makes sense for Wii since it allows for more body-tracking capabilities. All of these graphs show that Puzzle, Strategy, and Simulation games are not doing very well.

### Outlier

```{r outlier}
ggplot(vgstop, aes(x = Global_Sales, y = Global_Sales, color = Genre))+ geom_point()+ labs(title = "Global Sales by Genre",x = "Global Sales", y = "Global Sales")


```

The Global Sales by Genre gives us a good look at why the Sports genre is regarded as a top genre. The one pink dot at the very top right of this graph represents a Sports game. This game is "Wii Sports" from Wii. This game is the top-ranked game and made 82.74 million sales. That number is more than double the second-ranked game, "Super Mario Bros." This tells us that Sports games don't make as much as was shown on the Video Games Global Sales by Genre graph, this one game just made a huge skew in the results. As we can see in the Platform Genre VS Global Sales graphs, Sports is generally making an average amount of sales.

## Methodology

This subject was a little difficult to examine since a key part of it was based on data that was classed as characters. This made it difficult to perform things like scatterplots, linear regression, correlation, etc. since I was not able to deal with only numbers. The bar charts helped give me an initial idea of what to look for, but they didn't include enough data to tell me the full story. I think the boxplots were extremely helpful in showing the details and helping me understand the breakdown of genres and how much they sell. I also had difficulty with the huge size of the data and the fact that it spanned over a large amount of time because things that were made earlier were able to get higher rankings. When I used the whole dataset, it didn't give me an accurate answer for how genres affect games currently. The graphs looking at the rankings through the years helped me realize this. I think the top 1,000 games helped with this because a huge amount of the overall games were old games that could skew the results. It also gave me a more detailed look into what factors contributed to having high-ranking games in particular. The very last  graph helped finish the story and explain why Sports was so huge in the earlier bar charts. Overall, I think my work became more detailed and helpful in explaining the dataset chronologically. 

## Final Conclusions

There is not one answer for what is the best and worst video game genre. Even though Action and Sports games had the most global sales, that was largely due to outliers. This means that Action and Sports games are not very reliable in making many sales, but when a game is done well, it will have a much larger amount of sales than other possible genres. Shooter games do relatively well across the board. If I were to recommend a video game genre to make a game in, it would be the Shooter genre because they typically do well without the presence of outliers skewing results. This also means that when they are great games, they might not sell as much as Action and Sports games. However, if someone were to only publish a game in Japan, I would tell them to make it in the Role-Playing genre. Another trend that can be seen is that platforms, like the Wii, that have most of their marketing on movement tracking should make games in the Sports genre as they do relatively well. If someone were to make a game on the PS, I would tell them that pretty much any genre would do well. The PS has a much wider variety of games in different genres that sold in the top 1,000. However, this could be because it was one of the oldest platforms, and not as many games were being published so they had a greater chance of being in the top. One thing that carries across all platforms is that you should not make Puzzle or Strategy games as they did badly all across the board.

### Simplified Conclusions

- Action and Sports games are not reliable, but when they do well, they sell the most.
- Shooter games do relatively well across the board.
- In Japan, Role-Playing games do the best.
- Movement Tracking platforms should focus on Sports games.
- Every genre does relatively well on the PS, though this can be because it is an old platform.
- Puzzle and Strategy games do relatively badly.

## The Future

A factor that I think should be added to this research should be star ratings. Even though something sold a lot, doesn't mean that people enjoyed it. Especially when taking into consideration that many old games were ranked highly, I think it would be more telling to see if people enjoyed them or if they just bought them because they had no other games to play. I would also like to see this data for recent years. There is only one game in this dataset that was published in 2020, and there are no later years than that. I think it would be especially interesting because my observational research has told me that people are starting to really enjoy RPGs. After Baldur's Gate 3 won the Game of the Year in 2023, I would be interested to see if there was a shift in genre sales.

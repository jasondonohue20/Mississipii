# Mississipii

# shinny link 
http://jasondonohue20.shinyapps.io/Mississipii?_ga=2.133186124.354208832.1682541392-746115546.1677884294
# librariers
```
library(tidyverse)
library(ggplot2)
library(dplyr)
library(shiny)
library(lubridate)
```

# dates
I changed the format of the date in excel to show the number instead of 3 letters for each month. This allowed me to format the dates in R using as.Date.
```
Kickapoo$Sample.Date <- as.Date(Kickapoo$Sample.Date, format = "%m/%d/%y")
Crow$Sample_Date <- as.Date(Crow$Sample_Date, format = "%m/%d/%y")
```

# merge code
```
Kickapoo_date <- Kickapoo %>%
  select(Sample.Date, Nitrate..mg.L., Phosphate..PO4.P.mg.L., Chloride..mg.l.) %>%
  pivot_longer(cols = -Sample.Date, names_to = "variable", values_to = "value")

Kickapoo_site <- Kickapoo %>%
  select(Site.code, Nitrate..mg.L., Phosphate..PO4.P.mg.L., Chloride..mg.l.) %>%
  pivot_longer(cols = -Site.code, names_to = "variable", values_to = "value")

Master_site <- Master %>%
  select(Site, Wetland, Grass, Forest) %>%
  pivot_longer(cols = -Site, names_to = "variable", values_to = "value")

Master_water <- Master %>%
  select(Water, Wetland, Grass, Forest) %>%
  pivot_longer(cols = -Water, names_to = "variable", values_to = "value")
  ```
  This allowed me to have a column called variable that combines all of the rows that I select. It then takes the value that was assigned to each piece of data and put that in the value row. 
  
  # Code to explain Inputs
  ```
  ui<-fluidPage( 
  
  tabsetPanel(
    
    tabPanel("table",
             fluidRow(
               column(2.5, textOutput("text_output1")),
               column(3.5,DT::dataTableOutput("table", width = "100%")))),
    
    tabPanel("plot1-2",
             fluidRow(
               column(7, textOutput("text_output2")),
               column(8,plotOutput('plot_01', width = '1000px')),
               column(8, textOutput("text_output3")),
               column(8,plotOutput('plot_02', width = '2000px')))),
  ```
  Each one has a text related to each chart/table created. 
  
  # Code to explain outputs
   I created six different charts and one table. I used geom_point() and geom_col() to create my charts. 
  
  ```
  output$text_output7 <- renderText({ text7 })
  output$table <- DT::renderDataTable(Kickapoo[,c("Nitrate..mg.L.", "Phosphate..PO4.P.mg.L.", "Chloride..mg.l.", "Sample.Date")],options = list(pageLength = 4))
  
  
  output$plot_01 <- renderPlot({
    ggplot(Kickapoo_date, aes(x = Sample.Date, y = value, fill = variable)) +
      geom_col() +
      xlab("Sample Date") +
      ylab("Totals") +
      ggtitle("Totals of Nitrate, Phosphate, and Chloride by Sample Date") 
    
  })
  
  output$plot_02 <- renderPlot({
    ggplot(Kickapoo_site, aes(x = Site.code, y = value, fill = variable)) +
      geom_col() +
      xlab("Site") +
      ylab("Totals") +
      ggtitle("Totals of Nitrate, Phosphate, and Chloride by Site") 
   
   output$plot_03 <- renderPlot({
    ggplot(Crow, aes(x = Site , y = Temperature , color = pH)) +
      geom_point(size = 2) +
      xlab("site") +
      ylab("Temp") +
      ggtitle("How Temperature affects Ph at each site")

    }) 
    
    output$plot_05 <- renderPlot({
      ggplot(Master_site, aes(x = Site , y = value , color = variable)) +
        geom_point(size = 2) +
        xlab("Site") +
        ylab("Totals") +
        ggtitle("How Site affects the Wetlands, Grass, and forest")
    
  })
  
  
  
  
  
  

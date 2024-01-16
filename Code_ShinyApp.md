### Here is the R code for the Shiny application: 

<br> 

```
library(shiny)

# Load in the data
sdm <- readRDS("pre_outputList2.RData") # Fix file pathway
names(sdm) <- c("Maxent Range: Present-Precise", "Maxent Range: Present-Total",
                "Maxent Range: Future-Precise", "Maxent Range: Future-Total", 
                "Difference: Present & Future Precise", "Difference: Present & Future Total", 
                "Difference: Precise & Total Present", "Difference: Precise & Total Future",
                "All Differences: Maxent", "All Differences: Range", 
                "All Differences: Climate", "All Differences: County")

# Create the User Interface
ui <- fluidPage(
  titlePanel("title panel"),
  
  sidebarLayout(
    sidebarPanel("sidebar panel",
                 selectInput("var", 
                             label = "Choose a variable to display",
                             choices = names(sdm),
                             selected = "Maxent Range: Present-Precise"),),
    mainPanel("main panel",
              textOutput("selected_var"),
              plotOutput("selected_plot"))
  )
)


# Define the server 
server <- function(input, output) {
  
  output$selected_var <- renderText({ 
    paste("You have selected", input$var)
  })
  
  output$selected_plot <- renderPlot({ 
    show(sdm[which(names(sdm) == input$var)])
  })
  
}

# Run the app 
shinyApp(ui = ui, server = server)

```

<br>

<br>

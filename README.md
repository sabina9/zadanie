require(babynames)

require(shiny)
ui <- fluidPage(
  titlePanel("Moja aplikacja"), #tytul
  textInput("input1", "Wpisz imię", "domyslnie"),
  h5('Wykonamy wykres dla imienia:'),
  textOutput("tekst1"),
  
  sidebarLayout(
    sidebarPanel(
      sliderInput("bins",
                  "Ilość słupków:",
                  min = 1,
                  max = 134,
                  value = 100)
    ),
    
    # Show a plot of the generated distribution
    mainPanel(
      plotOutput("distPlot")
    )
  ))
  

server <- function(input, output){
  
  output$tekst1 <- renderText(input$input1)
  output$distPlot <- renderPlot({
    nie_imie = which(babynames[,3]!= input$input1)
    x    <- rep(babynames[-nie_imie,][,1],babynames[-nie_imie,][,4]) 
    bins <- seq(min(x), max(x), length.out = input$bins + 1)
    # draw the histogram with the specified number of bins
    hist(x, breaks = bins, col = 'cadetblue3', border = 'black', xlab = 'rok',
         ylab = 'Czestość występowania', main = paste("Wykres dla imienia" , input$input1)
  )})}
shinyApp(ui, server)

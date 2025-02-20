#schatztruhe #haldane

## Haldane Treasure Info

#### Packages Used
- learnr (including shiny, rmarkdown, knitr) -> is on server!
- tidyverse (mainly ggplot2 & dplyr)
- ggbeeswarm (beeswarm plots)
- RColorBrewer
### learnr
Package to make tutorials in R, with interactive html output. Activated by setting `runtime=shiny-prerendered`and `output=learnr::tutorial`in the YAML & loading the learnr library in setup.

[learnr-docs](https://rstudio.github.io/learnr/) 

- Based on R Markdown & Shiny
- uses the [*shiny-prerendered*](https://rmarkdown.rstudio.com/authoring_shiny_prerendered) runtime for R Markdown:
	- this allows us to split up the ui() and server() chunks of shiny apps & put other content between them :)
	- should be faster than a normal Rmd with "shiny" runtime
	- gives a simple and pretty framework for the (otherwise not very well documented?) shiny-prerendered runtime
- simple structuring of content:
	- every \## header becomes a new section, otherwise basic R Markdown
- allows for interactive exercises, videos etc.

learnr loads shiny, knitr and rmarkdown internally, so no need to load `library(shiny)` etc.

### Shiny & learnr
[Simple Shiny Basics](https://shiny.posit.co/r/getstarted/shiny-basics/lesson1/) and [Input Widgets](https://shiny.posit.co/r/gallery/widgets/widget-gallery/).

Shiny apps are generally made up of a UI function and a Server function, which get processed inside a ShinyApp(). Inside learnr, these are not explicitly called but set via the `context` in the chunk header. This may make it a bit harder to understand how the app's parts are connected, but in turn allows us to add other content between related shiny chunks (e.g. between the input widgets and the resulting plot).

I'll show these differences with a minimal example:

|                         | **classic shiny & Rmd**                                                                                                              | **learnr** version                                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| YAML                    | `output: html`<br/>`runtime: shiny`                                                                                                  | `output: learnr::tutorial` <br/>`runtime:shiny_prerendered`                                                                                                        |
| setup code              | `{r setup, include=F}`<br/>`library(shiny)`                                                                                          | `{r setup, include=F}`<br/>`library(learnr)`                                                                                                                       |
| data / global functions | `{r} # any r code chunk`<br/>`dd <-read_csv("data.csv")`                                                                             | `{r, context="data"}`<br/>`dd <- read_csv("data.csv")`                                                                                                             |
| UI                      | `{r, echo=F} #`<br/>`ShinyApp( ui = fluidPage(numericInput("num", label = "Numeric Input", value = 1), verbatimTextOutput("ddsum"))` | `{r, echo=F} # any r code chunk`<br/>`numericInput("num", label = "Numeric Input", value = 1)`<br/>throw in a markdown chunk<br/>`{r} verbatimTextOutput("ddsum")` |
| Server                  | `# still inside ShinyApp()!!`<br/>`server = function(input, output) { output$value <- renderPrint({ input$num + dd[1] }) } )`        | `{r, context="server"}`<br/>`output$value<-renderPrint({ input$num + dd[1] })`                                                                                     |

### Publishing it onto shinyio/posit
From your local computer this is super simple via RStudio. Just click publish, enter your shinyio account details, choose which files to upload and let it do its thing.
Files/Folders to upload (minimal, the rest will be recreated on the server):
- Haldane-Treasure.Rmd
- data_human_rec.csv
- /images

### Publishing it onto the Banklab Shiny-Server (IBU)
-> see the shiny server instructions I sent to Claudia if needed (for putting up a new version).

Generally, shinyio/posit is simpler for publishing it (no need to worry about which packages may or may not be available for instance), but for larger lectures it _might_ make more sense to host on the IBU server. Maybe check with Pierre what he thinks is best (how much our server can handle & how consistently it should work).

### Finally
Don't hesitate to shoot me an email at (jalounic@pm.me) before you spend too much time battling with something that I might be able to help with :).




# docker-shiny-studio
Trying to get Shiny and RStudio to play in the same container...

An unofficial "fork" of the `sevenbridges-r` Docker config: https://github.com/sbg/sevenbridges-r

`Goal`: get Shiny Server + Rstudio to launch with one container. Allow user to "pull" that image (e.g. `FROM docker-shiny-studio` inside their own Dockerfile) and then containerize their own developmental version of their Shiny app for dev+testing purposes.

WHY? Having the container launch an Rstudio session that has access to the bash terminal is a nice way of checking the container config and allows new users to explore the setup. It also allows the user to modify their containerized Shiny app on the fly in case it's not working right off the bat (e.g. need to install another R Library, fix code, install Linux dependencies etc.). 

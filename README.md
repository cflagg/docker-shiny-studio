# docker-shiny-studio
Trying to get Shiny and RStudio to play in the same container...

An unofficial "fork" of the `sevenbridges-r` Docker config: https://github.com/sbg/sevenbridges-r

`Goal`: get Shiny Server + Rstudio to launch with one container. Allow user to "pull" that image (e.g. `FROM docker-shiny-studio` inside their own Dockerfile) and then containerize their own developmental version of their Shiny app for dev+testing purposes.

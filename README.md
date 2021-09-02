# docker-shiny-studio
Trying to get Shiny and RStudio to play in the same container...

An unofficial "fork" of the `sevenbridges-r` Docker config: https://github.com/sbg/sevenbridges-r

`Goal`: get Shiny Server + Rstudio to launch with one container. Allow user to "pull" that image (e.g. `FROM docker-shiny-studio` inside their own Dockerfile) and then containerize their own developmental version of their Shiny app for dev+testing purposes.

WHY? Having the container launch an Rstudio session that has access to the bash terminal is a nice way of checking the container config and allows new users to explore the setup. It also allows the user to modify their containerized Shiny app on the fly in case it's not working right off the bat (e.g. need to install another R Library, fix code, install Linux dependencies etc.). 
It may also assist with development on older libraries within the Docker container, as opposed to your local computers **likely** up to date libraries.

## Recap: Docker

The general Docker workflow consists of three primary concepts: 

* **Dockerfile**: contains instructions for what to install onto a "virtual machine" (VM) and what files/folders (your R code) to copy INTO it. Dockerfiles can "borrow" features from other images to simplify the instructions. 
* **Image**: a Dockerfile is used to create an image, which is the VM with the installed Linux and R dependencies. This file can be several gigabytes in size.
* **Container**: an image can be used to launch N number of containers, which are active running instances of your VM and code. 

## Quick Setup Steps

For when you already have Docker installed on your computer. 

`<text>` = variables input by user, without angle brackets

### Build image from Dockerfile and run container from image

1. From command line/bash terminal: you must be in the working/same directory as the Dockerfile
2. Execute `docker build -t <name-of-your-image> .` 
    * NOTE the period here, which indicates "everything here in this folder"
3. Once done compiling, launch an image into a container with `docker run --rm -p 3838:3838 <name-of-your-image>`
    * If the image is compiled with RStudio, you can set the password with `docker run -d -p 3838:3838 -p 8787:8787 -e ADD=shiny -e PASSWORD=<password> <name-of-your-image>`
4. There are other variations of using `docker run` with different arguments, see documentation for more details:
    * `docker run -d --name=<name-your-container> --restart=always -p 3838:3838 <name-of-image>` e.g. `docker run -d --name=shiny-app-container --restart=always -p 3838:3838 my-shiny-app-image`
	
### Share files between Docker container and local computer

* This is useful if you need to persist some files from the Docker container like logs. For serious data that need to be permanent, consider writing your files to cloud storage or a database.

Sources (a tutorial and repository put together by Symbolix): https://www.symbolix.com.au/blog-main/r-docker-hello
https://github.com/SymbolixAU/useR_docker_tutorial
https://github.com/SymbolixAU/r_docker_hello

* __From Git Bash__: `docker run -d --rm -p 8787:8787 --name <container-name> -e USERID=$UID -e PASSWORD=<password> -v ${PWD}/<relative-local-folder-path>:<defined-container-file-path> <image-name>

* **From Windows Command Line**: `docker run -d --rm -p 8787:8787 --name <container-name> -e USERID=$UID -e PASSWORD=<password> -v <absolute-file-path>:<defined-container-file-path> <image-name>

e.g. 

**From Git Bash**: `docker run -d --rm -p 28787:8787 --name hello-world -e USERID=$UID -e PASSWORD=SoSecret! -v ${PWD}/Data:/home/rstudio/Data rstudio/hello-world`
    * Meaning: "/${PWD}/Data" = "present working directory, use this existing folder named Data"
	* "/home/rstudio/Data" = "whatever is written to this Docker container's folder will also appear in the local name space" . Anything the Docker container writes OUTSIDE of this path will NOT appear locally
**From Windows Command Line**: `docker run -d --rm -p 28787:8787 --name hello-world -e USERID=$UID -e PASSWORD=SoSecret! -v C:\Users\your_user_name\Documents\GitHub\r_docker_hello\Data:/home/rstudio/Data rstudio/hello-world`

### Interact or debug Docker container by linking into terminal

This will log you into the active container, allowing you to explore directories and execute code if necessary. For example, you can launch R or execute R commands to explore. 

1. Use Windows Command Line for the following commands
2. `docker exec -it <container-name> bash`
3. If a Shiny app isn't launching, check the logs
	4. `cd var/log/shiny-server`	
	5. `cat <log_name.log>`

### Other Diagnostic/Helpful Commands

* `docker info`
* `docker image ls`
* `docker container ls`

# Whats this
This docker image contains, **out of the box**, the haskell compiler along with the Haskell IDE Engine (HIE) running on an ubuntu base image.

## Who is this for
This image is the result of my frustrations from trying to learn haskell. Haskell is a pretty cool language but the web of abandonded or *deprecated* projects combined with poor IDE tooling has made learning haskell an 80/20 situation where 80 of the effort has been spent on tooling. To make things simpler, I've come up with this docker image. 

### Installed
* GHCup v0.1.5
  * Allows for easily installing different versions of the haskell compiler
* GHC 8.6.5 ("base-4.12")
* Stack 2.3.1
  * Resolver set to [lts-14.27](https://www.stackage.org/lts-14.27)
  * system-ghc set to true - Stack won't try to install isolated GHC compilers. 
  * It's a package manager
* Cabal 3.2.0.0
  * It's a package manager
* Haskell IDE Engine 1.4
* gen-hie
  * Generate a hie.yaml file from a .cabal project file (optional)
* code-server
  * Server/browser based Visual Studio Code 
  
# How to use
This image can be used in two ways: as a .devcontainer or as a stand alone docker image

## .devcontainer
#### Info
Visual Studio Code with the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension lets you use docker images. Docker images contain a virtualized environment for your language/project. When VS code opens a folder it looks for a ".devcontainer" folder. When it finds a .devcontainer folder, it reads the configuration file. From this file, Visual Studio Code then builds a container from a specific image, exposes any specified ports, and installs any specified Visual Studio Code extensions. It then maps the folder containing the .devcontainer as a volume inside the docker container. 

#### How do I use it
Install the Remote Development extension, download the .devcontainer from this repo, drop it into whatever folder contains your haskell project. Reopen that folder in Visual Studio Code.

## Stand alone
This image contains [code-server](https://github.com/cdr/code-server), which is a browser based version of VS Code. To start it, run the following docker command:

`docker run -p 8000:8000 --rm lyxica/haskell-vscode-ide code-server` 

The VS code server will be accessible at http://127.0.0.1:8000

# Beginner FAQ
## How do I start a new empty project?
`cabal init -n --package-name=your_project-name`

### I got the error `Error: no package name provided.`
If you tried to run `cabal init -n` without specifiyng a package name, cabal will use the name of folder it's being run inside of as the package name. This error arrises when the package name that cabal is trying to use has been used already by another package on hackage.haskell.org

## How I add new libraries/modules?
Add 

## Stack? Cabal? Stackage... hackage...???
A wonderful explanation of both these tools is [Quick primer on Stack](https://www.vacationlabs.com/haskell/environment-setup.html#quick-primer-on-stack) and [Hackage vs Stackage & Cabal vs Stack](https://www.vacationlabs.com/haskell/environment-setup.html#hackage-vs-stackage-cabal-vs-stack)

# Recommended workflow
Don't compile!!! DON'T COMPILE!!! IT TAKES FOREVER.

The best way workflow at this level is to use GHC in it's REPL mode. To do this, open the terminal in Visual Studio Code (Ctrl+Shift+\`) and type in `cabal repl`. Cabal will then process your projects cabal file, resolving/downloading any missing libraries/modules your project depends on, and then load GHC as a REPL. 

Once you're in REPL mode (terminal says "Prelude >") you can then use the following commands:

:load filename.hs 
:r - reloads the last loaded file

Once you make a change, type in :r and then type in the function name you want to run.

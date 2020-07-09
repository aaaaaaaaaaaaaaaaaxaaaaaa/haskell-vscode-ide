# Whats this
This docker image contains, **out of the box**, the haskell compiler along with the Haskell IDE Engine (HIE) running on an ubuntu base image.

## Who is this for
This image is the result of my frustrations from trying to learn haskell. Haskell is a pretty cool language but the web of abandoned or *deprecated* projects combined with poor IDE tooling has made learning Haskell an 80/20 situation where 80% of the effort has been spent on tooling issues. To make things simpler, I've come up with this docker image. All that is required is Docker, WSL2 (for windows), and Visual Studio Code.

### What's installed?

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
  * The [Haskell Language Server](https://marketplace.visualstudio.com/items?itemName=alanz.vscode-hie-server) VSCode extension uses this to collect type data, lint data, etc
* gen-hie
  * Generate an hie.yaml file from a .cabal project file (optional)
* code-server
  * Server/browser based VSCode

# How to use
This image can be used in two ways: as a .devcontainer for VSCode, or as a standalone docker image

## .devcontainer
#### Info
Visual Studio Code with the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension lets you use docker images. Docker images contain a virtualized environment for your language/project. When VS code opens a folder it looks for a ".devcontainer" folder. When it finds a .devcontainer folder, it reads the configuration file. From this file, VSCode then builds a container from a specific image, exposes any specified ports, and installs any specified VSCode extensions. It then maps the folder containing the .devcontainer as a volume inside the docker container. 

#### How do I use it
Install the Remote Development extension, download the .devcontainer from this repo, drop it into whatever folder contains your haskell project. Reopen that folder in VSCode.

## Stand alone
This image contains [code-server](https://github.com/cdr/code-server), which is a browser based version of VS Code. To start it, run the following docker command in powershell:

`docker run -p 8000:8000 --rm -v ${PWD}:/workspace lyxica/haskell-vscode-ide code-server` 

It will mount the current directory at /workspace inside the container. The VS code server will be accessible at http://127.0.0.1:8000

# Beginner FAQ
## How do I start a new empty project?
`cabal init -n --package-name=your_project-name`

### I got the error `Error: no package name provided.`
If you tried to run `cabal init -n` without specifying a package name, cabal will use the name of the folder it's being run inside of as the package name. This error arises when the package name that cabal is trying to use has been used already by another package on hackage.haskell.org

## How do I add new libraries/modules?
Add the desired package name to your projects cabal file under the "build-depends" field. Restart your REPL using Cabal to resolve the changes.

e.g., let's say you wanted to add hex-text (process hex strings into bytestrings) to your new project, your cabal file would look like so:

```executable your_packages_name
  main-is:             Main.hs
  -- other-modules:
  -- other-extensions:
  build-depends:       base >=4.12, hex-text
  -- hs-source-dirs:
  default-language:    Haskell2010
```
## Stack? Cabal? Stackage... hackage...???
A wonderful explanation of both these tools is [Quick primer on Stack](https://www.vacationlabs.com/haskell/environment-setup.html#quick-primer-on-stack) and [Hackage vs Stackage & Cabal vs Stack](https://www.vacationlabs.com/haskell/environment-setup.html#hackage-vs-stackage-cabal-vs-stack)

# Recommended workflow
Don't compile! DON'T COMPILE!!! IT TAKES FOREVER. 

The best workflow at this level is to use GHC in it's REPL mode. To do this, open the terminal in VSCode (Ctrl+Shift+\`) and type in `cabal repl`. Cabal will then process your projects cabal file, resolving/downloading any missing libraries/modules your project depends on, and then load GHC as a REPL. 

Once you're in REPL mode (terminal says "Prelude >") you can then use the following commands:

* :load filename.hs 
* :r - reloads the last loaded file

Once you make a change, type in :r and then type in the function you want to run.

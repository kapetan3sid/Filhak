# RFP Proposal: Filhak - FVM Development Environment

**Name of Project:** Filhak 

**Proposal Category:** `devtools-libraries`

**Proposer:** @thedivic

Do you agree to open source all work you do on behalf of this RFP and dual-license under MIT, GPL, and APACHE2 licenses?: Yes

# Project Description

### Filhak 
(`>$ filhak)` is (*supposed to be*) a filecoin development environment heavily influenced by [Hardhat](https://hardhat.org/).

It allows FVM developers to:

- Easily launch a (stripped-down) local devnet Filecoin node (+FVM), with no setup required
- One or more **Go** packages that implement helper functions used in scripting **tasks** that are common to actor development (deploying and calling actors, creating & funding accounts, working with deals and the retrieval market etc)
- Task runner (similar to Hardhat Runner) that is used to run your Go tasks
- Tools and helpers to write and run **integration tests** **that interact** with the local **Filhak** devnet to test your actors.
- Deploy your custom actors to the testnet and mainnet.

### Motivation

After our research of the existing **EVM** tooling we realized how extensive and well rounded are SDKs, libraries and plugins. Even though there are some more "advanced" enviroments based on **WASM**, it was quite obvious to us why **EVM** is currently most utilized in blockchain and why it is so easy to build on it. 

Above all other **EVM** tools stand:

**Hardhat** - A one stop sandbox where developers are able to compile, deploy, test, and debug EVM smart contracts. 

We believe that sandbox similar to **Hardhat** would be highly valuable for developers that want to explore **FVM** and would greately ease the building process.

## Deliverables

For MVP version **Filhak** will consist of these functionalities:
1. Dev net automatization, stripped down lotus node
2. CLI tool - start dev net node, helper functions 
3. Support Filhak scrypts 
4. Test Helper packet
5. CLI test runner
6. Integrate existing actor libraries/actor templating

## Development Roadmap

Project development for **MVP** version will last from 1st June until 30th September (4 months) and will consist of one milestone. 
![My Remote Image](https://www.dropbox.com/s/k1pyerxvs3nj9ji/Capture.PNG?dl=0)

Team working on the project:
- Nikola Divic - System Architect
- Darko Brdareski - Go Developer
- Milos Sekuloski - Go Developer
- Katarina Milosavljevic - Project Manager
- Andreja Markovic - Product Owner

Funding requested: XXXXX USD 

### Filhak network

`>$ filhak node --port 8765`

**Filhak network** is a (stipped-down) local development network for Filecoin that works out of the box, with no setup required.

Every filhak task is executed inside the local filhak network.

The idea is to bootstrap and connect one lotus client and miner node and run them in the background.

Nodes should be headless and stripped down of all of the features that are unnecessary for local development but are required to operate a full lotus node, to improve performance and optimize the development experience. 

The network can be exposed for external JSON-RPC calls by running  `>$ filhak node` (if necessary).

⚠️  This assumes that lotus will integrate with FVM and at least built-in FVM actors (we can use a development version if it’s available, before launching to mainnet).

### Filhak Go package

Filhak provides one or more **Go packages** that implement common helper functions & data structures used in writing filhak tasks during actor development.

Stuff like:

- Creating and funding accounts
- Creating and sending transactions
- Creating and interacting with deals
- Working with the retrieval market
- Interacting with deployed (or built-in) actors
- Reading the (devnet) chain state tree
- etc

These helper functions should be optimized for **developer experience,** and hide underlying complexity of instantiating big Lotus data structures and making multiple API calls.

### Filhak init

`>$ filhak init [path]`

Initialized a filhak project in the given directory.

Creates a `filhak.toml` configuration file, sets up the default file and directory structure, fetches **Go** modules etc.

### Filhak scripts

`>$ filhak run script.go [--task=<name>]` 

Runs a script file on the local development network.

Given the task parameter, it executes a single task from the file.

Tasks are simple go functions:

```go
func DeployActorTask(ctx Context, fh *filhak.Network) { ... }
```

Positional parameters can be passed to tasks via the run command, and accessed inside the task code

```go
func FundAccountTask(ctx Context, fh *filhak.Network) {
	amount := ctx.args[0]
	addr := ctx.args[1]
  fh.Accounts.Fund(addr, amount)
}
```

### Filhak testing

Filhak uses `go test` to run tests and provides helper functions for interacting with the filhak network and the deployed and built-in actors.

It also provides a set of filecoin-specific assertions that can be used in your tests.

### Project Requirements

User programmability (3rd party actors) are required for filhak to reach it’s full potential. 

Until then, we can focus on:

- Development network automatization
- Implementing helper functions for currently available Filecoin features: creating and funding accounts, sending transactions, working with deals and the retrieval market through filhak scripts.
- Maybe implement a small set of tools for implementing new local hardcoded (built-in) actors to help other early builders in their PoC endeavors (I heard somebody’s building ERC-20 and ERC-721 token actors in Rust). These actors are (obviously) available only in the local devnet and can’t be deployed to testnet or mainnet until the networks are ready.

## Total Budget Requested
XXXXX USD

## Maintenance and Upgrade Plans

We plan on maintaining existing codebase as well as develop and integrate additional features such as: 

- Test existing Rust actors with **Filhak**  
- Deploy actors on Testnet and Mainnet
- Integrate VS Code extension for running tests
- Etc

⚠️ Some of these future features depend on FVM delivery.

Our end goal is to develop "Filecoin Simulation" similar to what **Ganache** is offering, where developers will be able to quickly fire up a personal **Filecoin** blockchain which they can use to run tests, execute commands, and inspect state while controlling how the chain operates.

# Team
## Contact Info

**On Filecoin slack:**

    @Nikola Divic
    @Andreja Markovic

## Team Members

    Nikola Divic: @TheDivic
    Milos Sekuloski: @milos1991
    Darko Brdareski: @brdji

## Team Member LinkedIn Profiles

[Nikola Divic](https://www.linkedin.com/in/thedivic/).

   [Milos Sekuloski](https://www.linkedin.com/in/milos-sekuloski-488a93166/).
   
   [Darko Brdareski](https://www.linkedin.com/in/darko-brdareski-b7463b63/).
   
   [Katarina Milosavljevic](https://www.linkedin.com/in/milosavljevicgkatarina/).
   
   [Andreja Markovic](https://www.linkedin.com/in/andreja-markovic-6704ba13b/).

## Team Website

https://bloxico.com/
## Relevant Experience 

Our team have extensive experience in regards to writting in **Go** as well as experiance with **Filecoin** technology. We have successfuly developed:
- **System Test Matrix** - Lotus Filecoin test coverage improvement and visualization dashboard.

And currently we are developing first phase of:
- **Testground** -  platform for testing, benchmarking, and simulating distributed and p2p systems at scale infrastructure and software revamp.

Furthermore, we followed the FVM development since its inception. We  participated in various **FVM AMA** sessions as well as recurring **FVM Early Builders Check In** where we indicated that we would like to to develop **Hardhat** inspired development enviroment for **FVM**
## Team code repositories

[fil-filhak](https://github.com/Bloxico/fil-filhak)

[fil-lotus](https://github.com/Bloxico/lotus)

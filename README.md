# RFP Proposal: Filhak - FVM Development Environment

**Name of Project:** Filhak 

**Proposal Category:** `devtools-libraries`

**Proposer:** `@thedivic`

Do you agree to open source all work you do on behalf of this RFP and dual-license under MIT, GPL, and APACHE2 licenses?: `Yes`

## Project Description

### Filhak 
(`>$ filhak)` is (*supposed to be*) a filecoin development environment heavily influenced by [Hardhat](https://hardhat.org/).

For MVP Filhak will allow FVM developers to:

- Easily launch a (stripped-down) local devnet Filecoin node (+FVM), with no setup required
- Use one or more **Go** packages that implement helper functions used in scripting **tasks** that are common to actor development (deploying and calling actors, creating & funding accounts, working with deals and the retrieval market etc)
- Use the task runner (similar to Hardhat Runner) that is used to run your Go tasks

### Motivation

After our research of the existing **EVM** tooling we realized how extensive and well rounded SDKs, libraries and plugins are. Even though there are some more "advanced" environments based on **WASM**, it was quite obvious to us why **EVM** is currently most utilized in blockchain and why it is so easy to build on it. 

Above all other **EVM** tools stand:

**Hardhat** - A one stop sandbox where developers are able to compile, deploy, test, and debug EVM smart contracts. 

We believe that sandbox similar to **Hardhat** would be highly valuable for developers that want to explore **FVM** and would greatly ease the building process.

## Features in-depth

### Filhak init

`>$ filhak init [path]`

Initializes a filhak project in the given directory.

Creates a `filhak.toml` configuration file, sets up the default file and directory structure, fetches **Go** modules etc.

### Filhak network

`>$ filhak node --port 8765`

**Filhak network** is a (stripped-down) local development network for Filecoin that works out of the box, with no setup required.

Every filhak task is executed inside the local filhak network.

The idea is to bootstrap and connect one lotus client and miner node and run them in the background.

Nodes should be headless and stripped down of all of the features that are unnecessary for local development but are required to operate a full lotus node, to improve performance and optimize the development experience. 

The network can be exposed for external JSON-RPC calls by running  `>$ filhak node` (if necessary).

⚠️  This assumes that lotus will integrate with FVM and at least built-in FVM actors (we can use a development version if it’s available, before launching to mainnet).

### Filhak Go package

`import "github.com/Bloxico/filhak/script"`

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

## Milestones and deliverables


### Milestone 1 - Filhak Network

**Deliverables:**

- `filhak` CLI tool
- project scaffolding and configuration 
- launch and interact with the development network via `filhak node` command

**Funding for this milestone:**

For M1 two full time **Go** developers rate will be $50 / hour in order to fund the milestone.

Team will be paid an estimated $17,500 per month, and $35,000 per milestone.

The estimated payment amount per month will be subject to change due to different wokring days in a month, public holidays, sick leaves, etc. 

Upon completion of M1, team will be paid $25,000. 

**Total budget for M1 - $60,000.**

**Estimated Milestone Delivery:**
First half of August 2022

### Milestone 2 - Filhak Scripting

**Deliverables:**

- run filhak scripts via the `filhak run script.go [--task=<name>]` command on the local devnet
- implement the `github.com/Bloxico/filhak/script` helper package
- interact with locally deployed actors in scripts using the helper functions

**Funding for this milestone:**

For M2 two full time **Go** developers rate will be $50 / hour in order to fund the milestone.

Team will be paid an estimated $17,500 per month, and $35,000 per milestone.

The estimated payment amount per month will be subject to change due to different wokring days in a month, public holidays, sick leaves, etc. 

Upon completion of M2, team will be paid $25,000. 

**Total budget for M2 - $60,000.**

**Estimated Milestone Delivery:**
First Half of October 2022
 
![](https://github.com/kapetan3sid/Filhak/blob/main/Capture.PNG)

## Maintenance and Upgrade Plans

We plan on maintaining our existing codebase as well as to develop and integrate additional features such as: 

**Filhak testing**

`import "github.com/Bloxico/filhak/testing"`

Filhak uses `go test` to run tests and provides helper functions for interacting with the filhak network and the deployed and built-in actors.

It also provides a set of filecoin-specific assertions that can be used in your tests.

**Filhak templates**

`>$ filhak template <template_name> [--name=<local_name>]` 

You can use filhak to instantiate actors from existing best-practice, reviewed actor templates implemented by other builders
or by PL developers (inspired by OpenZeppelin contracts).
Templated actors then can be further extended with additional business logic.

The repository that holds these contracts will be determined in sync with other early builders and PL developers and filhak will pull the templates from that repo and instantiate them locally.

**Filhak deployment**

Actor deployment on Testnet and Mainnet as soon as it's available.

**Filhak extensions and pluggins**

Implement a VS Code extension for filhak to improve the dev experience

⚠️ Some of these future features depend on FVM delivery.

🚀 **Moonshot**: Our end goal is to develop "Filecoin Simulation" similar to what **Ganache** is offering, where developers will be able to quickly fire up a personal **Filecoin** blockchain which they can use to run tests, execute commands, and inspect state while controlling how the chain operates. But this is a long-shot and way out of scope for this proposal.

## Project Requirements

- User programmability (3rd party actors) are required for filhak to reach it’s full potential. 
- Feedback from other early builders and PL developers is highly recommended, so we can improve the development experience of our framework

## Total Budget Requested

**M1:** 

$35,000 - Time and Material

$25,000 - Completion of M1

**M2:**

$35,000 - Time and Material

$25,000 - Completion of M2

**Total Budget: $120,000**

# Team
## Contact Info

**On Filecoin slack:**

    @Nikola Divic
    @Andreja Markovic

## Team Members

    Nikola Divic: @TheDivic
    Bojan Milinkovic: @bokimilinkovic 
    Katarina Milosavljevic
    Andreja Markovic

## Team Member LinkedIn Profiles

[Nikola Divic](https://www.linkedin.com/in/thedivic/).

   [Bojan Milinkovic](https://www.linkedin.com/in/bojan-milinkovic-169780151/).
   
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

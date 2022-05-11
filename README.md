# RFP Proposal: Filhak - FVM Development Environment

Name of Project: FVM - High level Rust SDK

Link to RFP: Please link to the RFP that you are submitting a proposal for.

RFP Category: devtools-libraries

Proposer: @thedivic

Do you agree to open source all work you do on behalf of this RFP and dual-license under MIT, GPL, and APACHE2 licenses?: Yes
Project Description
References

In the context of the FVM Early Builder program, @BlocksOnAChain suggested that we could handle the creation of a High level Rust SDK to help with the development of Rust-native actors for the FVM.

The following specifications are meant to produce a Rust crate that could be the only fvm_* crate-related import to produce valid code. The different specifications have multiple references:

    FVM spec: architecture
    @raulk example: Hello world actor example
    @jimpick experiment: Hanoi actor
    Current low-level sdk

Ideal behaviours

From those sources came out some ideal features to implement:
From current actors

    Remove the need for the user to code the invoke function
    Remove the need for the user to handle serde of payloads
    Ease State structure declaration in actors and auto generate save() and load() functions
    Some low-level SDK syscall should be available to the user if they are deemed interesting (crypto ...)
    Have access to basic types that could be useful for actor development

From other languages

    Have access to block & tx properties
    Implement error handling function such as assert_* for the user to use in their contract
    Have access to the deployed actor information (address ...)

From specifications

    potentially mapping dynamically-linked libraries (e.g. predefined SDK versions)

    Means that the SDK core module shall not be within the actor base code but linked to it. Reduce actors’ size.

TBC Draft Specifications

Note: These are the proposed implementation choices to be developed. It is still to be discussed before considering a roadmap.
Glue code generation and internal dispatch
Ideal lifecycle of in a called actor

Note: Each steps are either developer code or SDK generated.

    invoke() , match proper function - SDK generated
    Deserialize received parameters to Rust types - SDK generated
    Call function - SDK generated
    <state>.load() - Dev code
    Run function code - Dev code
    <state>.save() - Dev code
    Insert the return data block if necessary, and return the correct block id - SDK generated

Mandatory elements in an actor

Entry point

    invoke() : entry point for the actor, contains a map to dispatch call to proper method based on its number.

State

    Struct that derives Serialize_tuple and Deserialize_tuple
    Should have a save() and load() implementation from a trait. Proposal for StateObject trait.

Proposed implementation

This is a part where it is still kind of blurry tome. I guess that both @raulk and @BlocksOnAChain could help. I was thinking about implementing a procedural macro to generate glue code needed for an actor to work as I previously stated.

Nonetheless, there are a few problems that I can not get my mind around:

    If we generate invoke() then how can the user and future callers know which function are associated with which index ?
    I was thinking of having something that would look like that as an actor:

use fvm_sdk::{fvm_actor};

#[fvm_actor]
struct State {
    ...
}

#[fvm_actor]
Impl State {
    ...
}

to generate glue code for structures (#[derive(Serialize_tuple, Deserialize_tuple, StatObject)]) and for its implementation (invoke(), deserialization of message parameters before calling matched function and serialization of the return of the called function).

But it would mean that all types as parameters and/or returned from functions should implement Serialize_tuple and/or Deserialize_tuple otherwise generated code would not work. Is it alright? Should we do it another way?

    Having glue code generated means heavier actor bytecode on first deployment and more gas spent at runtime. Is it tolerable ?

Extension of low level SDK
Access basic types

Access basic types (Address , TokenAmount ...) that are available in the fvm_shared crate
Call other actor on the chain

    Extend the access to the send() syscall already available.
    Deploy new actor
        @BlocksOnAChain @raulk I am currently wondering if there is currently a way to deploy actor from one already deployed (a Factory use case). Would you have more information on that?

Access network, message and actor information

Available information
Note: Syscall to be extended so that the developer can access it

    Current actor
        root() : current root of IPLD state
        current_balance() : self-explanatory
    Network
        curr_epoch() : current chain epoch
            Note: Should sanitize function naming
        version() : current network version
        base_fee() : current base fee
        total_fil_circ_supply() : total fil supply
    Message
        caller() : actor id of the caller
        receiver() : actor id of the receiver
        method_number() : method number that was called with the message
        params_raw(id: BlockId) : returns the message codec and parameters
        value_received() : value received in AttoFil

Complementary work

    Add origin() that allows to get the actor ID as originated the transaction and not the message.
    Add a way to check for another actor balance
        @BlocksOnAChain @raulk Could you confirm that it does not exist ? Could not find anything related to current ref-fvm/fvm_sdk or ref-fvm/fvm_shared. Also I do not see an easy way of doing so without adding another syscall so that might have to be discussed and maybe excluded of the scope.

Error handling

** Add assert_* macros

    Useful macro for the user to panic based on conditions.
    Need to also export an abort macro. Available in low-level SDK.

TODO Deliverables

Please describe in details what your final deliverable for this project will be.
TODO Development Roadmap

Please break up your development work into a clear set of milestones. You can use the milestones suggested in the RFP or create your own. This section needs to be very detailed (will vary on the project, but aim for around 2 pages for this section).

For each milestone, please describe:

    The software functionality that we can expect after the completion of each milestone. This should be detailed enough that it can be used to ensure that the software meets the RFP scope.
    How many people will be working on each milestone and their roles
    The amount of funding required for each milestone
    How much time this milestone will take to achieve (using real dates)

TODO Total Budget Requested

Sum up the total requested budget across all milestones, and include that figure here. Ensure that it does not exceed the total funding limit on the RFP.
Maintenance and Upgrade Plans

We have no plan on maintaining the SDK in this proposal. This should be arranged either at a later time or in another program.
Team
Contact Info

On Filecoin slack:

    @philippe Métais
    @thomas

Team Members

    Philippe Métais: @PhilippeMts
    Thomas Chataigner: @tchataigner

Team Member LinkedIn Profiles

    Thomas Chataigner:

Team Website

https://polyphene.io
Relevant Experience

We already have some experience in handling wasm runtime and development of tooling around it. We spent last year designing and building the Holium project. It allowed us to hone our skills to build rust-based protocols and libraries.

Moreover, we followed the FVM specification and development since its premises. We also participated in its development by helping on the creation of an integration test framework.
Team code repositories

Holium Rust

Holium Rust SDK

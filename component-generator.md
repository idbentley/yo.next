# Component Generator Proposal

Re-define generator:

 * Describe the installation process of one component.
 * Explicitly state all the places that they will touch files as resources
   rather than paths.
 * Explicitly define all the grunt tasks that they effect.
 * Explicitly expose any state that reliant generators may need.
 * Explicitly list npm, bower dependencies.
 * Explicitly list state that this generator may need.

Resources:

 * An abstraction around a path.
 * Should be defined as state by scaffolding generators.

Benefits:

 * Useful generators are simply compositions of a bunch of different generators
 * Can determine based upon generator specifications whether a given sequence of
   generators resolves into a workable generator
 * Can automatically build hierarchies of generators with dependencies that
   allows yo to install a complex set of generators based off of a simple spec

Oustanding Questions:

 * How does grunt task resolution work?
 * What does a generator specification actually look like?
 * How is state exposed to future generators at yo installation time.
 * Should generator order of application matter 

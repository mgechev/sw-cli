# Service Workers CLI Tool
* Status: Draft
* Authors: Minko Gechev (mgechev@gmail.com)

## Objective
Build a CLI tool, which accepts command line parameters and/or reads a JSON config file and generates service workers code
Implement different strategies for:
* Assets caching
* Model caching
* Background sync?

## Background

## Prior Art
Since there are a lot of common patterns when using service workers, most of the code for them could be automatically generated. Later the user can do some application specific customizations.

## Detailed Design
### Overview:
The CLI tool should have the following modules:
* Configuration generator
* Service Worker's code generator
* Code templates, which implement the main service workers' strategies

The code templates should contain placeholders for the resources which should be handled by the service workers.

### Config generation:
The tool should provide interactive configuration helper, similar to `karma init`, which asks the user for the following configuration details:
* Scope of the service worker
* Service Workers' strategy to be used (cache only, network only, cache and falling back to network, etc.)
* List of assets to be cached
* Fallback resources, if required by the strategy
* Output directory

### Code generation
Once configured the tool can generate the service worker by filling the placeholders in the template. Which code template will be used depends completely on the chosen strategy.

### High-level Flow

On the following sequence diagram is shown the communication between the different high-level modules of the generator. During development it is highly possible changes to be introduced:

![](/assets/sw-high-level.png)

## Caveats

## Security Considerations
I don't believe there are any security considerations.

## Performance Considerations / Test Strategy
The CLI tool could be tested with any testing framework (for example jasmine and its node binding).
We can test the service workers' code through karma with different browsers.

## Work Breakdown
# Service Workers CLI Tool
* *Status: Draft*
* *Authors: Minko Gechev (mgechev@gmail.com)*

## Objective
Build a CLI tool, which accepts command line parameters and/or reads a JSON config file and generates service workers code
Implement different strategies for:
* Assets caching
* Model caching
* Background sync?

## Motivation
Jake Archibald introduced a few [common patterns](http://jakearchibald.com/2014/offline-cookbook/) for using service workers. Since most applications will use service workers in quite similar fashion, around these patterns, there are going to be a lot of boilerplates. In order to handle the code duplication most of the code could be generated automatically. Later, the user can do further customizations if required.

## Prior Art

## Detailed Design
### Overview:
The CLI tool should have the following high-level modules:
* Configuration generator
* Service Worker's code generator
* Code templates, which implement the main service workers' [strategies](http://jakearchibald.com/2014/offline-cookbook/)

The code templates should contain placeholders for the resources which should be handled by the service worker.

### Config generation:
The tool should provide interactive configuration helper, similar to `karma init`, which asks the user for the following configuration details:
* Scope of the service worker
* Service Workers' strategy to be used (cache only, network only, cache and falling back to network, etc.)
* List of assets to be cached
* Fallback resources, if required by the strategy
* Output directory

The configuration options depend on the chosen strategy, which means that the initializer might need to plug strategy specific config generators run-time.

### Code generation
Once configured the tool can generate the service worker by filling the placeholders in the template. Which code template will be used depends on the chosen strategy during initialization.

### High-Level Flow

On the following sequence diagram is shown the communication between the different high-level modules of the generator. During development it is highly possible changes to be introduced:

![](/assets/sw-high-level.png)

## Caveats

## Security Considerations
I don't believe there are any security considerations.

## Performance Considerations / Test Strategy
The CLI tool could be tested with any testing framework (for example jasmine and its node binding).
We can test the service workers' code through karma with different browsers.

## Work Breakdown
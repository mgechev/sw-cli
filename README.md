# Service Workers CLI Tool
* *Status: Draft*
* *Authors: Minko Gechev (mgechev@gmail.com)*

## Objective
Build a configurable CLI tool, which generates the code for service workers.
Implement different strategies for:
* Assets loading interception (caching, disk/network race, etc.)
* Model loading interception
* Background sync?

## Motivation
Jake Archibald introduced a few [common patterns](http://jakearchibald.com/2014/offline-cookbook/) for using service workers. Since most applications will use service workers in quite similar fashion, around these patterns, there are going to be a lot of boilerplates. In order to handle the code duplication most of the code could be generated automatically. Later, the user can do further customizations if required.

## Prior Art
Not aware of existing tools with similar functionality

## Detailed Design
### Overview:
The CLI tool should have the following high-level modules:
* Configuration generator (called `Initializer` below)
* Service Worker's code generator (called `CodeGenerator` below)
* Code templates, which implement the main service workers' [strategies](http://jakearchibald.com/2014/offline-cookbook/)

The code templates should contain placeholders for the resources, which should be handled by the service worker.

### Config generation:
The tool should provide interactive configuration helper, similar to `karma init`, which asks the user for the following configuration details:
* Scope of the service worker
* Service Workers' strategy to be used (cache only, network only, cache and falling back to network, etc.)
* List of assets to be cached (relative/absolute paths with wildcards)
* Fallback resources, if required by the strategy
* Output directory

The configuration options depend on the chosen strategy, which means that the initializer might need to plug strategy specific config generator run-time.

Since the user may want to implement different caching strategies for different resources in his application, the tool's configuration should be a list of the options above.

### Code generation
Once configured the tool can generate the service worker by filling the placeholders in the template. The code template which will be used depends on the strategy in the configuration file.

Since multiple service workers' strategies could be chosen, in order to generate code for all of them we have the following options:
- Generate separate service workers with different scope
  - **Cons** Easier for implementation
  - **Pros** Limiting since we may want to apply different strategies for resources in the same scope
- Generate a single service workers with the different strategies implemented
  - **Cons** Eventually a bit more complex for implementation
  - **Pros** Much more flexible. We can have different strategies for the resources in the same scope

### High-Level Flow
On the following sequence diagram shows the communication between the different high-level modules of the generator. During development it is highly possible changes to be introduced:

![](/assets/sw-high-level.png)

## Caveats

Suggested strategies for implementation (only the titles are listed here, for further information see ["The offline cookbook"](http://jakearchibald.com/2014/offline-cookbook/):

### Caching strategies
* On install - as a dependency
* On install - not as a dependency
* On network response
* Stale-while-revalidate

### Serving strategies
* Cache only
* Network only
* Cache, falling back to network
* Cache then network
* Generic fallback
* **Cache & network race - it could be used in rare cases, mostly low end mobile devices**

The CLI tool should support generation of service workers based on different strategies since the different resources in the app might have different requirements.

## Security Considerations
I don't believe there are any security considerations.

## Performance Considerations / Test Strategy
The CLI tool could be tested with any testing framework (for example jasmine and its node binding).
We can test the service workers' code through karma with different browsers.

## Work Breakdown
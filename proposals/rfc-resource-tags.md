# Resource Tags

| Status        | Proposed                                               |
:-------------- |:------------------------------------------------------ |
| **RFC #**     | [0000](https://github.com/CRModders/rfcs/pull/0000)    |
| **Author(s)** | DarkenLM                                               |
| **Updated**   | 2023-03-25                                             |

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in [IEEE RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

## Objective
This proposal aims to implement a Resource Tag API, as a means to provide interoperability and compatibility between
mods powered by Fabric/Quilt for Cosmic Reach.  
It intends to provide a universal API to allow for the same kind of Element added by different mods to be used 
interchangably.  
It does not intend to provide a standard way to create and initialize any kind of Element for mods, such as Builders,
Templates or Abstract Classes.

## Motivation
Mod compatibility has always been a big problem within modding communities, especially within sandbox games, which provide
a great degree of freedom for both users and mod developers. One of the greatest sources of incompatibility is item 
redefinition. Multiple mods might implement their own version of the same item (e.g., copper ingot), which would not be 
able to interact with each other, even though they are seen as the same item by users.

A good example of this problem exists within the Minecraft modding community. During the early days of the Forge mod
loader, many mod developers reached the conclusion that multiple mods implemented the same kind of items, and when said
mods were used together, each implemented their own items, which would not work with each other's tools and equipment.
A solution often employed was to manually add compatibility for every mod that implemented a certain kind of item, but 
such was deemed unmaintainable. Eventually, Forge created the Ore Dictionary API, which would group items added
by different sources, allowing compatibility by default, given the mod developers used the tags correctly.

The Ore Dictionary was, however, too limited to apply to every face of mod development, such as compatibility between 
machines added by technical mods, blocks placed in the world, or energy systems. The latter was especially problematic, 
as when Forge implemented the Forge Energy API, most mods used the Redstone Flux API, and the change brought along many 
problems, especially with outdated mods that did not update to the new system, leaving mod developers on the fence on 
whether to use the standard system, which was new and unstable or use the battle-tested and wildly adopted RF system.

## User Benefit
By allowing multiple mods to create the same kind of Elements, the user would not have to worry about which kind of 
Element is compatible with each mod's equipment, as such compatibility will be automatically provided by the loader 
itself. In the case where a user uses each mod separately, the mod would still retain its items, remaining 
feature-complete both when isolated and when used together with other mods.

Mod developers would also benefit from this API, as compatibility would be ensured by the API, allowing the developer to
focus on the actual functionality of their mod, instead of having to cut features due to the fear of incompatibility.

## Design Proposal
This document proposes a data-oriented API to define Resource Tags for multiple Elements of a mod. Resource Tags 
provide a way for mods to define interchangeable Elements referenceable by any other mods without friction.

To ensure the usage of the correct Resource Tags from the Mod Developers, certain APIs SHOULD be locked behind specific 
Resource Tags, and implement interfaces tied to those Resource Tags and prevent the mod from being able to access those 
APIs outside of the Elements that have declared such Resource Tags (e.g.: An Element can only access the Energy API if it
declared the `global:energy` Resource Tag).

### What is a Resource Tag?
A Resource Tag MUST be an Identifier that represents a tag. A Resource Tag MUST be a string with the format 
`<namespace>:<tag>`, where `<namespace>` MUST be either a Mod ID or a [Reserved Namespace](#reserved-namespaces), and 
`<tag>` MUST be an Identifier that uniquely identifies an Element kind. A Resource Tag MUST be registered as a Resource 
Tag File present on a Resource Tag Directory that corresponds to the kind of Resource it is representing.

### What is an Identifier?
An identifier MUST be a printable, ASCII-encoded string that uniquely identifies a Resource globally within the Modding 
Environment of a given game instance.

### What is a Resource Tag Directory?
A Resource Tag Directory is an asset directory namespaced to a given Mod ID that contains all Resource Tags 
registered by a given Mod. A Resource Tag Directory is a directory that follows the format `assets/<modid>/tags/<kind>`,
where `<modid>` SHOULD be the Mod ID of the mod registering the resources and `<kind>` SHOULD be the kind of resource 
the Resource Tag Files contained inside are registering.  
An example Resource Tag Directory structure is presented below:
```
\ assets
    \ examplemod
        \ tags
            |- block
                \ copper_block.json
            |- item
                \ copper_ingot.json
            |- fluid
                \ liquid_copper.json
            |- entity
                \ example_entity.json
```

### What is a Resource Tag File?
A Resource Tag File is a specification-compliant JSON file that contains the Tags for a given Resource, using the 
following format:
```json
{
    "tags": []
}
```
Where `tags` is an array of Resource Tags.

### Reserved Namespaces
There are several namespaces that, by convention, MUST NOT be used to add Tags other than the ones already defined:
- `base`: The `base` namespace is used internally by Cosmic Reach itself to tag its Elements.
- `global`: The `global` namespace is used by the API to represent commonly used kinds of Elements. It is defined 
internally and MAY be extended by a future rectification for this proposal or superseded by a newer proposal.

### Well-known Resource Tags
Well-known Resource Tags are `global` Tags that are commonly used on Mods, and as such are globally defined for ease of 
targeting and integration for multiple mods. Enumerated below is a non-extensive list containing some Well-known Tags:
- `global:item`: Defines an Item.
- `global:block`: Defines a Block.
- `global:machine`: Defines an Element that may be treated as a machine.
- `global:energy`: Defines an Element that exposes interfaces for the Energy API (not implemented as of the last update 
to this proposal)
- `global:fluid`: Defines an Element that exposes interfaces for the Fluid API (not implemented as of the last update 
to this proposal).
- `global:inventory`: Defines an Element that exposes interfaces for the Inventory API (implemented on the base game)


## Performance Implications
No significant performance degradation is expected with the introduction of this API. Most of the impact is expected to 
happen at loading time, as the Resource Tags should be sourced and loaded as Mods are loaded into the Modding 
Environment.

To benchmark the proposed API, the usage of a profiler on the same Modding Environment changing only the loader, 
testing both with and without the proposed API, was considered to measure loading times and resources used during the 
startup phase of the Mod Loader. Preliminary testing concluded that this approach provides insufficient data for 
accurate benchmarks using the tools available during said tests, more specifically, the 
[Java JFR Profiler](https://plugins.jetbrains.com/plugin/20937-java-jfr-profiler) for IntelliJ IDEA Community Edition 
2023.3.4.

## Dependencies
This proposal has no dependencies.

## Engineering Impact
This proposal SHOULD be maintained by the contributors to the current *de facto* API. As of the last update to this 
proposal, the *de facto* API is the [FluxAPI](https://github.com/CRModders/FluxAPI). The author of this proposal also 
offers to implement this design, should it be accepted.

No breaking changes are required to FluxAPI or any loader as of the last update to this proposal.

## Best Practices
This proposal introduces a new set of best practices for developing a Mod within the CRM Modding Community, detailed 
below.

- Mods SHOULD declare only Resources associated with themselves, using their Mod ID to uniquely differentiate their Tags
within the larger Modding Environment (e.g. `examplemod:copper` instead of `materials:copper`).
- Mods MUST declare at least one [Well-known Element Tag](#well-known-resource-tags) for all Resources that apply.
- Mods MUST NOT add, modify, or remove Tags from [Reserved Namespaces](#reserved-namespaces).
- Mods SHOULD use Resource Tags for Elements accessible by a User to ensure compatibility with other Mods.
- Mods MAY choose not to implement Resource Tags on internal Elements that are not intended to be used by any third 
party, including the User and other Mod Developers.

## Tutorials and Examples
This is a broad, incomplete, and non-functional code snippet with the aim of providing an example of the usage of the 
proposed API. It is subject to substantial changes during the RFC discussion phase and might suffer complete redesigns. 
The current PoC is one of multiple iterations on the design of the API, detailed on 
[Alternatives Considered](#alternatives-considered).

The current iteration was designed to have the Resource Tags defined using the data-oriented API 
and sourced by the logic-oriented API from the former. It would involve the usage of two classes, `ResourceTags` and 
`ResourceTag`.

The `ResourceTags` class would contain helpers to source and manipulate sets of Tags within the codebase. When using the 
static method `source` at runtime, the mod would attempt to source the Resource Tags from a given Resource Tag Directory. 
It MUST crash with a descriptive error message that allows for easy identification of the problem.

The `ResourceTag` class would provide an easy way to reference Resource Tags within the code, providing safety for 
eventual breaking changes. The static method `create` would create an Identifier that correctly identifies a Resource 
Tag defined using the data-oriented API.

As of the last update to this proposal, the usage of FluxAPI's Registries for reading and registering the Resource Tags 
was discussed. However, no PoC was implemented or discussed.

```java
class ModResourceTags {
    public static final ResourceTag COPPER = ResourceTag.create(MODID, "copper"); // "modid:copper"
}
```

```java
class CopperIngotItem extends Item {
    public CopperIngotItem() {
        super(ResourceTags.source(MODID, "items/copper_ingot")) // Would fetch tags from "assets/modid/tags/items/copper_ingot.json"
    }
}
```

Below is an example of how Resource Tags could be used within the game. In this case, a specific interaction would only 
be allowed if the player holds an item with a specific Resource Tag.
```java
class CopperBlock extends Block {
    public void onInteract(World world, Player player, BlockState blockState, BlockPosition position) {
        ItemStack selectedItem = Player.hotbar.getSelectedItem();
        ResourceTags selectedItemTags = selectedItem.item.tags;

        // Check if the player is holding a copper ingot, from any mod
        if (selectedItemTags.contains(ModResourceTags.COPPER)) {
            // Execute some special interaction.
        }
    }
}
```

## Alternatives Considered
While designing an initial logic-oriented interface for the Resource Tags, multiple design choices were considered and 
discussed with members of the CRM community before landing on a candidate that was considered the most promising. The 
iterations of the designs are listed and described below.

### First Iteration
The first iteration was designed with the ability to define the Resource Tags exclusively from a logic standpoint. At 
this point, the data-oriented API was not considered due to forgetfulness during the design phase. It would involve the 
usage of a class `ResourceTag` and an annotation `ResourceTags`. 

The `ResourceTag` class was supposed to contain helpers to create and manipulate Tags within the codebase. When 
initialized, it would define a Resource Tag that would uniquely identify that resource within the Modding Environment.  
The `ResourceTags` annotation was supposed to inject a private field containing the declared Resource Tags, which would
be accessed through the `ResourceTag` class.

At this point, no way of registering the Tags on the global Modding Environment had been considered.

```java
class ModResourceTags {
    public static final ResourceTag COPPER = new ResourceTag(MODID, "copper");
}
```

```java
@ResourceTags(
    tags = {
        @GlobalTag(GlobalTag.INGOT),
        ModResourceTags.COPPER,
    }
) // Tags would be ["global:ingot", "modid:copper"]
class CopperIngotItem extends Item {

}
```

### Second Iteration
The second iteration was designed also with the ability to define the Resource Tags exclusively from a logic standpoint.
For the same reasons as the previous iteration, the data-oriented API was once again not considered. This design would 
involve the usage of a single class `ResourceTags`.

The `ResourceTag` class was supposed to contain not only helpers to create and manipulate Tags within the codebase, but 
also a set of well-known and commonly used global Resource Tag kinds (such as `ingot`, `ore`, `food`, `tool`, etc).

At this point, no way of registering the Tags on the global Modding Environment had been considered.

```java
class ModResourceTags {
    public static final ResourceTag COPPER = new ResourceTag(MODID, "copper");
}
```

```java
class CopperIngotItem extends Item {
    ResourceTags tags = ResourceTags.create(ResourceTags.GLOBAL.INGOT, ModResourceTags.COPPER); // Tags would be ["global:ingot", "modid:copper"]
}
```

## Compatibility
As of the last update to this proposal, this proposal would not introduce any incompatibilities or breaking changes to any
existing mod, given the Modding Environment is still under active development and still lacks support for more complex 
systems that are present on Mods that are considered more advanced (e.g technical Mods from Minecraft, like Mekanism, 
Immersive Engineering, etc).

## User Impact
As of the last update to this proposal, the implementation of this proposal would go unnoticed, due to the reasons 
explained in the [Compatibility Section](#compatibility).

## Questions and Discussion Topics
Before a review request, the following questions are presented by the author to be discussed:

1. Is the current design enough to define a complete specification? If not, what is missing?
2. Is the current logic-oriented interface design simple and easy to use?
  - 2.1. Are any of the previous iterations of the design better than the current one?
3. Are the current [Reserved Namespaces](#reserved-namespaces) needed, or are they unnecessary? 
4. Should [Well-known Resource Tags](#well-known-resource-tags) be as restrictive as they are currently specified, or 
should they be more lax on their usage?

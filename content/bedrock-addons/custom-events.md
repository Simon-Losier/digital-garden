+++
title = 'Make an interactable block with custom components'
date = 2024-04-22T22:43:21-03:00
draft = false
author = 'Simon'
+++

NOTE: This guide was made for Minecraft Bedrock Preview 1.21.0.22, this will not work in 1.20

## Requirements
- Minecraft preview 1.21 or higher
- Have a block created

## Step 1: Register custom component
Custom compoenent registration can only be done in `world.beforeEvents.worldInitialize' and will only take into effect in a world restart

```ts
world.beforeEvents.worldInitialize.subscribe((eventData) => {
    eventData.blockTypeRegistry.registerCustomComponent(
        'example:customEvent', // Name of custom event, this will go in the block JSON later
        new onCustomEvent(); // Custom event binding object
};
```
## Step 2: Bind component
The next step is to bind the component to the event, there is a list of events [here](https://learn.microsoft.com/en-us/minecraft/creator/documents/customcomponents?view=minecraft-bedrock-stable#roadmap)

```ts
export class onCustomEvent implements BlockCustomComponent {
    constructor() {
        this.onPlayerInteract = this.onPlayerInteract.bind(this); // Event type
    }
    onPlayerInteract(eventData: BlockComponentPlayerInteractEvent) {
        world.sendMessage("Block interacted");  // Event logic here
    }
}

```

## Step 3: Add custom component to block
Once the event created, to add the custom component to the block, add to the block components the `minecraft:custom_component`feild with an array of custom component strings. 
```json
{
    "minecraft:block": {
        "components": {
            "minecraft:custom_components": ["example:customEvent"]
        }
    }
}

```

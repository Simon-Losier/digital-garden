+++
title = 'Creating a score HUD with JSON UI'
date = 2024-04-23T07:06:54-03:00
draft = false
author = 'Simon'
+++

## Step 1: Create Score UI Element
Create a hud object in the `RP/ui/hud_screen.json` file
``` JSON
{
    "namespace": "hud",
    "hud_text": {
        "type": "label",
        "text": "#text",
        "anchor_from": "top_left",
        "anchor_to": "top_left",
        "offset": [ 4, 4 ],
        "$update_string": "update",
        "controls": [
            {
                "data_control": {
                    "type": "panel",
                    "size": [ 0, 0 ],
                    "bindings": [
                        {
                            "binding_name": "#hud_title_text_string"      // reads in the current title string
                        },
                        {
                            "binding_name": "#hud_title_text_string",
                            "binding_name_override": "#preserved_text",   // updates #preserved_text when visibility of this element changes
                            "binding_condition": "visibility_changed"
                        },
                        // element becomes visible then immediately turns invisible when a title containing the update string is passed
                        {
                            "binding_type": "view",
                            "source_property_name": "(not (#hud_title_text_string = #preserved_text) and not ((#hud_title_text_string - $update_string) = #hud_title_text_string))",
                            "target_property_name": "#visible"
                        }
                    ]
                }
            }
        ],
        "bindings": [
            {
                "binding_type": "view",
                "source_control_name": "data_control",                          // reads bindings from the "data_control" child element
                "source_property_name": "(#preserved_text - $update_string)",   // remove string update text from the text to be displayed
                "target_property_name": "#text"
            }
        ]
    },
    "root_panel": {
        "modifications": [
            {
                "array_name": "controls",
                "operation": "insert_front",
                "value": [
                    { "hud_text@hud.hud_text": {} }
                ]
            }
        ]
    }
    
}

```

## Step 2: Update UI element programmatically
### Title Command
#### Making title invisible:
```
/title @s times 0 0 0
```

#### Sending the title

```
/title @s title updateScore: 7
```

### Sending title programmatically
```ts
player.onScreenDisplay.setTitle("updateScore: 7", {
    stayDuration: 0,
    fadeInDuration: 0,
    fadeOutDuration: 0
});
```

# Resources modding

## Resources

The resources used by the game are found in /Hearts of Iron IV/common/resources/00_resources.txt.

The resource file follows this format:

```
resources = {
     = {
        icon_frame =
        cic =
        convoys =
    }
}
```

### Icon frame

Icon frame controls which frame from the resource image strip is used for the resource icon. A 'frame' is the 27 by 27 pixel square the icon occupies in the image.

The resource image strip is defined by the **GFX_resources_strip** spritetype, which points to /Hearts of Iron IV/gfx/interface/resources_strip.dds by default. By default 1 will refer to the Oil icon, 2 to Aluminium, etc.

The icons are defined in /Hearts of Iron IV/interface/general_stuff.gfx. Their definition must be changed if you're adding or removing resources:

```
spriteType = {
		name = "GFX_resources_strip"
		texturefile = "gfx/interface/resources_strip.dds"
		noOfFrames = X #X being the number of resources you have,
	}

	spriteType = {
		name = "GFX_missing_resources_strip"
		texturefile = "gfx/interface/missing_resources_strip.dds"
		noOfFrames = X
	}
```

The number of frames strictly corresponds to the amount of resources.

### CIC

CIC defines the amount of resources needed to trade for 1 Civilian Factory. By default, this is 0.125, meaning 8 units of that resource are traded for 1 factory. Value can not be larger than 1.

### Convoys

Convoys controls the maximum amount of this resource a single convoy carries. By default, this is 0.1, meaning a convoy can carry 10 of the resource.

## Localization

Localisation is defined in /Hearts of Iron IV/localisation/production_l_<language>.yml with the following localization keys:

```
PRODUCTION_MATERIALS_:0 "Name of resource"
_desc:0 "Description of resource"
```

## Interface

If implementing different resources, you will need to edit the interface so the game understands how to display the new resource. /Hearts of Iron IV/interface/countryproductionlineview.gui is the relevant file here.

Each resource is utilised two GUI elements: *<resource>_icon* and *<resource>_value*, which are found under the **resources** windowtype.

```
buttonType = {
    name = "_icon"
    position = { x=0 y=2 }
    spriteType = "GFX_resources_strip"
    frame = 1 # Which icon is used from the spriteType image referred to above.
}

instantTextboxType = {
    name = "_value"
    position = { x = 31 y = 5 }
    textureFile = ""
    font = "hoi_18mbs"
    borderSize = {x = 0 y = 0}
    text = "999"
    maxWidth = 50
    maxHeight = 20
    format = left
}
```

### Additional notes

A fuel can also correspond to the fuel dynamic, however only one resource at a time can do that. This is decided by the FUEL_RESOURCE [define](/Defines).

Infrastructure's bonus on resource gain is decided by the INFRASTRUCTURE_RESOURCE_BONUS define and applies to all resources the same.

**[Modding](/Modding)**
# Posteffect modding

Posteffects are color correction effects that are applied to the lighting in certain areas (volumes) on the map map. They are used to tint the lighting for areas where the lighting would be noticable different, i.e. deserts.

All posteffects are found in /Hearts of Iron IV/gfx/posteffect_volumes.txt.

## Posteffect Values

A posteffect values entry follows this format:

```
posteffect_values = {
    name =
    inherit =

    lut =


    hdr_min_adjustment =
    hdr_max_adjustment =

    BLOOM_WIDTH =
    BLOOM_SCALE =
    BRIGHT_THRESHOLD =

    tonemap_middlegrey =

}
```

**name** is the name of the posteffect values entry.

**inherit** causes the values for the current posteffect to be inherited from the specified posteffect.

**lut** is the path for the tone map used for this entry.

**hdr_min_adjustment** defines the minimum adjustment used in the HDR effect.

**hdr_max_adjustment** defines the maximum adjustment used in the HDR effect.

**BLOOM_WIDTH** defines the width of the bloom effect.

**BLOOM_SCALE** defines the scale of the bloom effect.

**BRIGHT_THRESHOLD** defines the threshold of the bloom effect.

**tonemap_middlegrey** defines the middle grey used in the tone map.

## Volumes

### Position Volume

A volume entry specifies an area on the map in which posteffect values are applied. It follows this format:

```
posteffect_volumes = {
    posteffect_volume = {
        name =
        posteffect_values_day =
        posteffect_values_night =
        posteffect_values_day_winter =
        posteffect_values_night_winter =

        position = {


        }
        size = {


        }
        fade_distance =
    }
}
```

**name** is the name of the volume entry.

**posteffect_values_day** is the posteffect values entry to use during the day.

**posteffect_values_night** is the posteffect values entry to use during the night.

**posteffect_values_day_winter** is the posteffect values entry to use during the day during winter months.

**posteffect_values_night_winter** is the posteffect values entry to use during the night during winter months.

**position** defines the location of the volume center.

**size** defines the size of the volume box.

**fade_distance** defines the fade distance for the posteffect values.

### Height Volume

A height volume entry specifies a height at which to apply posteffect values. It follows this format:

```
posteffect_volumes = {
    posteffect_height_volume = {
        name =
        posteffect_values_day =
        posteffect_values_night =

        height =
        fade_distance =
    }
}
```

**name** is the name of the volume entry.

**posteffect_values_day** is the posteffect values entry to use during the day.

**posteffect_values_night** is the posteffect values entry to use during the night.

**height** defines at which height the values are applied.

**fade_distance** defines the fade distance for the posteffect values.

## Tips

Make use of the console commands: `PostEffectVolumes.Enabled` and `PostEffectVolumes.Draw` when working with posteffects. Currently developer-only.

You can reload posteffects by using `reload posteffectvolumes` in the console.

**[Modding](/Modding)**
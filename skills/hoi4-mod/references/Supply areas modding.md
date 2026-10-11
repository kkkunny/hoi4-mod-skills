# Supply areas modding

**NOTE: Supply areas are deprecated as of patch 1.11 Barbarossa/No Step Back. The following only applies to version 1.10 and before.**

Supply areas are defined in /Hearts of Iron IV/map/supplyareas/*.txt.

Each supply area is typically stored in it's own file, although you can store multiple supply area definitions within the same file, as the ID is defined within the supply area definition, rather than the file title.

Here is a generic example of a supply area:

```
supply_area = {
    id =
    name =
    value =
    states = {

    }
}
```

- **id** defines the numerical id used by the supply area. The supply area IDs must be added sequentially, skipping numbers will cause crashes.
- **name** defines the localization key the supply area uses. You can use a non-localized string (i.e. "Paris"), but it is best practice to use localized strings.
- **value** defines the amount of base supply the supply area grants. In vanilla, the supply varies from 0 to 16.
- **states** scope defines which states the supply area covers. Note that supply areas should normally cover more than one state.

## Tips
- When placing supply areas, the states they are compose of should be contiguous. This means islands should have their own supply areas.
- The average supply value in vanilla is 10.

**[Modding](/Modding)**
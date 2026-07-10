# Effect

Effects (also known as Commands) are used in order to affect the game dynamically from within a specific scope. They are a one-time change to the current condition of the game, **without the ability to have a lasting effect**. Instead, [modifiers](/Modifiers) are used to have a continuous, everlasting effect on the game's condition that can be represented with a number. Effect blocks cannot be used to apply modifiers directly, however they can add something that can apply modifiers, most commonly with [add_ideas](#add ideas).

Effects are used throughout the game in numerous scopes, most commonly edited effect blocks are [national focus rewards](/National focus modding), [event options](/Event modding) and [decision effects](/Decision modding).

Note that certain effects may take a value from a variable, i.e. `add_manpower = var:my_var` This is noted by **<variable>** in an effect's parameters. See [Variables](/Variables) for information on the variable effects.

The list of effects may be outdated. A complete, but unsorted, list of effects can be found in /Hearts of Iron IV/documentation/effects_documentation.html or /Hearts of Iron IV/documentation/effects_documentation.md.

## Scopes

:
*Main article: [Scopes](/Scopes)*

Scopes serve as special effect types that modify the entity that serves as the context for the effects being executed, such as `GER = { add_political_power = 150 }` adding 150 political power to [/Germany](/Germany) [Germany](/Germany).

### Effect scopes

These can only be used as [effects](/Effects); trying to use them as [triggers](/Triggers) will result in nothing happening.

| Name | Usage | Target type | Example | Description | Version Added |
| --- | --- | --- | --- | --- | --- |
| every_possible_country | Always usable | Country | every_possible_country = { ... } | Executes children effects on every country that meets the limit, including those that do not exist. | 1.11 |
| every_country | Always usable | Country | every_country = { … } | Executes contained effects on every country that meets the limit. | 1.0 |
| random_country | Always usable | Country | random_country = { … } | Executes contained effects on a random country that meets the limit. | 1.0 |
| every_other_country | Within country scope only | Country | every_other_country = { … } | Executes contained effects on every country that meets the limit and is not the same country as the one this is contained in. | 1.0 |
| random_other_country | Within country scope only | Country | random_other_country = { … } | Executes contained effects on a random country that meets the limit and is not the same country as the one this is contained in. | 1.0 |
| every_country_with_original_tag | Always usable | Country | every_country_with_original_tag = {     original_tag_to_check = TAG  #required     …                  #effects to run } | Executes contained effects on every country that meets the limit and has the specified original tag. | 1.9 |
| random_country_with_original_tag | Always usable | Country | random_country_with_original_tag = {     original_tag_to_check = TAG  #required     …                  #effects to run } | Executes contained effects on a random country that meets the limit and has the specified original tag. |
| every_neighbor_country | Within country scope only | Country | every_neighbor_country = { … } | Executes contained effects on every country that meets the limit and borders the country this is contained in. | 1.0 |
| random_neighbor_country | Within country scope only | Country | random_neighbor_country = { … } | Executes contained effects on a random country that meets the limit and borders the country this is contained in. | 1.0 |
| every_occupied_country | Within country scope only | Country | every_occupied_country = { … } | Executes contained effects on every country that meets the limit and has any core states controlled by the country this is contained in. | 1.9 |
| random_occupied_country | Within country scope only | Country | random_occupied_country = { … } | Executes contained effects on a random country that meets the limit and has any core states controlled by the country this is contained in. | 1.9 |
| every_allied_country | Within country scope only | Country | every_allied_country = { … } | Executes children effects on every Allied Country different from the one in scope (or `random_select_amount` of random country if specified) that fulfills the `limit` trigger. | 1.15 |
| random_allied_country | Within country scope only | Country | random_allied_country = { … } | Executes children effects on a random Allied Country different from the one in scope that fulfills the `limit` trigger. | 1.15 |
| every_enemy_country | Within country scope only | Country | every_enemy_country = { … } | Executes contained effects on every country that meets the limit and is at war with the country this is contained in. | 1.0 |
| random_enemy_country | Within country scope only | Country | random_enemy_country = { … } | Executes contained effects on a random country that meets the limit and is at war with the country this is contained in. | 1.0 |
| every_subject_country | Within country scope only | Country | every_subject_country = { … } | Executes contained effects on every country that meets the limit and is a subject of the country this is contained in. | 1.11 |
| random_subject_country | Within country scope only | Country | random_subject_country = { … } | Executes contained effects on a random country that meets the limit and is a subject of the country this is contained in. | 1.11 |
| every_faction_member | Within country scope only | Country | every_faction_member = { … } | Executes children effects on every faction member of the country's faction in scope, if country does not have a faction it will only work on itself. | 1.17 |
| every_state | Always usable | State | every_state = { … } | Executes contained effects on every state that meets the limit. | 1.0 |
| random_state | Always usable | State | random_state = {     prioritize = { 123 321 } #optional     …    #effects to run } | Executes contained effects on a random state that meets the limit. | 1.0 |
| every_neighbor_state | Within state scope only | State | every_neighbor_state = { … } | Executes contained effects on every state that meets the limit and neighbours the state this is contained in. | 1.0 |
| random_neighbor_state | Within state scope only | State | random_neighbor_state = { … } | Executes contained effects on a random state that meets the limit and neighbours the state this is contained in. Does not support prioritizing. | 1.0 |
| every_owned_state | Within country scope only | State | every_owned_state = { … } | Executes contained effects on every state that meets the limit and is owned by the country this is contained in. | 1.0 |
| random_owned_state | Within country scope only | State | random_owned_state = {     prioritize = { 123 321 } #optional     …    #effects to run } | Executes contained effects on a random state that meets the limit and is owned by the country this is contained in. | 1.0 |
| every_core_state | Within country scope only | State | every_core_state = { … } | Executes contained effects on every state that meets the limit and is a core of the country this is contained in. | 1.11 |
| random_core_state | Within country scope only | State | random_core_state = {     prioritize = { 123 321 } #optional     …    #effects to run } | Executes contained effects on a random state that meets the limit and is a core of the country this is contained in. | 1.11 |
| every_controlled_state | Within country scope only | State | every_controlled_state = { … } | Executes contained effects on every state that meets the limit and is controlled by the country this is contained in. | 1.9 |
| random_controlled_state | Within country scope only | State | random_controlled_state = {     prioritize = { 123 321 } #optional     …    #effects to run } | Executes contained effects on a random state that meets the limit and is controlled by the country this is contained in. | 1.9 |
| random_owned_controlled_state | Within country scope only | State | random_owned_controlled_state = {     prioritize = { 123 321 } #optional     …    #effects to run } | Executes contained effects on a random state that meets the limit and is owned and controlled by the country this is contained in. | 1.3 |
| every_unit_leader | Within country scope only | Unit Leader | every_unit_leader = { … } | Executes contained effects on every unit leader (corps commanders, field marshals, admirals) that meets the limit and is recruited by the country this is contained in. | 1.5 |
| random_unit_leader | Within country scope only | Unit Leader | random_unit_leader = { … } | Executes contained effects on a random unit leader (corps commanders, field marshals, admirals) that meets the limit and is recruited by the country this is contained in. | 1.5 |
| every_army_leader | Within country scope only | Unit Leader | every_unit_leader = { … } | Executes contained effects on every army leader that meets the limit and is recruited by the country this is contained in. | 1.5 |
| random_army_leader | Within country scope only | Unit Leader | random_army_leader = { … } | Executes contained effects on a random army leader that meets the limit and is recruited by the country this is contained in. | 1.5 |
| global_every_army_leader | Always usable | Unit Leader | global_every_army_leader = { … } | Executes contained effects on every army leader that meets the limit. Preferable to use every_army_leader unless necessary to use global_every_army_leader. | 1.5 |
| every_navy_leader | Within country scope only | Unit Leader | every_navy_leader = { … } | Executes contained effects on every navy leader that meets the limit and is recruited by the country this is contained in. | 1.5 |
| random_navy_leader | Within country scope only | Unit Leader | random_navy_leader = { … } | Executes contained effects on a random navy leader that meets the limit and is recruited by the country this is contained in. | 1.5 |
| every_operative | Within country scope or operations only | Operative | every_operative = { … } | Executes contained effects on every operative that meets the limit and is recruited by the country this is contained in. | 1.9 |
| random_operative | Within country scope or operations only | Operative | random_operative = { … } | Executes contained effects on a random operative that meets the limit and is recruited by the country this is contained in. | 1.9 |
| every_character | Within country scope only | Character | every_character = { … } | Executes contained effects on every character that meets the limit and is recruited by the country this is contained in. | 1.11 |
| random_character | Within country scope only | Character | random_character = { … } | Executes contained effects on a random character that meets the limit and is recruited by the country this is contained in. | 1.11 |
| every_country_division | Within country scope only | Division | every_country_division = { … } | Executes contained effects on every division that meets the limit and is owned by the current country. | 1.12 |
| random_country_division | Within country scope only | Division | random_country_division = { … } | Executes contained effects on a random division that meets the limit and is owned by the current country. | 1.12 |
| every_state_division | Within state scope only | Division | every_state_division = { … } | Executes contained effects on every division that meets the limit and is located within the current state. | 1.12 |
| random_state_division | Within state scope only | Division | random_state_division = { … } | Executes contained effects on a random division that meets the limit and is located within the current state. | 1.12 |
| every_military_industrial_organization | Within country scope only | MIO | every_military_industrial_organization = { … } | Executes contained effects on every MIO within the current country that meets the limit. | 1.13 |
| random_military_industrial_organization | Within country scope only | MIO | random_military_industrial_organization = { … } | Executes contained effects on a random MIO within the current country that meets the limit. | 1.13 |
| every_purchase_contract | Within country scope only | Purchase contract | every_purchase_contract = { … } | Executes contained effects on every purchase contract within the current country that meets the limit. | 1.13 |
| random_purchase_contract | Within country scope only | Purchase contract | random_purchase_contract = { … } | Executes contained effects on a random purchase contract within the current country that meets the limit. | 1.13 |
| every_scientist | Within country scope only | Character | every_scientist = { … } | Executes children effects on every scientist (or "random_select_amount" of random character if specified) of the country in scope, that fulfills the "limit" trigger. | 1.15 |
| random_scientist | Within country scope only | Character | random_scientist = { … } | Executes children effects on random scientists that fulfills the "limit" trigger. | 1.15 |
| every_active_scientist | Within country scope only | Character | every_active_scientist = { … } | Executes children effects on every active scientist (or "random_select_amount" of random character if specified) of the country in scope, that fulfills the "limit" trigger.title. | 1.15 |
| random_active_scientist | Within country scope only | Character | random_active_scientist = { … } | Executes children effects on random scientists that fulfills the "limit" trigger. | 1.15 |
| party_leader | Within country scope only | Character | party_leader = {     limit = {         has_ideology = liberalism     }     set_nationality = BHR } | Executes the effects on the party leader with the specified ideology type. Must contain a has_ideology in the limit that refers to a specific ideology type (e.g. Despotic), not a group that contain the type (e.g. Non-Aligned). The selected character must be the leader of a party corresponding to the ideology group. | 1.11 |
| every_collection_element | Always usable | Collection/Any | every_collection_element = {     input = {         input = collection_id # This can be a collection name or an inline definition of a collection         limit = {             # Trigger - limit effect execution to a subset of elements         }     }     # Effects to be executed } | Applies arbitrary effects to all elements of a collection. To learn more about collections, see the documentation in /Hearts of Iron IV/common/collections. | 1.17 |

**NOTE:** Some of these scopes may have no countries/states that match the criteria.

### Effects with scopes

Effects that change the scope include the following:

- [start_civil_war](/Effect#start civil war), which changes it to the rebelling dynamic country.
- [create_dynamic_country](/Effect#create dynamic country), which changes it to the newly-created dynamic country.

### Dual scopes

The following scopes can be used either as effect or trigger scopes; some can also be used as the right side of some effects and triggers as a target. If usage as a target is possible, it's marked within the table.
Several dual scopes may have a scope that varies depending on where it's used, such as variables, which can be set to anything.

| Name | Usage | Target type | Example | Description | Usable as target | Version Added |
| --- | --- | --- | --- | --- | --- | --- |
| TAG | Always usable | Country scope | SOV = { country_event = my_event.1 } | The country defined by the tag or tag alias. Tag aliases are defined in /Hearts of Iron IV/common/country_tag_aliases, as a way to refer to a specific country (such as a side in a civil war) in addition to its actual tag. If the country with the exact tag doesn't exist, but a dynamic country originating from the specified tag does, the scope will refer to the dynamic country. | ✓ | 1.0 |
|  | Always usable | State scope | 123 = { transfer_state_to = SCO } | The state defined by this id. | ✓ | 1.0 |
|  | not within Character scope | Character scope | ENG_theodore_makhno = { set_nationality = UKR } | On game versions prior to 1.12.8, the character must be already recruited by the country this is scoped from. | ✓ | 1.11 |
| mio: | Within country scope only | MIO scope | mio:AST_cockatoo_doe_organization = { … } | The MIO identified by that ID as defined within the /Hearts of Iron IV/common/military_industrial_organization/organizations/*.txt file. | ✓ | 1.13 |
| sp: | Within country scope only | Special project scope | sp:sp_land_flamethrower_tank = { … } | The special project identified by that ID as defined within the /Hearts of Iron IV/common/special_projects/projects/*.txt file. | ✓ | 1.15 |
| ROOT | Always usable | Depends on usage | ENG = {     FRA = {         GER = {             declare_war_on = {                 target = ROOT                 type = annex_everything             }         }     } } #GER declares war on ENG (if there is no scope before ENG) | Targets the root node of the block, an inherent property of each block. Most commonly, this is the default scope: for example, ROOT within a national focus will always refer to the country doing the focus and ROOT within a event will always refer to the country getting the event. However, some blocks do distinguish between the default scope and ROOT, such as certain scripted GUI contexts or certain on actions. If a block doesn't have ROOT defined (such as on_startup in on actions), then it is impossible to use it. | ✓ | 1.0 |
| THIS | Always usable | Depends on usage | set_temp_variable = { target_country = THIS } | Targets the current scope where it's used. For example, when used in every_state, it will refer to the state that's currently being evaluated. Primarily useful for variables (as in the example, where omitting it wouldn't work) or for built-in localisation commands, where some scope must be specified. More rarely, this may help with scope manipulation when using PREV. Since omitting it makes no difference in how the code gets interpreted, there is little to no usage outside of these cases. | ✓ | 1.0 |
| PREV | Always usable | Depends on usage | FRA = {     random_country = {         GER = {             declare_war_on = {                 target = PREV                 type = annex_everything             }         }     } } #Germany declares war on random_country | Targets the scope that the current scope is contained in. Can have additional applications where the assumed default scope differs from the ROOT, such as in state events or some on_actions. Can be chained indefinitely as PREV.PREV. Commonly results in broken-looking tooltips: what's shown to the player doesn't always correlate with reality. See also: PREV usage. | ✓ | 1.0 |
| FROM | Always usable | Depends on usage | declare_war_on = {     target = FROM     type = annex_everything }  FROM = {     load_oob = defend_ourselves } | Can be chained indefinitely as FROM.FROM. Used to target various hardcoded scopes inherent to the block, often a secondary scope in addition to ROOT. For example: In events, this refers to the country that sent the event (i.e. if the event was fired using an effect, then it's the ROOT scope where it was fired). In targeted decisions or diplomacy scripted triggers, this refers to the scope that is targeted. | ✓ | 1.0 |
| overlord | Within country scope only | Country scope | overlord = { … } | The overlord of the country if it is a subject. Subject to the 'invalid event target' error. | X | 1.3 |
| faction_leader | Within country scope only | Country scope | faction_leader = { add_to_faction = FROM } | Faction leader of the faction the country is a part of. Subject to the 'invalid event target' error. | X | 1.10.1 |
| owner | Within state, character, or combatant scope only | Country scope | owner = { add_ideas = owns_this_state } | In state scope, the country that owns the state. In combatant scope, the country that owns the divisions. In character scope, the country that has recruited the character. Subject to the 'invalid event target' error when used for a state. | X | 1.0 |
| controller | Within state scope only | Country scope | controller = {      ROOT = {         create_wargoal = {             target = PREV             type = take_state_focus             generator = { 123 }         }     } } | The controller of the current state. Subject to the 'invalid event target' error. | X | 1.0 |
| capital_scope | Within country scope only | State scope | capital_scope = { … } | The state where the capital of the current country is located in. Subject to the 'invalid event target' error in rare cases. | X | 1.0 |
| event_target: | Always usable | Depends on usage | event_target:my_event_target = { … } | Saved event target or global event target, with no space after the colon. Subject to the 'invalid event target' error. | ✓ | 1.0 |
| var: | Always usable | Depends on usage | var:my_variable = { … }add_to_faction = my_variable or add_to_faction = var:my_variable | Variable set to a scope. When used as a target rather than a scope, the var: can be omitted in most cases. | ✓ | 1.5 |

## Any scope

Can be used in **country**, **state** or **character** scopes.

### General

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_dynamic_modifier | modifier = The name of the Modifier.scope = If you specify it, your dynamic modifier will be scoped to this scope. Optional.days = x The modifiers will be removed after x days have passed. Optional. | add_dynamic_modifier = {     modifier = example_dynamic_modifier     scope = GER     days = 14 } | Adds a dynamic modifier to the specified scope (the default scope is ROOT).It will be updated daily, unless forced to update early by force_update_dynamic_modifier effect. | Examples can be found in /Hearts of Iron IV/common/dynamic_modifiers/*.txt. Any modifiers that use variables within of the dynamic modifier will not show up in the tooltip of this effect, while those that are set to a static value will. Supports the state, country, character, and special project scopes. | 1.6 |
| remove_dynamic_modifier | modifier = The name of the Modifier. | remove_dynamic_modifier = { modifier = sabotaged_ressources } | Removes a dynamic modifier from the current scope | Examples can be found in /Hearts of Iron IV/common/dynamic_modifiers/*.txt | 1.6 |
| force_update_dynamic_modifier | Boolean. | force_update_dynamic_modifier = yes | Forces an update to the effects given by variables within dynamic modifiers. | An update is done daily by default; this can be used if the applied values need to be changed urgently, such as if modifiers are checked or used later in the effect block. | 1.6 |
| add_state_resistance_compliance_modifier | modifier = Modifier to apply.state= Affected state. | add_state_resistance_compliance_modifier  = {        modifier = dynamic_modifier_name 	   state = 738 } | Adds either a resistance or compliance modifier to a state. Can only use modifiers from the /Hearts of Iron IV/common/resistance_modifiers.txt/compliance_modifiers.txt that are marked as is_dynamic = yes | 1.17 |
| remove_state_resistance_compliance_modifier | modifier = Modifier to remove.state= Affected state. | remove_state_resistance_compliance_modifier  = {        modifier = dynamic_modifier_name 	   state = 738 } | Removes either a resistance or compliance modifier from a state. Can only use modifiers from the /Hearts of Iron IV/common/resistance_modifiers.txt/compliance_modifiers.txt that are marked as is_dynamic = yes | 1.17 |
| set_global_flag | An unique string to identify the global flag with. OR flag = The flag to set. days = Sets the flag to last for the specified amount of days. Optional. value = The new value of the flag on the scale from -2 147 483 648 to 2 147 483 647. | set_global_flag = my_flag set_global_flag = {     flag = my_flag     days = 123     value = 1 } | Defines a global flag. | No tooltip is shown. The flag in this effect is used in the meaning of 'boolean flag', used to store information. | 1.0 |
| play_song | A music file located in the music folder and .asset | play_song = "general_peace_1" | Plays an audio track | The song must be defined in a music station in order to work. More information can be found in the Music modding page. If you wish to simply play a sound, the sound_effect effect should be used instead. The song will start playing for every country if the effect is executed. See scoped_play_song if only one country should have the song. | 1.9.3 |
| clr_global_flag | The unique string of a global flag to clear. | clr_global_flag = my_flag | Clears a defined global flag. | No tooltip is shown | 1.0 |
| modify_global_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_global_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. | 1.3 |
| custom_effect_tooltip | A localized string to display in the tooltip. | custom_effect_tooltip = my_tooltip_tt custom_effect_tooltip = {     localization_key = my_loc     NESTEDLOC = myotherloc/string } | Displays a localized key in the effect tooltip. | Also supports Localisation#Bindable_localisation. | 1.0 |
| custom_override_tooltip | tooltip = A localized string to display in the tooltip. not_tooltip = A localized string to display in the tooltip for NOT block. Optional. | custom_override_tooltip= {     tooltip = my_tt     not_tooltip = my_tt_NOT      } | Executes the provided effects but with a custom tooltip surpressing all tooltips from all other effects inside this block. | Can also be used as trigger. Also supports Localisation#Bindable_localisation. | 1.15 |
| effect_tooltip |  | effect_tooltip = {     declare_war_on = {         target = FRA     } } | Displays the effects in the tooltip without executing them. |  |  |
| log | An string to in the game.log | log = "myVariable: [?myVariable]" | Displays a string in the user directory's /Hearts of Iron IV/logs/game.log file when executed, as well as showing up in the console if it is open when the logging effect was executed. | Accepts all localisation commands (e.g. [Root.GetName], [GetDateText], etc) | 1.5 |
| save_event_target_as | An unique string to identify the event target with. | capital_scope = {     save_event_target_as = my_state } | Saves the current scope as a key. Is cleared once execution ends (i.e. end of event). | Use event_target: to access the scope.Do not use in Scripted GUIs. | 1.0 |
| save_global_event_target_as | An unique string to identify the global event target with. | random_other_country = {     save_global_event_target_as = my_country } | Saves the current scope as a key. Persists after execution until cleared via effect. | Use event_target: to access the scope.Do not use in Scripted GUIs. | 1.0 |
| clear_global_event_target | The unique string of the global event target to clear. | clear_global_event_target = my_country | Clears a specific global event target. |  | 1.0 |
| clear_global_event_targets | yesBoolean. | clear_global_event_targets = yes | Clears all global event targets. |  | 1.0 |
| sound_effect | A sound reference from an .asset file. | sound_effect = "boom" | Plays the specified sound once. | The sound effect must be properly defined in /Hearts of Iron IV/sound/ See also: Sound modding. The sound will play for every country if the effect is executed. See scoped_sound_effect if only one country should hear it. | 1.0 |
| randomize_weather | A seed integer. | randomize_weather = 12345 | Randomizes the weather with the specified seed. |  | 1.0 |
| set_province_name | id = The id of the province to be changed. name = The name to change the province to. | set_province_name = {     id = 325     name = LOC_KEY }  set_province_name = { id = 325 name = "New Name" } | Changes the specified province/victory point's name to the specified name. | Localisation keys are to be defined in /Hearts of Iron IV/localisation/*_l_.yml | 1.3 |
| reset_province_name | The id of the province to reset. | reset_province_name = 325 | Resets the specified province's name. |  | 1.3 |
| damage_units | province = Province where to damage units. state = State where to damage units. region = Strategic region where to damage units. limit = {  }Will only delete units if the triggers within are met for the country that owns the units. damage = The percentage of damage done to units. org_damage = The percentage of damage done to units to organisation in particular. str_damage = The percentage of damage done to units to strength in particular. ratio = Will damage a ratio damage to total organisation/strength of unit if set. template = If specified, requires the template name to match. army = Will damage the army units. navy = Will damage the navy units. | damage_units = {     province = 42     state = 5     region = 5     limit = { has_country_flag = TAG_test }     damage = 0.5     org_damage = 0.5     str_damage = 0.5     ratio = yes     template = "template_name"     army = no     navy = yes } | Damages units in the specified area. |  | 1.11 |
| create_entity | entity = The entity to spawn, defined within /Hearts of Iron IV/gfx/entities/*.asset files. id = intA number ID which can be referred to by other effects. Optional. var = If provided, the id of the entity will be stored using this variable. Optional. x = The X position of the entity. y = The Y position of the entity. z = The Z position of the entity. province = The province the middle of which to use as the entity's position. state = The state the middle of which to use as the entity's position. rotation = The rotation of the entity in radians. scale = The size of the entity. min_zoom = Minimum zoom level needed to be able to see the entity. visible = The scripted trigger that must be met for a country for it to see the entity. | create_entity = {     entity = entity_name     id = 123     var = var_name     x = 42     y = 21     z = 3     province = 123     state = 42     rotation = 1.2     scale = 10.0     min_zoom = 100.0     visible = scripted_trigger_name } | Creates an entity. | Uses the the same coordinate system that the map uses. A positive change in rotation results in counter-clockwise rotation, a full 360 degrees rotation is approximately 6.28 radians. For comparison, default minimum zoom level (closest to the map) is 50 units, while default maximum zoom level is 3000 units. | 1.11 |
| destroy_entity | The ID of the entity to destroy. | destroy_entity = 123 | Deletes an entity | IDs are set by the create_entity effect. | 1.11 |
| set_entity_movement | id = The ID of the entity to modify. ratio = Distance between starting position and target position where the entity is to be placed. rotation = The rotation to apply after the positioning. start and target arguments:  x =  The X position of the point. y =  The Y position of the point. z =  The Z position of the point. province =  The province the middle of which to use as the point. state =  The state the middle of which to use as the point. | set_entity_movement = {     id = 123     start = {         x = 42         y = 21         z = 3     }     target = {         province = 124     }     ratio = 0.5     rotation = 1.2 } | Sets the position and rotation of an entity using two coordinates. | IDs are set by the create_entity effect. Uses the the same coordinate system that the map uses. A positive change in rotation results in counter-clockwise rotation, a full 360 degrees rotation is approximately 6.28 radians. | 1.11 |
| set_entity_position | id = x = y = z = province = state = | set_entity_position = {   id = 123   x = 42   y = 21   z = 3   province = 123   state = 42 } | Sets the position of an existing entity | IDs are set by the create_entity effect. Uses the the same coordinate system that the map uses. | 1.11 |
| set_entity_rotation | id = The ID of the entity to modify. rotation = The new angle in radians. | set_entity_rotation = {     id = 123     rotation = 0.23 } | Sets the currently-facing angle of an existing entity. | IDs are set by the create_entity effect. A positive change results in counter-clockwise rotation, a full 360 degrees rotation is approximately 6.28 radians. | 1.11 |
| set_entity_scale | id = The ID of the entity to modify. scale = The scale to change the entity to. | set_entity_scale = {   id = 123   scale = 5.0 } | Sets the size of an existing entity. | IDs are set by the create_entity effect. | 1.11 |
| set_entity_animation | id = The ID of the entity to modify. animation = The animation entry to apply. | set_entity_animation = {     id = 123     animation = "shoot_lasers" } | Sets the animation of a specified entity. | IDs are set by the create_entity effect. Animations are defined within the /Hearts of Iron IV/gfx/models/**/*.asset files. | 1.11 |
| build_railway | level = Defaults to 1 build_only_on_allied = No by default, if yes and in a country scope, it will only build on allied territories for the country scoped. fallback = Defaults to no, if yes each option will try to fallback to the next available one. path = {  } start_province =  target_province =  start_state =  target_state = If using start state/target state, the game will pick the provinces with the best supply available. If using state province/target province, the game will link those two provinces. | build_railway = {     level = 1     build_only_on_allied = yes     controller_priority = {         base = 1         modifier = {             tag = MAN             add = 2         }     }     fallback = yes     path = { 42 10 20 30 40 84 }     start_province = 42     target_province = 84 }  build_railway = {     level = 1     build_only_on_allied = yes     controller_priority = {         base = 1         modifier = {             tag = MAN             add = 2         }     }     fallback = yes     path = { 50 10 20 30 40 100 }     start_state = 50     target_state = 100 } | Adds a railway level between two provinces or along a predefined path. |  | 1.11 |
| event_option_tooltip | The name of the option. | event_option_tooltip = mtg_usa_civil_war_fascists.1.a | Shows the tooltip usually received for hovering over an event option with the specified name. | ROOT and FROM scopes are swapped. | 1.13 |
| create_purchase_contract | seller = The seller in the contract. buyer = The buyer in the contract. civilian_factories = The amount of civilian factories required by the contract. equipment = { ... }The equipment that the contract is for. In particular, contains these attributes:  type = The archetype of the equipment. amount = The amount of the specified equipment. | create_purchase_contract = {     seller = ROOT     buyer = FROM     civilian_factories = 2     equipment = {         type = artillery_equipment         amount = 300     } } | Creates a purchase contract with the specified parameters. | Allows using equipment = { ... } several times. | 1.13 |

### Border wars

These effects refer to the border wars that simulate combat on a border between two countries, with provinces where it takes place being highlighted in white. For the state-based border wars represented with orange stripes on states, see [set_border_war](/Effect#set border war) in the state scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| start_border_war | change_state_after_war = Whether the state changes hands after the war. Attacker or Defender scope state =  / The state the side is fighting on. num_provinces = The number of provinces used in the state. on_win = The event to fire for the side on a win. on_lose = The event to fire for the side on a loss. on_cancel = The event to fire for the side on a draw. modifier = The modifier on combat. Defaults to 0. dig_in_factor = The modifier applied to dig-in bonuses. Defaults to 1. terrain_factor = The modifier applied to terrain bonuses. Defaults to 1. | start_border_war = {     change_state_after_war = no     attacker = {         state = 527         num_provinces = 4         on_win = japan_border_conflict.2         on_lose = japan_border_conflict.3         on_cancel = japan_border_conflict.4         modifier = 0.1         dig_in_factor = 0         terrain_factor = 0     }	     defender = {         state = 408         num_provinces = 4         on_win = japan_border_conflict.3         on_lose = japan_border_conflict.2         on_cancel = japan_border_conflict.4     } } | Starts a border war for the specified attacker and defender. The participating countries are the owners of the specified states. |  | 1.5 |
| set_border_war_data | attacker =  / The attacker state. defender =  / The defender state.attacker_modifier =  / The modifier applied to attacker strength.defender_modifier =  / The modifier applied to attacker strength.combat_width =  / The combat width used in the border war battle. | set_border_war_data = {     attacker = 527     defender = 408     defender_modifier = 0.15     combat_width = 100 } | Sets the bonuses or penalties for the attacker and defender in an on-going border war. Used after start_border_war. |  | 1.5 |
| cancel_border_war | attacker =  / The attacker state. defender =  / The defender state.dont_fire_events = Stops the events from start_border_war from firing. | cancel_border_war = {     dont_fire_events = yes     defender = 408     attacker = 527 } | Cancels an on-going border war without a winner. |  | 1.5 |
| finalize_border_war | attacker =  / The attacker state. defender =  / The defender state.attacker_win = Makes the attacker the winner.defender_win = Makes the defender the winner. | finalize_border_war = {     attacker_win = yes     attacker = 527     defender = 408 } | Ends an on-going border war. |  | 1.5 |

### Variables

*This section is transcluded from [Data structures § Operators](/Data structures#Operators)*
The following is a list of variable-related effects and triggers. Variable-modifying effects have an equivalent for temporary variables, with `temp_variable` being used instead of `variable`, and these temporary variable operators are also valid triggers, as described above. Every operator can be used with variables that do not exist, assuming a value of 0 unless a null-coalescing operator is used.

| Name | Parameters | Examples | Description | Notes |
| --- | --- | --- | --- | --- |
| set_variable | var = The variable to modify or create. value = /The value to set the variable to. tooltip = localisation_keyLocalisation used by the operation. Optional. | set_variable = {     var = my_variable     value = 100     tooltip = set_var_to_100_tt } set_temp_variable = { temp_var = ROOT.overlord } | Sets a variable's value to the specified amount, creating it if not defined. | Shortened version exists with set_variable = {  =  }. |
| set_variable_to_random | var = The variable to modify or create. min = The minimum possible value, defaults to 0. max = The maximum possible value, defaults to 1. integer = Sets if the variable must be an integer or if it can be decimal. Defaults to false. | set_variable_to_random = {     var = random_num     max = 11     integer = yes } set_temp_variable_to_random = my_var | Sets a variable's value to the specified amount, creating it if not defined. The result will be greater than or equal than the minimum and strictly less than the maximum. | Shortened version exists with set_variable_to_random = , setting it to a decimal between 0 and 1. Can be used in triggers. |
| clear_variable | Variable to clear. | clear_variable = my_variable | Clears the value from the memory entirely. | Can only be used on regular variables. |
| add_to_variable | var = The variable to add to. value = /The value to add to the variable. tooltip = localisation_keyLocalisation used by the operation. Optional. | add_to_variable = {     var = my_variable     value = 100     tooltip = add_100_to_var_tt } add_to_temp_variable = { temp_var = num_owned_states } | Increases a variable's value by the specified amount, creating it if not defined. | Shortened version exists with add_to_variable = {  =  }. |
| subtract_from_variable | var = The variable to subtract from. value = /The value to subtract from the variable. tooltip = localisation_keyLocalisation used by the operation. Optional. | subtract_from_variable = {     var = my_variable     value = 100     tooltip = sub_100_from_var_tt } subtract_from_temp_variable = { temp_var = num_owned_states } | Decreases a variable's value by the specified amount, creating it if not defined. | Shortened version exists with subtract_from_variable = {  =  }. Equivalent to adding a negative amount. |
| multiply_variable | var = The variable to multiply. value = /The value to multiply the variable by. tooltip = localisation_keyLocalisation used by the operation. Optional. | multiply_variable = {     var = my_variable     value = 100     tooltip = multiply_var_by_100_tt } multiply_temp_variable = { temp_var = num_owned_states } | Multiplies a variable's value by the specified amount. | Shortened version exists with multiply_variable = {  =  }. |
| divide_variable | var = The variable to divide. value = /The value to divide the variable by. tooltip = localisation_keyLocalisation used by the operation. Optional. | divide_variable = {     var = my_variable     value = 100     tooltip = divide_var_by_100_tt } divide_temp_variable = { temp_var = num_owned_states } | Divides a variable's value by the specified amount. | Shortened version exists with divide_variable = {  =  }. |
| modulo_variable | var = The variable to modulo. value = /The value to modulo the variable by. tooltip = localisation_keyLocalisation used by the operation. Optional. | modulo_variable = {     var = my_variable     value = 50     tooltip = get_modulo_of_var_by_50_tt } modulo_temp_variable = { temp_var = num_controlled_states } | Makes the variable become the remainder of Euclidean division of the variable by the specified value. | Shortened version exists with modulo_variable = {  =  }. |
| round_variable | The variable to round. | round_variable = my_variable round_temp_variable = temp | Rounds the variable towards the closest integer value. | If exactly between two integers (Such as 1.5), the option with lager absolute val gets chosen ( if -1.5,will be -2 ). |
| clamp_variable | var = The variable to clamp. min = /The minimum value of the variable after the clamp. max = /The maximum value of the variable after the clamp. | clamp_variable = {     var = my_var     min = 0 } clamp_temp_variable = {     var = my_var     min = 0 } | Clamps the variable to ensure its value is between the two specified numbers, raising to the minimum if smaller or lowering to the maximum if larger. | Either min or max can be omitted, in which case it'll not be checked. Does nothing if the variable is already in the range between min and max. This only changes the current value of the variable, it can still go beyond the minimum or the maximum after the clamp. |
| career_profile_set_temp_playthrough_variable | var = The variable to modify or create. value = /The value to set the variable to. | career_profile_set_temp_playthrough_variable = {   sum = rocket_sites_built_1936 } | Sets a temporary variable to a value or another variable. |  | ??? |
| career_profile_set_temp_variable | var = The variable to modify or create. value = /The value to set the variable to. | career_profile_set_temp_variable = {   var = num_dogs   value = num_dogs_in_career_profile } | Sets a temporary variable to a value or another variable. |  | ??? |

### Arrays

*See also: [Arrays](/Arrays)*

| Name | Parameters | Examples | Description | Notes |
| --- | --- | --- | --- | --- |
| add_to_array | array = The array to modify. value = /The variable to add. index = The index to place the variable on in the array. Optional, defaults to the end of the array. | add_to_array = {     array = global.my_countries     value = THIS.id } add_to_temp_array = { temp_states = THIS } | Adds an element to the array either at the specified index, defaulting to the end otherwise. | Shortened version exists with add_to_array = {  =  }. |
| remove_from_array | array = The array to modify. value = /The variable to remove. Optional. index = The index to remove the variable from in the array. Optional. | remove_from_array = {     array = global.my_countries     index = 0 } remove_from_temp_array = { temp_states = THIS } | Removes an element from the array with the specified value or index. | Shortened version exists with remove_from_array = {  =  }. If neither value nor index are specified, then the last element is deleted. |
| clear_array | The array to clear. | clear_array = global.my_countries clear_temp_array = temp_states | Clears the array, removing every element inside. |  |
| resize_array | array = The array to modify. value = /The variable to add to the array if the size is larger than the array's current size. Optional, defaults to 0. size = The amount of elements inside of the array after the resizing. | resize_array = {     array = global.countries_by_states     value = 10     size = global.countries^num } resize_temp_array = { temp_states = 20 } | Resizes the array, removing or adding elements in the end if necessary. | Shortened version exists with resize_array = {  =  }. |
| find_highest_in_array | array = The array to modify. value = The temporary variable where the largest value will get stored. index = The temporary variable where the index of the largest value will get stored. | find_highest_in_array = {     array = global.countries_by_states     value = temp_largest_country     index = temp_country_index } | Finds the largest value in the array and assigns its value and index to a temporary variable. | Either value or index are optional to specify. |
| find_lowest_in_array | array = The array to modify. value = The temporary variable where the smallest value will get stored. index = The temporary variable where the index of the smallest value will get stored. | find_lowest_in_array = {     array = global.countries_by_states     value = temp_largest_country     index = temp_country_index } | Finds the smallest value in the array and assigns its value and index to a temporary variable. | Either value or index are optional to specify. |

## Country scope

The effects here must be used within a **country** scope.

### General

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_country_flag | An unique string to identify the country flag with. OR flag = The flag to set. days = Sets the flag to last for the specified amount of days. Optional. value = The new value of the flag on the scale from -2 147 483 648 to 2 147 483 647. | set_country_flag = my_flag set_country_flag = {     flag = my_flag     days = 123     value = 1 } | Defines a country flag. | No tooltip is shown. The flag in this effect is used in the meaning of 'boolean flag', used to store information. In order to change the flag that represents the country, see cosmetic tags. | 1.0 |
| clr_country_flag | The unique string of a country flag to clear. | clr_country_flag = my_flag | Clears a defined country flag. |  | 1.0 |
| modify_country_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_country_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. | 1.3 |
| country_event | id = The event to fire. days =  / Fires the event in the specified number of days. Optional.hours =  / Fires the event in the specified number of hours. Optional.random_hours =  / Adds a random number (between 0 and random_hours, inclusive) of hours to the scheduled fire time. Optional.random_days =  / Adds a random number (between 0 and random_days, inclusive) of days to the scheduled fire time. Optional. | country_event = {     id = my_event.1     days = 10     random_hours = 12     random_days = 10 }  country_event = my_event.1 | Fires the specified event for the current country. | Where triggers do not need to be repeatedly checked random can be a performance light alternative to mean_time_to_happen for scheduling events. Shortened variant exists if the event's ID is used instead of arguments. | 1.0 |
| news_event | id = The event to fire. days =  / Fires the event in the specified number of days. Optional.hours =  / Fires the event in the specified number of hours. Optional.random_hours =  / Adds a random number (between 0 and random_hours, inclusive) of hours to the scheduled fire time. Optional.random_days =  / Adds a random number (between 0 and random_days, inclusive) of days to the scheduled fire time. Optional. | news_event = {     id = my_event.1     days = 10     random_hours = 12     random_days = 10 }  news_event = my_event.1 | Fires the specified news event for the current country. | The news event uses a different interface to the country event. Where triggers do not need to be repeatedly checked random can be a performance light alternative to mean_time_to_happen for scheduling events. Shortened variant exists if the event's ID is used instead of arguments. | 1.0 |
| set_cosmetic_tag | The cosmetic tag to switch to. | set_cosmetic_tag = SAF_SOV_communism | Makes the current scope use the specified cosmetic tag, changing name and flag. |  | 1.3 |
| drop_cosmetic_tag | Boolean. | drop_cosmetic_tag = yes | Makes the current scope drop the current cosmetic tag they are using. |  | 1.3 |
| set_rule | Boolean. desc = The localisation used as the description for why the rule is set. | set_rule = {     desc = TAG_my_rule_description     can_create_factions = yes } | Toggles the special game rules for the current scope. Note: each rule can only be toggled a few times before a reload is required. | Game rule list |
| The following game rules exist as possible options:   Internal name | Localised name | Notes |
| can_access_market | Can access International Market (Puppets and Overlords can always access each other's market) |  |
| can_be_spymaster | Can be Spy Master |  |
| can_boost_other_ideologies | Can boost popularity of other ideologies |  |
| can_boost_own_ideology | Can boost own party popularity in other countries |  |
| can_create_collaboration_government | Can create collaboration governments |  |
| can_create_factions | Can Create Factions |  |
| can_declare_war_on_same_ideology | Can declare war on country with the same ideology group without a war goal |  |
| can_declare_war_without_wargoal_when_in_war | Can declare war on a neighbor without a wargoal when at war with a major |  |
| can_decline_call_to_war | Can decline call to war |  |
| can_force_government | Can force government of another country to adopt the same ideology |  |
| can_generate_female_aces | Women in your country are allowed to become military pilots |  |
| can_generate_female_country_leaders | Can generate female country leaders |  |
| can_generate_female_unit_leaders | Can generate female unit leaders |  |
| can_guarantee_other_ideologies | Can guarantee other ideologies |  |
| can_join_factions | Can join factions |  |
| can_join_factions_not_allowed_diplomacy | Country's name is not allowed to join factions |  |
| can_join_opposite_factions | Can Join Factions led by another Ideology |  |
| can_lower_tension | Lowers World Tension with Guarantees |  |
| can_not_build_buildings | CAN_NOT_BUILD_BUILDINGS | Doesn't seem to work. |
| can_not_declare_war | Can not declare wars | Prevents generating wargoals, but not using existing ones. |
| can_occupy_non_war | Can hold territory owned by a country they are not at war with |  |
| can_only_justify_war_on_threat_country | Can justify war goals against a country that have not generated world tension |  |
| can_puppet | Can puppet a country |  |
| can_send_volunteers | Can send volunteer forces |  |
| can_use_kamikaze_pilots | Can use kamikaze pilots |  |
| contributes_operatives | Contributes Operatives to Spy Master: Yes | Only has an effect for subjects. |
| units_deployed_to_overlord | Control over deployed units go to overlord | Only has an effect for subjects. |

1.0

set_party_rule

`ideology = `Ideology group of the party.
`desc = `A description used for the rule. Optional, defaults to being the same as default.
` = `Rule's new value.

```
set_party_rule = {
    ideology = democratic
    desc = TAG_my_rule_description
    can_create_factions = yes
}
```

Toggles the special game rules for the current scope's political party.

1.12

add_relation_rule_override

`target = `Target of the rule.
`usage_desc = `A description used as the reason for the rule applying. Optional.
`trigger = `A [scripted trigger](/Scripted trigger) deciding when the override should be active. Optional, defaults to always true.
` = `Rule's new value.

```
add_relation_rule_override = {
    target = SOV
    usage_desc = TAG_my_rule_description
    trigger = my_scripted_trigger
    can_access_market = yes
}
```

Toggles the special game rules for the current scope in diplomacy towards the specified country only, if the trigger is met.

Currently `can_access_market` and `can_send_volunteers` are supported. In case of overlap, restricting actions is preferred (e.g. `can_send_volunteers = no` or `can_not_declare_war = yes` are preferred over the alternatives). In the scripted trigger, `ROOT` is the country with the override and `FROM` is the target.

1.13

remove_relation_rule_override

`target = `Target of the rule.
`usage_desc = `A description used as the reason for the rule applying. Optional.
`trigger = `A [scripted trigger](/Scripted trigger) for identifying the relation rule.
` = `Rule's new value.

```
remove_relation_rule_override = {
    target = SOV
    usage_desc = TAG_my_rule_description
    can_access_market = yes
}
```

Removes the toggle added with [add_relation_rule_override](#add relation rule override).

1.13

scoped_sound_effect

``A sound reference from an .asset file.

```
scoped_sound_effect = "boom"
```

Plays the specified sound once only for the current country.

The sound effect must be properly defined in /Hearts of Iron IV/sound/ More info can be found in the [Sound modding](/Sound modding) article.

1.6

scoped_play_song

``A music file located in the music folder and .asset

```
scoped_play_song = "general_peace_1"
```

Plays an audio track for the specified country only.

The song must be defined in a music station in order to work. More information can be found in the [Music modding](/Music modding) page. If you wish to simply play a sound, the scoped_sound_effect effect should be used instead.

1.9.3

goto_province

``The id of the province go to.

```
goto_province = 325
```

Moves the camera position over the specified province.

1.0

goto_state

` / `The id of the state go to.

```
goto_state = 1
```

```
goto_state = var:some_state
```

Moves the camera position over the specified state.

1.0

change_tag_from

` / `The country to change from.

```
change_tag_from = ROOT
```

```
change_tag_from = var:from.country
```

Switches the player to the current scope from the target scope. Nothing happens if the target scope is controlled by AI.

**The country the player becomes needs to be the scope in which the command is used.** For example, `ABC = { change_tag_from = XYZ }` will make the player controlling XYZ play as ABC instead.

1.0

reserve_dynamic_country

``

```
reserve_dynamic_country = yes
```

Reserves the dynamic country, making sure that it does not get recycled for civil war even if it does not exist.

Usually used in combination with [create_dynamic_country](#create dynamic country).

1.9

force_update_map_mode

`limit = { ... }`Triggers required for the map mode to refresh. Optional.
`mapmode = `The ID of the custom map mode.

```
force_update_map_mode = {
    limit = {
        is_ai = no
    }
    mapmode = my_map_mode
}
```

Forcefully refreshes the specified mapmode for the player, rather than waiting for a daily update.

Map modes are defined in /Hearts of Iron IV/common/map_modes/*.txt

1.11

add_ai_strategy

`type = `The type of strategy.
`id = `What country the strategy is against.
`value = `The weighting added by the strategy.

```
add_ai_strategy = {
    type = alliance
    id = GER
    value = 200
}
```

Sets an AI strategy for the current scope.

See [AI Modding](/AI modding) for more details.

1.0

create_dynamic_country

`original_tag = `The original tag to be used by the country.
`copy_tag = `If specified, copies stuff from this tag rather than the original tag.
``Effects that will be executed on the new dynamic country.

```
create_dynamic_country = {
    original_tag = POL
    copy_tag = SOV
    add_political_power = 100
    transfer_state = 123
}
```

Creates a new dynamic country, akin to ones used in civil wars.

The [reserve_dynamic_country](#reserve dynamic country) effect can be used if the dynamic country does not yet exist in order to ensure that it does not get overwritten by other creations of dynamic countries. If this is not done, the dynamic country will immediately stop existing if no states are transferred in the same scope.
Every state of the original country immediately gets set as a dynamic country's core: if that's unneeded, the cores would need to be removed after creation.

1.9

### States

These effects in particular are country-scoped effects that are related to states rather than effects within the state scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_state_core | / The state to add core to. | add_state_core = 345 | Adds a core for the current scope to the specified state. |  | 1.0 |
| remove_state_core | / The state to remove core from. | remove_state_core = 345 | Removes the core of the current scope from the specified state. |  | 1.0 |
| set_capital | state =  / The state to make capital. remember_old_capital = noWhether the old capital gets "remembered", making the country change to it in case the current capital is lost. | set_capital = {state = 345} set_capital = {   state = 345   remember_old_capital = no } | Makes the specified state the current scope's capital state. | Syntax has been changed in 1.11. It was "set_capital = 345" Old capital is remembered, if not specified otherwise. | 1.0 |
| add_state_claim | / The state to add a claim to. | add_state_claim = 345 | Adds a claim for the current scope on the specified state. |  | 1.0 |
| remove_state_claim | / The state to remove the claim from. | remove_state_claim = 345 | Removes a claim of the current scope from the specified state. |  | 1.0 |
| set_state_owner | / The state to change ownership of. | set_state_owner = 345 | Makes the current scope the owner of the specified state. | This can fail to carry over the control, so it's recommended to instead use transfer_state unless transferring the ownership without transferring over the control. | 1.0 |
| set_state_controller | / The state to change controller of. | set_state_controller = 345 | Makes the current scope the controller of the specified state. |  | 1.0 |
| add_contested_owner | / State to contest. | add_contested_owner = 42 | Adds a contested owner to a state. The effect can be used either from a country or a state scope and accepts the other as parameter. | Can also be used in state scope. | 1.15 |
| remove_contested_owner | / State to stop contest. | remove_contested_owner = 42 | Removes a contested owner to a state. The effect can be used either from a country or a state scope and accepts the other as parameter. | Can also be used in state scope. | 1.15 |
| transfer_state | / The state to change owner and controller of. | transfer_state = 345 | Makes the current scope the owner and controller of the specified state. | transfer_state_to exists as a state-scoped variant. | 1.0 |
| set_province_controller | The province to change controller of. | set_province_controller = 2999 | Changes the controller of the specified province to the current scope. | A peace conference or the controller being at peace will reset the control of the province to the owner unless the controller is at war with the owner. | 1.0 |

### Mana

Mana in this usage means political power, stability, war support, and other values in the topbar. Fuel is, instead, in the [resources section](#Resources), while convoys can be added/removed with [add_equipment_to_stockpile](#add equipment to stockpile).

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_political_power | / The amount to add. | add_political_power = 100 add_political_power = var:my_var | Adds the specified amount of political power to the current scope. |  | 1.0 |
| set_political_power | / The amount to add. | set_political_power = 100 | Sets the specified amount of political power for the current scope. |  | 1.0 |
| add_stability | / The amount to add. | add_stability = 0.1 | Adds to the current stability value for the current scope. | Stability values are between 0 and 1. | 1.5 |
| set_stability | / The amount to add. | set_stability = 0.5 | Sets the current stability value for the current scope. | Stability values are between 0 and 1. | 1.5 |
| add_war_support | / The amount to add. | add_war_support = 0.1 | Adds to the current war support value for the current scope. | War Support values are between 0 and 1. | 1.5 |
| set_war_support | / The amount to set. | set_war_support = 0.5 | Sets the current war support value for the current scope. | War Support values are between 0 and 1. | 1.5 |
| add_command_power | / The amount to add. | add_command_power = 100 | Adds the specified amount of command power to the current scope. |  | 1.5 |
| add_manpower | / The amount to add. | add_manpower = 100000 add_manpower = var:my_var | Adds the specified amount of manpower to the current scope. |  | 1.0 |
| army_experience | / The amount to add. | army_experience = 10 | Adds the specified amount of army experience to the current scope. |  | 1.0 |
| navy_experience | / The amount to add. | navy_experience = 10 | Adds the specified amount of navy experience to the current scope. |  | 1.0 |
| air_experience | / The amount to add. | air_experience = 10 | Adds the specified amount of air experience to the current scope. |  | 1.0 |

### Politics

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_politics | ruling_party = The party to set. elections_allowed = Whether elections are allowed. Optional. last_election = When the last election was. Optional. election_frequency = How often in months an election occurs. Optional. long_name = The long name of the country's new ruling party, appearing when hovering over it. Optional. name = The name of the country's new ruling party. Optional. | set_politics = {     ruling_party = democratic     elections_allowed = no     last_election = "1935.12.17"     election_frequency = 48     long_name = TAG_party_long     name = TAG_party } | Sets the political status of the country, including the ruling party and elections. | Before 1.7, included parties = { ... } for assigning party popularities, which has been moved to set_popularities | 1.0 (updated 1.7) |
| set_popularities | = /The popularity to set. | set_popularities = { 	democratic = 50 	neutrality = 15 	fascism = 30 	communism = 5 } | Sets the political party popularities for the current scope. | The popularities must add up to 100, otherwise the command will have no effect. | 1.7 |
| add_popularity | ideology = The party to change. If using a tag, uses that tag's ruling party. popularity =  / The amount of popularity to change. | add_popularity = {     ideology = fascism     popularity = -0.5 } | Adjusts the popularity for the specified party in the current scope. | Values used are 0 to 1.  You can use ideology = ROOT to increase the popularity of the currently ruling party. | 1.0 |
| set_political_party | ideology = The party to change. popularity = The amount of popularity to set. | set_political_party = {     ideology = fascism     popularity = 50 } | Sets the popularity for the specified political party in the current scope. |  | 1.0 |
| set_party_name | ideology = The party to change. long_name = The new full name for the party.name = The new short name for the party. | set_party_name = {     ideology = neutrality     long_name = GER_neutrality_party_kaiserreich_long     name = GER_neutrality_party_kaiserreich } | Changes the name of the specified political party for the current scope. | The name appears in the country politics/diplomacy view, the long name appears in the tooltip when hovering over the party. | 1.0 |
| hold_election | The country to hold an election for. | hold_election = ROOT | Executes the events in the on_new_term_election on action for the current scope. |  | 1.0 |

### Balance of power

Balance of power is defined in /Hearts of Iron IV/common/bop/*.txt files.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_power_balance | id = Balance of power to set/modify. left_side = The left side of the BoP. right_side = The right side of the BoP. set_default = Resets the BoP to the initial state defined in the file. Optional, defaults to false. set_value = The new value of the BoP. Optional, defaults to not changing the value. | set_power_balance = {     id = my_bop     left_side = my_bop_left_side     right_side = my_bop_right_side } | Sets a new balance of power or edits the existing one. | Necessary for a balance of power to appear. For the default state, initial_value, left_side, and right_side directly inside of the BoP are read. | 1.12 |
| remove_power_balance | id = Balance of power to modify. | remove_power_balance = {     id = my_bop } | Removes the balance of power in entirety. |  | 1.12 |
| add_power_balance_value | id = Balance of power to modify. value = The value to add. tooltip_side = The side to show in the tooltip. Optional. | add_power_balance_value = {     id = my_bop     value = -0.1     tooltip_side = my_bop_side } | Pushes the balance of power towards one side. |  | 1.12 |
| add_power_balance_modifier | id = Balance of power to modify. modifier = The static modifier to apply. | add_power_balance_modifier = {     id = my_bop     modifier = my_static_modifier } | Applies a balance of power modifier. |  | 1.12 |
| remove_power_balance_modifier | id = Balance of power to modify. modifier = The static modifier to apply. | remove_power_balance_modifier = {     id = my_bop     modifier = my_static_modifier } | Cancels a balance of power modifier. |  | 1.12 |
| remove_all_power_balance_modifiers | id = Balance of power to modify. | remove_all_power_balance_modifiers = {     id = my_bop } | Cancels all balance of power modifiers. |  | 1.12 |
| set_power_balance_gfx | id = Balance of power to modify. side = The side whose GFX to change. gfx = The sprite to change the GFX to. | set_power_balance_gfx = {     id = my_bop     side = my_bop_side     gfx = GFX_my_bop_side_new } | Changes the appearance of one of the sides within the balance of power. | Sprites are defined within /Hearts of Iron IV/interface/*.gfx files. | 1.12 |

### Diplomacy

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_major | Boolean. | set_major = yes | Makes the current scope a major country. |  | 1.0 |
| release | The target country. | release = GER | Releases the specified non-existent country as a free nation within the current country's owned states. | The effect does nothing if the country exists. All states that are cored by the specified country will be given to it. If the current country has a core on a state transferred to the released country, the core will be lost. If looking to make a subject into an independent nation, use set_autonomy. States that are owned but not controlled will be transferred to the released country, but won't be controlled by it. | 1.0 |
| release_on_controlled | The target country. | release_on_controlled = GER | Releases the specified non-existent country as a free nation within the current country's controlled states. | The effect does nothing if the country exists. All states that are cored by the specified country will be given to it. If the current country has a core on a state transferred to the released country, the core will be lost. | 1.9.1 |
| release_puppet | The target country. | release_puppet = GER | Releases the specified non-existent country as a puppet of the current scope within the current country's owned states. | The effect does nothing if the country exists. All states that are cored by the specified country will be given to it. If the current country has a core on a state transferred to the released country, the core will be lost. States that are owned but not controlled will be transferred to the released country, but won't be controlled by it. | 1.0 |
| release_puppet_on_controlled | The target country. | release_puppet_on_controlled = GER | Releases the specified non-existent country as a puppet of the current scope within the current country's controlled states. | The effect does nothing if the country exists. All states that are cored by the specified country will be given to it. If the current country has a core on a state transferred to the released country, the core will be lost. | 1.9.1 |
| release_autonomy | target =  / The subject country. autonomy_state = The type of autonomy state to set. freedom_level = The new freedom level value. Optional. | release_autonomy = {     target = VIN     autonomy_state = autonomy_puppet     freedom_level = 0.5 } | Releases the specified non-existent country as a subject of the specified autonomy of the current scope within the current country's owned states. | The effect does nothing if the country exists. All states that are cored by the specified country will be given to it. If the current country has a core on a state transferred to the released country, the core will be lost. States that are owned but not controlled will be transferred to the released country, but won't be controlled by it. The autonomy states are found in /Hearts of Iron IV/common/autonomous_states/*.txt. | 1.3 |
| give_guarantee | The target country. | give_guarantee = GER | The current scope guarantees the target country. | diplomatic_relation effect can be used to remove it. | 1.0 |
| give_military_access | The target country. | give_military_access = GER | The current scope grants military access to the target country. | diplomatic_relation effect can be used to remove it. | 1.0 |
| recall_attache | The target country with an attache. | recall_attache = GER | Recalls the current scope's attaché from the specified country. |  | 1.5 |
| diplomatic_relation | country = The target country to alter the relationship with ROOT. relation = The relation to change.active = Whether the relation is started or broken. | diplomatic_relation = {     country = SOV     relation = guarantee     active = no } | Used to define a diplomatic relation between the current scope and target scope country. | Possible relations: non_aggression_pact guarantee puppet military_access docking_rights embargo air_base_access | 1.0 |
| add_opinion_modifier | target = The target country. modifier = The opinion modifier to add. | add_opinion_modifier = {     target = GER     modifier = faction_traitor } | The current scope gains the specified opinion modifier towards the target scope. Can also be used to modify trade relations by adding 'trade = yes' in the opinion  in /Hearts of Iron IV/common/opinion_modifiers/*.txt. If used with a trade opinion_modifier the behaviour is reversed, meaning that the target gains the trade opinion towards the current scope. | Opinion modifiers are found in /Hearts of Iron IV/common/opinion_modifiers/*.txt. | 1.0 |
| remove_opinion_modifier | target = The target country. modifier = The opinion modifier to remove. | remove_opinion_modifier = {     target = GER     modifier = faction_traitor } | The current scope loses the specified opinion modifier towards the target scope. | Opinion modifiers are found in /Hearts of Iron IV/common/opinion_modifiers/*.txt. | 1.0 |
| reverse_add_opinion_modifier | target = The target country. modifier = The opinion modifier to add. | reverse_add_opinion_modifier = {     target = GER     modifier = faction_traitor } | The target scope gains the specified opinion modifier towards the current scope. | Opinion modifiers are found in /Hearts of Iron IV/common/opinion_modifiers/*.txt.Useful for when you don't know what the current scope will be. | 1.0 |
| add_relation_modifier | target = The target country. modifier = The relation modifier to add. | add_relation_modifier = {     target = SWE     modifier = HUN_dynastic_ties_license } | The current scope gains the specified relation modifier towards the target scope. | Relation modifiers are found in /Hearts of Iron IV/common/modifiers/*.txt files, used to apply a targeted modifier with a non-static target. To change the diplomatic opinion of a country, see add_opinion_modifier. | 1.4 |
| remove_relation_modifier | target = The target country. modifier = The relation modifier to remove. | remove_relation_modifier = {     target = SWE     modifier = HUN_dynastic_ties_license } | The current scope loses the specified relation modifier for towards the target scope. | Relation modifiers are found in /Hearts of Iron IV/common/modifiers/*.txt, used to apply a targeted modifier with a non-static target. To change the diplomatic opinion of a country, see remove_opinion_modifier. | 1.4 |
| add_collaboration | target = The target country. value = <0-1>How much collaboration to add. | add_collaboration = {     target = TAG     value = 0.3 } | Adds collaboration in TAG with the scoped country. |  | 1.9 |
| set_collaboration | target = The target country. value = <0-1>How much collaboration will be set. | set_collaboration = {     target = TAG     value = 0.3 } | Sets the collaboration in TAG with the scoped country. |  | 1.9 |
| recall_volunteers_from | The target country. | recall_volunteers_from = SPR | Recalls volunteers sent to the specified country back to the current country. |  | 1.9 |
| set_occupation_law | The new occupation law enacted by the previous scope or default_law. | USA = {   GER = {     set_occupation_law = foreign_civilian_oversight   } }# Changes USA's occupation law for GER. USA = {   USA = {     set_occupation_law = default_law   } }# Changes the USA's default occupation law to the default. | Sets the occupation law of the country. | PREV will be the country for whom the occupation law will be changed. If PREV is not a country, nothing changes. If PREV is the same country, changes the default occupation law. If PREV is different, default_law resets the country-specific law to the global default, otherwise it resets the default law to the occupation law with starting_law = yes in definition. Can also be used in state scope. | 1.12 |
| set_occupation_law_where_available | The new occupation law enacted by the previous scope or default_law. | USA = {   GER = {     set_occupation_law_where_available = foreign_civilian_oversight   } }# Changes USA's occupation law for GER where possible. USA = {   USA = {     set_occupation_law_where_available = default_law   } }# Changes the USA's default occupation law to the default where possible. | Sets the occupation law of the country. | Identical to set_occupation_law, except if the law is impossible to set, tries again at every smaller sub-set: if default is impossible, tries every single individual occupied country; if the country's law is impossible to change, tries every single state within the country. | 1.12 |
| send_embargo | The target country. | send_embargo = ITA | Embargos the target country. |  | 1.12 |
| break_embargo | The target country. | break_embargo = ITA | Stops embargoing the target country. | As of 1.14.7, this effect ignores country scoping and always applies to the ROOT, instead the diplomatic_relation effect can be used to break the embargoes of other countries. | 1.12 |
| give_market_access | The target country. | give_market_access = ITA | Opens market access between the two countries. |  | 1.13 |

### Faction

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| create_faction | The name of the faction. | create_faction = MY_FACTION_NAME | Creates a faction with the specified name for the current scope. The current scope and any subjects automatically join the faction. | OBSOLETE, use create_faction_from_template. | 1.0 |
| create_faction_from_template | Faction template id. OR template = The template of the faction. name = The name of the faction. icon = The icon of the faction. color = The color of the faction in RGB format. | create_faction_from_template = faction_template_GER_mitteleuropa_alliance create_faction_from_template = {    template = faction_template_defensive_democratic    name = AUS_alpine_federation    icon = GFX_faction_logo_generic_2    color = { 100 100 150 } } | Create a faction from a template allows for optional customization of name, icon and color. |  | 1.17 |
| add_to_faction | The TAG of the nation to add to the faction of the current scope. | add_to_faction = GER | Adds the country to the faction of the current scope. |  | 1.0 |
| dismantle_faction | Boolean. | dismantle_faction = yes | Dismantles the faction of the current scope. |  | 1.0 |
| leave_faction | Boolean. | leave_faction = yes | Removes the current scope from the faction they are part of. |  | 1.5 |
| remove_from_faction | The target country. | remove_from_faction = GER | Removes the specified scope from the faction led by the current scope. |  | 1.0 |
| set_faction_name | Sets a faction name as the loc name. | set_faction_name = SOME_LOC_KEY | Changes faction names. |  | 1.6 |
| set_faction_leader | Boolean. | set_faction_leader = yes | Sets the current country as the faction leader. |  | 1.0 |
| set_faction_spymaster | Boolean. | set_faction_spymaster = yes | Sets the current country as the faction spymaster. |  | 1.9 |
| set_faction_rule | Faction rule id. | set_faction_rule = rule_id | Set a rule on the country's faction. |  | 1.17 |
| set_faction_manifest | Faction manifest id. | set_faction_manifest = faction_manifest_id | Changes current country's faction manifest, the previous manifest is removed. |  | 1.17 |
| add_faction_goal | The goal of the faction. | add_faction_goal = faction_goal_an_armored_fist | Adds a goal to the current’s country faction. |  | 1.17 |
| remove_faction_goal | The goal of the faction. | remove_faction_goal = faction_goal_secure_the_oil_supply | Remove a goal from the current’s country faction. |  | 1.17 |
| add_faction_goal_slot | category = The category of the faction goal. value =  / A value of the faction goal slot. | add_faction_goal_slot = {     category  = short_term     value = 1  } | Adds extra goal slots to the faction for a specific category. |  | 1.17 |
| add_faction_influence_ratio | / The amount to add. | add_faction_influence_ratio = 0.075 | Adds influence to the country based on the given ratio of the faction’s total influence. |  | 1.17 |
| add_faction_influence_score | / The amount to add. | add_faction_influence_score = 5 | Adds influence to the country in the faction. |  | 1.17 |
| add_faction_initiative | / The amount to add. | add_faction_initiative = 1 | Adds Faction Initiative points to the current country’s faction. |  | 1.17 |
| add_faction_power_projection | / The amount to add. | add_faction_power_projection = 100 | Adds power projection to the faction. |  | 1.17 |
| set_faction_upgrade | Faction upgrade id. | set_faction_upgrade = token | Set either a member upgrade for the specified tag. |  | 1.17 |
| set_faction_member_upgrade_min | upgrade = Faction upgrade id. | set_faction_member_upgrade_min = {     upgrade = TOKEN_TO_FACTION_MEMBER_UPGRADE } | Set a faction's minimal requirements for an faction member upgrade group. |  | 1.17 |
| set_faction_military_unlocked | Boolean. | set_faction_military_unlocked = yes | Sets wheter the current countries faction can make changes to the faction research section. |  | 1.17 |
| set_faction_research_unlocked | Boolean. | set_faction_research_unlocked = yes | Sets wheter the current countries faction can make changes to the faction research section. |  | 1.17 |

### Autonomy

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| puppet | The target country. OR target = The target country. end_wars = Whether the target country will peace out in all of its non-civil wars it's participating in. Defaults to true. end_civil_wars = Whether the target country will peace out in all of its civil wars it's participating in. Defaults to true. | puppet = GER puppet = {     target = ITA     end_wars = no } | Makes the specified country a subject of the current scope. | The autonomous state picked is one which contains default = yes and where allowed = { ... } is fulfilled within the /Hearts of Iron IV/commmon/autonomous_states/ definition, rather than necessarily being autonomy_puppet. Results in a crash-to-desktop if the game is unable to find any such autonomous states. | 1.0 |
| end_puppet | The target country. | end_puppet = GER | Removes the subject status between the target and the current scope. | Must be used within the overlord's scope. | 1.0 |
| add_autonomy_ratio | value = The freedom score to add. localization = The localization key for the modifier. | add_autonomy_ratio = {     value = 0.1     localization = AST_adopt_westminster } | Adds a freedom score ratio modifier to the current scope. | Used in the subject's scope. | 1.3 |
| add_autonomy_score | value = The freedom score to add. localization = The localization key for the modifier. | add_autonomy_score = {     value = 10     localization = EXAMPLE } | Adds an exact freedom score modifier to the current scope. | Used in the subject's scope. | 1.3 |
| set_autonomy | target =  / The subject country. autonomous_state = The type of autonomy state to set. freedom_level = The new freedom level value. Optional. end_wars = Will end any wars the subject is involved in. end_civil_wars = Will end any civil wars the subject is subject to | set_autonomy = {     target = AST     autonomous_state = autonomy_free     end_wars = no     end_civil_wars = no } | Sets the autonomy level for the specified country, including independence. | The autonomy_free state will free the subject, however this effect has to be executed within the scope of the target country's current overlord for this to have effect. The autonomy states are found in /Hearts of Iron IV/common/autonomous_states/*.txt files. Although end_wars is an optional argument defaulting to no, omitting it results in the country's occupied states returning to its control, stranding enemy units.When setting the autonomy level in the history files, it is preferable to do it before the political effects in the subject's history file scoping to the overlord's tag, to avoid overwriting. | 1.3 |

### Governments in exile

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_legitimacy | Adds legitimacy to a government in exile. | add_legitimacy = 10 | Adds legitimacy. |  | 1.6 |
| set_legitimacy | Sets the legitimacy of governments in exile. | set_legitimacy = 10 | Sets legitimacy. |  | 1.6 |
| become_exiled_in | Makes a country a government in exile in a set country, with a set starting legitimacy. | become_exiled_in = { target =  legitimacy = <0-100> (starting legitimacy, optional) } | Creates a government in exile. | Must be fired from ROOT, the country that should be exiled, or a TAG specification must be used. This effect would not automatically force a country to capitulate. | 1.6 |
| end_exile | Ends a government in exile. | end_exile = yes | Ends a government in exile. |  | 1.6 |

### War

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_threat | The amount to change by. | add_threat = 10 | Adjusts the level of World Tension. |  | 1.0 |
| add_named_threat | threat = The amount to change by. name = The localization string. | add_named_threat = {     threat = 5     name = GER_rhineland } | Adjusts the level of World Tension and adds an entry in the World Tension tooltip. |  | 1.0 |
| annex_country | target = Which country to annex. transfer_troops = yesWhether to transfer the troops of the annexed country. | annex_country = {     target = GER     transfer_troops = yes } | Annex the specified country for the current scope. | Without transfering troops, the annexed country's divisions' equipment is lost. | 1.0 |
| add_to_war | targeted_alliance = The country to assist. enemy = The country attacking the ally.hostility_reason = Localization for the reason for joining. Optional. | add_to_war = {     targeted_alliance = PREV     enemy = HUN     hostility_reason = asked_to_join } | Forces the current scope to join the war of the specified ally against the specified enemy. |  | 1.0 |
| declare_war_on | target =  / The country to attack. type = The wargoal to declare with.generator = {  }The states to supply the wargoal (i.e. take_state_focus). | declare_war_on = {     target = GER     type = annex_everything } | Makes the current scope declare war on the specified country with the specified wargoal. | Wargoals are found in /Hearts of Iron IV/common/wargoals/*.txt. See also add_civil_war_target in order to assign a war between different countries to be a civil war. | 1.0 |
| white_peace | / The scope to white peace. OR tag =  / The scope to white peace. message = The reason for peace showing up in the pop-up. | white_peace = GER  white_peace = {     tag = GER     message = my_peace_tt } | Makes the current scope white peace the specified scope. |  | 1.0 |
| start_peace_conference | tag =  / The scope to peace with. score_factor =  / The fraction of the total score awarded to the winners compared to regular victory. message = The reason for peace showing up in the pop-up. Optional. winner_scope = Which countries should be present in the conference on the winner side alongside the current scope. Optional, defaults to LIMITED_FACTION. loser_scope = Which countries should be present in the conference on the loser side alongside the target country. Optional, defaults to LIMITED_FACTION. | start_peace_conference = {     tag = GER     score_factor = 0.4     message = my_peace_tt } | Makes the current scope start a peace conference with the specified scope on the other side. | Current scope is the winner, target and its subjects are the losers. Can only be used if at war with the target. A score_factor of 0.0 is equivalent to a whitepeace. winner_scope and loser_scope have the following possible values: ALL: all countries at war with the other side. FACTION: all countries in the same faction as the current scope or under its overlordship. LIMITED_FACTION: includes faction members if and only if the country is a faction leader, and includes subjects of the country. LIMITED: includes only subjects of the country. | 1.12 |
| set_truce | target = The scope to truce with. days = The duration of the truce. | set_truce = {     target = GER     days = 90 } | Makes the current scope truce with the specified scope. |  | 1.0 |
| create_wargoal | target =  / The country to target. type = The wargoal to generate. generator = {  }The states to supply the wargoal (i.e. take_state_focus). expire = 365The amount of days that the wargoal will last before expiring. If unset or set to 0, will never expire. | create_wargoal = {     type = puppet_wargoal_focus     target = ROOT } create_wargoal = {     type = take_state_focus     target = PREV     generator = { 123 321 }     expire = 90 } | Grants the current scope a wargoal against the specified country. | Wargoal type can be found in /Hearts of Iron IV/common/wargoals/*.txt | 1.0 |
| remove_wargoal | target =  / The country to target. type = The wargoal to remove. "all" will remove all wargoals. | remove_wargoal = {     type = all     target = ROOT } | Removes wargoals from the current scope to the specified country. | Wargoal type can be found in /Hearts of Iron IV/common/wargoals/*.txt | 1.10.2 |
| start_civil_war | ideology = The ideology of the breakaway country. ruling_party = Changes the ideology of the original, player-led country, if set. Optional. size = The size of the breakaway country and the fraction of the original stockpile and military units it will receive by default. Optional, defaults to 0.5. army_ratio = The size of the land army that the breakaway country gets. Optional, defaults to being the same as size. navy_ratio = The size of the naval forces that the breakaway country gets. Optional, defaults to being the same as size. air_ratio = The size of the airforce that the breakaway country gets. Optional, defaults to being the same as size. capital = The capital state of the breakaway country. Optional. states = {  }The states included in the breakway country. Optional, defaults to random states based off size. all will result in all states that meet the filter going to the breakaway. states_filter = {  }A trigger block checked for the state that must be met to be transferred to the breakaway. Optional. keep_unit_leaders = {  }List of unit leaders to be kept by their legacy_id. Optional. keep_unit_leaders_trigger = {  }Trigger block checked for every unit leader that forces them to be kept if they meet the triggers. The default scope is the unit leader, ROOT is the country receiving the unit leader, while FROM is the original owner of the unit leader. Optional. keep_scientists_trigger = {  }Trigger for scientist to remain with the original country. keep_political_leader = Controls if the promoted party leader (i.e. the one that'd take power if the country were to be switched to that ideology group) of the revolting ideology group will be kept by the country or join the revolt, yes resulting in the former. Optional, defaults to false. keep_political_party_members = Controls if non-promoted party leaders of the revolting ideology group will be kept by the country or join the revolt, yes resulting in the former. Optional, defaults to false. keep_all_characters = yesIf true, the revolter will have no characters from the original country transferred to them. Optional, defaults to false. An effect block executed for the breakaway country. | start_civil_war = {     ruling_party = communism     # Original country's ideology changes to communism     ideology = ROOT     # Breakaway gets old ideology of ROOT     size = 0.8     capital = 282     states = {         282 533 536 555 529 530 528     }     keep_unit_leaders = {         750 751 752     }     keep_political_leader = yes     keep_political_party_members = yes }  start_civil_war = {     ideology = democratic     size = 0.1     states = all     states_filter = {         is_on_continent = europe         is_capital = no     }     set_country_flag = TAG_my_country_tag_alias_trigger     # Sets a country flag that gets used in a country tag alias. } (See country tag aliases)  start_civil_war = {     ideology = neutrality     size = 0.1     army_ratio = 0.5     navy_ratio = 0     air_ratio = 1     keep_unit_leaders_trigger = {         has_trait = my_trait_name     }     keep_all_characters = yes     PREV = {  # Original country         TAG_airforce_leader = { # Character             set_nationality = PREV.PREV             # Transfers to breakaway         }     }     promote_character = TAG_airforce_leader } (See usage for PREV and PREV.PREV) | Starts a civil war for the current scope with the specified parameters. | states = all would include every single state controlled by the country. If the country's current capital state is set as one of the states that the revolt can gain, it won't fire. set_capital can be used to change the capital beforehand, with on_civil_war_end being used to set it back to the default after the civil war ends. Elections will always be disallowed for the breakaway. If the ruling_party attribute is used, the original country will have its elections disallowed. In the base game files, an on action is set up to ensure that elections get allowed if the democratic side wins the civil war. A civil war started via this effect cannot have more than two sides and the effect cannot be used in history or bookmark's effect = { ... }. For adding more sides or starting one before the game's start, this can be simulated by setting an existing war (typically originating from a dynamic country created via create_dynamic_country) as a civil war via add_civil_war_target. | 1.0 |
| add_civil_war_target | - The country to set as the target. | add_civil_war_target = TAG | Sets that the war between ROOT and TAG is a civil war, resulting in the victory being the annexation of the other side and setting world tension limits on intervention. | ROOT and TAG must already be at war with each other for the effect to take place. | 1.9 |
| remove_civil_war_target | - The country to set as the target. | remove_civil_war_target = TAG | Removes the status of the war as a civil war between the pair of countries. | The ongoing war must already be marked as a civil war, whether it was initiated by start_civil_war or add_civil_war_target was used to mark it as one. | 1.12.13 |
| transfer_units_fraction | target = The country which should receive the units from the current scope. size = The size of the breakaway country and the fraction of the original stockpile and military units it will receive by default. Optional, defaults to 0.5. army_ratio = The size of the land army that the breakaway country gets. Optional, defaults to being the same as size. navy_ratio = The size of the naval forces that the breakaway country gets. Optional, defaults to being the same as size. air_ratio = The size of the airforce that the breakaway country gets. Optional, defaults to being the same as size. keep_unit_leaders = {  }List of unit leaders to be kept by their legacy_id. Optional. keep_unit_leaders_trigger = {  }Trigger block checked for every unit leader that forces them to be kept if they meet the triggers. The default scope is the unit leader, ROOT is the country receiving the unit leader, while FROM is the original owner of the unit leader. Optional. | transfer_units_fraction= { 	target = SPD 	size = 0.5 	stockpile_ratio = 0.8 	army_ratio = 0.8 	navy_ratio = 0.5 	air_ratio = 0.5 	keep_unit_leaders_trigger = { 		has_trait = trait_SPA_nationalist_sympathies 	} } | Transfers a fraction of the military to a target, including units (either type: land, navy, or air), equipment, and unit leaders. |  | 1.9 |
| add_nuclear_bombs | Adds nuclear bomb to TAG's stockpile. | add_nuclear_bombs = 100 | Adds specified number of nukes to the country's stockpile | Needs the Nuke tech to use. | 1.6 |
| launch_nuke | province = The specific province to nuke. state = The state to nuke. controller = Prioritises provinces controlled by this country. use_nuke = Whether a nuke should be deducted from the country's stockpile. Defaults to false. nuke_type = type of nuke to use (e.g. nuclear_bomb, thermonuclear_bomb etc.) | launch_nuke = {     province = 1234 } launch_nuke = {     state = 42     controller = GER     use_nuke = yes     nuke_type = nuclear_bomb  } | Nukes the specified province or a province in the needed state. If a state is set rather than the specific province, first prioritises the country set in controller, then prioritises the countries at war with the current scope, and then countries that are neutral. | If set to use a nuke, then requires at least one nuclear bomb in the stockpile. | 1.6 |

### Resources

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_resource | type = The resource to add. amount = The amount of resource to add. state = Which state to add the resource to. Variables can be used. show_state_in_tooltip = Whether the state should be shown in the tooltip. Defaults to true. | add_resource = {     type = oil     amount = 50     state = 88 } | Adds the specified resource in the specified amount to the specified state. | Can also be used in state scope. | 1.0 |
| create_import | resource = The resource to import. amount = The amount of resource to import.exporter = Which country exports the resource. | create_import = {     resource = steel     amount = 100     exporter = GER } | Creates an import for the current scope with the specified resource and from the specified exporter. |  | 1.0 |
| give_resource_rights | receiver = The country that would get the resource rights. state = The state where the resource rights are located. resources = {  <...>  }The resources to which give resource rights to. Optional, defaults to all. | give_resource_rights = { receiver = ENG state = 291 } give_resource_rights = {     receiver = POL     state = 321     resources = { oil } } | Gives all the resources of a state to the target country | The resource rights will only be provided as long as the current country controls the state with resource rights. | 1.6 |
| remove_resource_rights | The state to remove current country's resource rights from. | ENG = { remove_resource_rights = 477 } | Removes given resource rights |  | 1.6 |
| add_fuel | The fuel amount | add_fuel = 400 | Adds fuel to the current country. |  | 1.6 |
| set_fuel | Fuel amount. | set_fuel = 400 | Sets country's current fuel amount. |  | 1.6 |
| set_fuel_ratio | The needed ratio of fuel. | set_fuel_ratio = 0.5 | Set country's current fuel ratio relative to its capacity. |  | 1.6 |

### Buildings

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_offsite_building | type = The building to add. level =  / The maximum level to add. | add_offsite_building = { type = arms_factory level = 1 } | Adds an off-map (offmap) building for the current scope that produces its effects without being present in a state. |  | 1.5 |
| modify_building_resources | building = The building to modify. resource = The resource to add.amount = The amount of resource to add. | modify_building_resources = {     building = synthetic_refinery     resource = oil     amount = 1 } | Modifies the resource output of the specified building for the current scope. |  | 1.5 |
| damage_building | type = The building to damage. state =  / The state to target. tags = The buildings with this tag to damage. tags = {  }The buildings with these tags to damage. repair_speed_modifier =  Repair will be x% slower until building is fully repaired damage = The amount of damage to inflict. province =  / The province to target for provincal buildings. | damage_building = {   type = infrastructure   state = 123   damage = 1 } damage_building = {   tags = dam_building   damage = 1   repair_speed_modifier = -0.8   province = 3488 } | Damages a building in a targeted state or province. | The health of buildings is determined by the value attribute in a building's definition. This is multiplied by their level to get their total health. Can also be used in state scope. | 1.3 |

### National focuses

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| load_focus_tree | The national focus tree to load. OR tree = The national focus tree to load. keep_completed = Whether focuses shared between the old and new trees should stay completed. Defaults to false. copy_completed_from = Copy completed focus from an existing country. | load_focus_tree = china_communist_focus load_focus_tree = {   tree = british_focus   keep_completed = yes   copy_completed_from = ENG } | Loads a new focus tree for the current scope, retaining any shared focuses if set. | Focuses that aren't present in the newly-loaded tree will not be kept as completed for has_completed_focus checks or when loading the old tree back. | 1.5 |
| unlock_national_focus | The focus to unlock. | unlock_national_focus = my_focus | Bypasses the specified focus for the current scope (marks as complete without firing complete_effect of the focus). |  | 1.0 |
| complete_national_focus | The focus to complete. OR focus = The focus to complete. use_side_message = Create popup notification in the bottom right that includes originator_name instead of normal focus popup. originator_name = Used for tooltip only. | complete_national_focus = my_focus complete_national_focus = {   focus = GER_autonomous_organization_todt   use_side_message = yes   originator_name = GER_fritz_todt } | Completes the specified focus for the current scope. | In 1.15 block version was added, 'originator_name' can be TAG, character, state, or any other localization key. | 1.0 |
| uncomplete_national_focus | focus = uncomplete_children = Defaults "no". Optional.refund_political_power = Defaults "no". Optional. | uncomplete_national_focus = {   focus = GER_oppose_hitler   uncomplete_children = yes   refund_political_power = no } | Removes a focus from list of completed focus, and potentially all focuses requiring it as a prerequisite.If the focus has one, the 'on_uncomplete' effect will be executed on each uncompleted focus. |  | 1.11 |
| mark_focus_tree_layout_dirty | Boolean. | mark_focus_tree_layout_dirty = yes | Refreshes the focus tree for the specified country, restarting the checks in allow_branch and position offsets for focuses. | If put within a focus' completion reward, the focus will not be marked as complete at the time the effect is executed, leading to has_completed_focus checks specifying that focus in particular to be marked as false. This can be bypassed by putting an effect within a hidden event fired immediately within the focus or by reloading the same focus tree with load_focus_tree set to keep completed focuses, marking the focus as complete, before using the effect. | 1.9 |
| activate_shine_on_focus | The focus to activate a shine effect on. | activate_shine_on_focus = my_focus | Activates the shine effect on the focus with the given id. Focuses that are completed cannot have an activated shine effect. | Tooltips are only shown in debug mode. Can be used to simulate work on more than one focus at a time. | 1.15 |
| deactivate_shine_on_focus | The focus to deactivate a shine effect on. | deactivate_shine_on_focus = my_focus | Deactivate the shine effect on the focus with the given id. The current focus cannot have it's shine effect removed. | Tooltips are only shown in debug mode. | 1.15 |
| reduce_focus_completion_cost | focus = The focus to reduce cost time. cost =  / Time to reduce (in days). | reduce_focus_completion_cost = {   focus = focus_id   cost = 35 } reduce_focus_completion_cost = {   focus = {focus_id_1 focus_id_2}   cost = 35 } | Reduce the cost needed to complete a specific focus. The cost accepts script constants. The focus can be a uniform list or a single token. |  | 1.17 |

### Decisions

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| activate_decision | The decision to activate. | activate_decision = my_decision | Activates the specified decision for the current scope, ignoring triggers for the decision. | Decisions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.0 |
| activate_targeted_decision | target = The country to target. decision = The decision to activate. | activate_targeted_decision = {     target = GER     decision = my_decision } | Activates the specified targeted decision for the specified target for the current scope. | Decisions are found in /Hearts of Iron IV/common/decisions/*.txtOnly works on missions; regular decisions targeted this way become visible but do not activate. | 1.5 |
| remove_targeted_decision | The decision to remove. | remove_targeted_decision = {     target = FROM     decision = my_decision } | Removes the specified targeted decision for the current scope. | Decisions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.5 |
| unlock_decision_tooltip | The decision to display.  Show decision effects (default is no)  Show decision modifiers. (default is no) | unlock_decision_tooltip = my_decision unlock_decision_tooltip = {     decision = my_decision     show_effect_tooltip = yes     show_modifiers = yes } | Displays a special tooltip for the specified decision in the effect tooltip. | Decisions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.5 |
| unlock_decision_category_tooltip | The decision category to display. | unlock_decision_category_tooltip = my_category | Displays a special tooltip for the specified decision category in the effect tooltip. | Decision categories are found in /Hearts of Iron IV/common/decisions/catergories/*.txt | 1.5 |
| add_days_remove | decision =  The decision to add days to. days =  / The number of days to add to the decision. | add_days_remove  = {     decision = decision_here     days = 30 } | Adds the number of days to the timer created by a decision's days_remove. | Decisions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.9 |
| remove_decision | Allows to remove specified decision without running remove_effect. | remove_decision = GER_MEPO | Removes a decision. |  | 1.6 |
| remove_decision_on_cooldown | The decision that is to be removed. | remove_decision_on_cooldown = TAG_my_decision | If the decision is on cooldown, it gets removed, in order to reactivate or remove completely. |  | 1.11 |

### Missions

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| activate_mission | The mission to activate. | activate_mission = my_mission | Activates the specified mission for the current scope, ignoring any triggers for the decision. | Missions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.5 |
| activate_mission_tooltip | The mission to display. | activate_mission_tooltip = my_mission | Displays a special tooltip for the specified mission in the effect tooltip. | Missions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.5 |
| remove_mission | The mission to remove. | remove_mission = my_mission | Removes the specified mission for the current scope. | Missions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.5 |
| add_days_mission_timeout | mission =  The mission to add days to. days =  / The number of days to add to the mission. | add_days_mission_timeout = {     mission = my_mission     days = 20 } | Adds the number of days to the specified mission. | Missions are found in /Hearts of Iron IV/common/decisions/*.txt | 1.9 |

### Technologies

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_research_slot | The number of slots to add or remove. | add_research_slot = 1 | Adjusts the number of research slots the current scope has. Can remove slots with negatives. |  | 1.0 |
| set_research_slots | The number of slots to set. | set_research_slots = 4 | Sets the number of research slots the current scope has. |  | 1.0 |
| add_tech_bonus | bonus = The bonus to technology given, default 0. uses = The amount of times the bonus can be used, default 1.ahead_reduction = The cost reduction if ahead of time, default 0.category = Which technology category the bonus applies to. Multiple can be defined.technology = Which technology the bonus applies to. Multiple can be defined. name =  Tooltip shown in research tabs, optional. | add_tech_bonus = {     bonus = 0.5     uses = 1     category = radar_tech } | Grants a research bonus to the current scope with the specified parameters. | Research bonus categories are defined in /Hearts of Iron IV/common/technology_tags/*.txt files, while technologies are defined in /Hearts of Iron IV/common/technologies/*.txt files. | 1.0 |
| set_technology | = The technology to add.popup = noTo not show the popup after adding technology | set_technology = {     suicide_craft = 1 } | Grants the specified technology to the current scope. | A value of 1 sets the technology. A value of 0 removes the technology, but if it is a researchable technology, the duration it takes to research isn't reset, meaning it can be researched in 1 day. Technologies that are mutually exclusive with other technologies can not be removed by this effect. Technologies are defined in /Hearts of Iron IV/common/technologies/*.txt files. To show the effects of the technology use custom_effect_tooltip = tech_effect\| | 1.0 |
| add_to_tech_sharing_group | The group to add the current scope to. | add_to_tech_sharing_group = us_research | Adds the current scope to the specified technology sharing group. | Technology sharing groups are found in Hearts of Iron IV\common\technology_sharing\*.txt | 1.3 |
| remove_from_tech_sharing_group | The group to remove the current scope from. | remove_from_tech_sharing_group = us_research | Removes the current scope from the specified technology sharing group. | Technology sharing groups are found in Hearts of Iron IV\common\technology_sharing\*.txt | 1.3 |
| modify_tech_sharing_bonus | id = The group to modify. bonus = The new bonus. | modify_tech_sharing_bonus = {     id = us_research     bonus = 0.5 } | Modifies the specified technology sharing group. | Technology sharing groups are found in Hearts of Iron IV\common\technology_sharing\*.txt | 1.3 |
| inherit_technology | The country to inherit technology from. | inherit_technology = CAN | Makes the current country's researched technologies be copied from the specified country. | Useful when making a country independent. | 1.6 |
| mark_technology_tree_layout_dirty | Boolean. | mark_technology_tree_layout_dirty = yes | Forces the refresh of the hidden technologies for the scoped country. |  | 1.15 |

### Ideas

This includes national spirits, laws, designers, and advisors. (using the idea_token)

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_ideas | The idea to add. | add_ideas = my_idea add_ideas = {     my_idea_1     my_idea_2 } | Adds the specified ideas to the current scope. | Can be used as a scope to add multiple at once. | 1.0 |
| add_timed_idea | idea = The idea to add. days =  / The number of days to add the idea for. months =  / The number of months to add the idea for. A month is equal to 30 days. years =  / The number of years to add the idea for. A year is equal to 365 days. | add_timed_idea = {     idea = my_idea     days = 180 } | Adds the specified ideas to the current scope for the specified number of days. | Either one of days, months, or years is mandatory. The tooltip will use the exact same phrasing in years/months/days as used in the attributes. | 1.0 |
| modify_timed_idea | idea = The idea to modify. days =  / The number of days to modify the idea by. months =  / The number of months to modify the idea by. A month is equal to 30 days. years =  / The number of years to modify the idea by. A year is equal to 365 days. | modify_timed_idea = {     idea = my_idea     days = 60 } | Extends or shortens the duration of the timed idea by the specified amount. | Positives add to the time, negatives shorten it. Either one of days, months, or years is mandatory. The tooltip will use the exact same phrasing in years/months/days as used in the attributes. | 1.0 |
| swap_ideas | add_idea = The idea to add. remove_idea = The idea to remove. | swap_ideas = {     remove_idea = my_idea_1     add_idea = my_idea_2 } | Switches two ideas with a tooltip displaying any modifier differences between them. | If the ideas have the same name in the localisation, it will show up as modifying the idea rather than swapping them. The add will occur before the removal of the old idea. | 1.3 |
| remove_ideas | The idea to remove. | remove_ideas = my_idea remove_ideas = {     my_idea_1     my_idea_2 } | Removes the specified idea from the current scope. | Can be used as a scope to remove multiple at once. | 1.0 |
| remove_ideas_with_trait | The trait to target. | remove_ideas_with_trait = motorized_equipment_manufacturer | Removes all ideas for the current scope that use the specified trait. |  | 1.0 |
| show_ideas_tooltip | The idea to display. | show_ideas_tooltip = my_idea | Displays the specified idea in the tooltip for the current effect scope. Does not add the idea. |  | 1.0 |

### Units

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| load_oob | The filename of the order of battle to load, without the .txt extension. | load_oob = "GER_default" | Loads the specified order of battle for the current scope, applying the effects within. The filename with the .txt extension omitted is used as the effect's target. | Orders of battle are stored within /Hearts of Iron IV/history/units/*.txt. Primarily used to spawn divisions at specified locations. | 1.0 |
| division_template | nameThe name of the division. regiments = {      = { x = 0 y = 0 } } support = {      = { x = 0 y = 0 } }  The composition of the division. Sub-units are defined in /Hearts of Iron IV/common/units/*.txt files. division_names_group =  The division names group that the template will use, deciding on the automatically-generated names of any new divisions built using that template. Optional, assigns one automatically if omitted. These are defined within /Hearts of Iron IV/common/units/names_divisions/*.txt files. is_locked = Whether the division is locked to modification and deletion. Optional. force_allow_recruiting = Whether the locked template can have units deployed using it without allowing editing. Optional, only has an effect in locked templates. division_cap = The maximum amount of divisions that this template may have; requires the template to be locked. Optional. priority = The priority the template receives in receiving supplies. Goes from 0 to 2. Optional, 1 by default. template_counter = The icon used by the division as an integer. Optional, defaults to the icon of the most common sub-unit within. The icons are defined as sprites within any /Hearts of Iron IV/interface/*.gfx file (By default subuniticons.gfx) with the pattern of GFX_div_templ__large and GFX_div_templ__small. override_model = Enforces the entity used by the units using this template to be the specified one. Optional. | division_template = {     name = "Test"     is_locked = yes     division_cap = 3      division_names_group = USA_INF_01     priority = 0     template_counter = 0     regiments = {         infantry = { x = 0 y = 0 }         infantry = { x = 0 y = 1 }         infantry = { x = 0 y = 2 }         infantry = { x = 0 y = 3 }     }     support = {         military_police = { x = 0 y = 0 }     } } | Creates and adds the specified division template to the current scope. | The x and y attributes represent the rows and columns in the division designer and start from 0. No tooltip is shown. | 1.0 |
| create_colonial_division_template | subject = Country tag for an overlords subject. division_template = { ... }The regular effect to create a division template. | create_colonial_division_template = {   subject = RAJ   division_template = {     name = "Infantry Division"     division_names_group = RAJ_INF_01     ...     regiments = {       infantry = { x = 0 y = 0 }       infantry = { x = 0 y = 1 }      }   } } | Create a colonial division template for overlord/owner. | In country scope of overlord, E.g. ROOT = ENG. | 1.15 |
| add_units_to_division_template | template_name = The template to change. Optional if used in division scope. regiments = {      =  } support = {      =  } The units to add to the template. Sub-units are defined in /Hearts of Iron IV/common/units/*.txt files. | add_units_to_division_template = {     template_name = "Test"     regiments = {         infantry = 2         infantry = 2     }     support = {         military_police = 0     } } | Adds the specified brigades to first available slots of specified columns to the template (if possible). | Columns go left-to-right starting with 0. Can also be used in division scope. | 1.0 |
| set_division_template_lock | division_template = The name of the division template. is_locked = Whether the division is locked or not. | set_division_template_lock = {     division_template = "Infantry Division"     is_locked = yes } | Toggles the locked status on a division template for the current scope, which prevents editing or deletion. |  | 1.5 |
| country_lock_all_division_template | Boolean. OR is_locked = Boolean. desc = Tooltip. | country_lock_all_division_template = yes country_lock_all_division_template = {   is_locked = yes   desc = loc_key } | Locks all division templates for the current scope. | Used to prevent training, disbanding, and editing units. | 1.9 |
| set_division_force_allow_recruiting | division_template = Template to modify. force_allow_recruiting = Whether to allow or disallow recruiting. Defaults to true if unset. | set_division_force_allow_recruiting = {     division_template = "My locked template" } | Changes whether it's possible to recruit divisions of a locked template without unlocking the template. |  | 1.12 |
| set_division_template_cap | division_template = The name of the division template. division_cap = The division cap. | set_division_template_cap = {  	division_template = "Swiss Citizen Militia"  	division_cap = SWI_militia_division_cap } | Sets the cap of a division template. The template has to be locked first. |  | 1.12 |
| clear_division_template_cap | division_template = The name of the division template. | clear_division_template_cap = {  	division_template = "Swiss Citizen Militia" } | Clears the cap on the template, allowing it to have an unlimited amount of divisions. |  | 1.12 |
| delete_unit_template_and_units | division_template = The name of the division template. | delete_unit_template_and_units = {     division_template = "Infantry Division"     disband = yes #will refund equipment and manpower } | Deletes the specified division template and all units using it for the current scope. |  | 1.5 |
| delete_unit | state = The id number of the state the unit must be in. division_template = The template the units must use to be deleted. id = The id given to the unit if created via the create_unit effect. disband = If true, will refund equipment and manpower. | delete_unit = {     state = 787     disband = yes #will refund equipment and manpower } delete_unit = {     division_template = "Infantry Division" } delete_unit = {} # Will delete all units | Deletes all units that meet the filters. | No tooltip is generated. delete_units can be used if deleting all units of a specific template. | 1.5 |
| delete_units | division_template = The template the units must use to be deleted. disband = If true, will refund equipment and manpower. | delete_units = {     division_template = "Infantry Division"     disband = yes } | Deletes all units with a certain template. | Generates a tooltip, unlike delete_unit. Mandatory to specify a division_template. | 1.9 |
| create_railway_gun | equipment = Equipment type used by the railway gun. name = The name used by the railway gun. Optional. location = Location where the railway gun is created. Assumes the capital by default. | create_railway_gun = {     equipment = railway_gun_equipment_1 	name = TAG_new_railway_gun 	location = 12406 } | Creates a railway gun. |  | 1.11 |
| teleport_railway_guns_to_deploy_province | Boolean. | teleport_railway_guns_to_deploy_province = yes | Teleports all railway guns to the province where they get deployed. |  | 1.11 |
| add_unit_bonus | = { ... } | add_unit_bonus = {   category_light_infantry = {     soft_attack = 0.05   }    cavalry = {     soft_attack = 0.05     hard_attack = 0.05   } } | Adds permanent subunit and subunit category bonuses for country. |  | ??? |

### Equipment

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_equipment_fraction | / The fraction of equipment to remove. | set_equipment_fraction = 0.5 | Reduces the overall equipment stockpile by the specified fraction. | This should not be used in civil wars to simulate stockpile splitting. start_civil_war automatically divides stockpiles according to the respective size. | 1.0 |
| add_equipment_to_stockpile | type = The equipment to add. Either types and archetypes are accepted. amount =  / The amount to add. producer =  / Defines who produced the equipment. Optional, defaults to the current scope. variant_name = The equipment variant to add. Mandatory if a variant needs to be created to produce the equipment, optional otherwise. | add_equipment_to_stockpile = {     type = infantry_equipment     amount = -100     producer = GER } add_equipment_to_stockpile = {     type = medium_tank_chassis_1     amount = 100     variant_name = "Panzer III" } | Edits the equipment stockpile of the current scope, adds or removes equipment of a specified type or archetype. | With negative numbers, optionally specifying a producer will ensure only equipment with that producer gets removed. The equipment must be unlocked by the producer for the effect to succeed. | 1.0 |
| send_equipment | type = The equipment to add. Can be archetype. amount =  / The amount to add.target =  / Which country receives the equipment. | send_equipment = {     equipment = infantry_equipment     amount = 100     target = GER } | Sends the specified amount of equipment to the specified target, removing said equipment from the current scope. | Cannot remove equipment into negatives, in which case equipment will not be received by the target in entirety. | 1.0 |
| send_equipment_fraction | value = <0-1>How much equipment to send. target =  / Which country receives the equipment. | send_equipment_fraction = {     value = 0.3     target = GER } | Sends the specified fraction of equipment to the specified target, removing said equipment from the current scope. |  | 1.9 |
| create_production_license | target = Which country receives the license. cost_factor = Modifies the production cost. Equipment scopetype = The equipment the country is licensed to produce. Must be an non-archetype equipment. version = The version indicates which variant should be licensed. The default is 0, meaning the base variant. new_prioritised = Whether new equipment is prioritised or not. Yes by default. | create_production_license = {     target = HUN     equipment = {         type = fighter_equipment_1         version = 0         new_prioritised = no     }     cost_factor = 0 } | Grants the specified country a license to produce the specified equipment from the current scope. |  | 1.4 |
| add_equipment_subsidy | cic = The amount of economic capacity required by the subsidy. equipment_type = The equipment archetype that the subsidy is for. seller_tags = { Countries that can have the subsidy. seller_trigger = The trigger deciding which countries can have the subsidy. | add_equipment_subsidy = {     cic = 300     equipment_type = support_equipment     seller_tags = { BHR } } add_equipment_subsidy = {     cic = 1000     equipment_type = infantry_equipment     seller_trigger = my_scripted_trigger } | Creates an equipment subsidy on the international market. | seller_tags and seller_trigger are mutually exclusive. In the scripted trigger, ROOT is the country with the subsidy and FROM is the seller. | 1.13 |
| add_cic | The amount of economic capacity to add. | add_cic = 300 | Modifies the economic capacity bank on the international market. | The economic capacity will be capped to 0 if the total after the effect is negative. | 1.13 |
| create_equipment_variant | name = The name of the variant. type = The equipment type the variant is of. parent_version = Ordering for multiple variants of the same equipment. 0 is the oldest, 1 is the second-oldest, etc. Optional, 0 by default. show_position = Dynamic equipment version numbering. If disabled removes the suffix from the equipment name. Numbering linked to parent_version. Optional, yes by default. obsolete = Whether the equipment variant is flagged as obsolete within the GUI and for AI. Optional, no by default. mark_older_equipment_obsolete = Marks all older (non-chassis) equipment variants as obsolete as long as the following matches: Archetype, niche, mission set (for planes). Optional, defaults to false. name_group = The name group used for equipment. Stored in /Hearts of Iron IV/common/units/names_ships. Optional, can only be defined for ships. role_icon_index = /autoIndex of the role icon that will be used, as an integer. If set to "auto", will pick automatically. If set to 0, will be unset. Optional, only can be defined for ships. model = Model that will be used by the equipment on the world map. Optional. icon = The icon that will be used by equipment. Stored as a spriteType within /Hearts of Iron IV/interface/*.gfx. Optional. design_team = mio:The military industrial organisation that should be set as the designer of the equipment. Optional. allow_without_tech = If set, bypasses the requirement that the equipment that the variant is for must be unlocked through research. Optional, defaults to false. Upgrade scope = The upgrades configuration for the variant. Module scope = The modules configuration for the variant. | create_equipment_variant = {     name = "Vetehinen Class"								     type = ship_hull_submarine_1     name_group = FIN_SS_HISTORICAL     role_icon_index = 1     modules = {         fixed_ship_torpedo_slot = ship_torpedo_sub_1         fixed_ship_engine_slot = sub_ship_engine_1         rear_1_custom_slot = ship_mine_layer_sub     } } create_equipment_variant = {     name = "He 112"     type = fighter_equipment_0     obsolete = yes     upgrades = {         plane_gun_upgrade = 1         plane_range_upgrade = 1     } } create_equipment_variant = {     name = "Light Tank Mk. IV"     type = light_tank_chassis_1     parent_version = 1     modules = {         main_armament_slot = tank_heavy_machine_gun     }     upgrades = {         tank_nsb_engine_upgrade = 2     }     icon = "GFX_ENG_basic_light_tank_medium"     model = ENG_MKIV_light_tank_entity     design_team = mio:ENG_vauxhall_organization } | Creates the specified equipment variant for the current scope. | Role icons for ships are defined in /Hearts of Iron IV/gfx/army_icons/army_icons.txt. Upgrades are defined within /Hearts of Iron IV/common/units/equipment/upgrades/*.txt. Equipment types, including module slots for them, are defined within /Hearts of Iron IV/common/units/equipment/*.txt. Equipment modules are defined within /Hearts of Iron IV/common/units/equipment/modules/*.txt. | 1.0 |
| add_equipment_production | amount = The amount to produce before automatically stopping. Optional. requested_factories = The number of factories to assigned initially. Optional. progress = The initial production progress. Optional. efficiency = The initial production efficiency. Optional. name = The name that'll be used for the equipment, such as with ships. Optional. industrial_manufacturer = mio:The military industrial organisation that's set as the equipment's designer. Equipment scope type = The name of the equipment to produce. creator = The country which is producing the equipment. Used if root scope isn't producer. Optional. version_name = The name of the variant to produce. Optional. | add_equipment_production = {     equipment = {         type = light_cruiser_2     }     requested_factories = 1     progress = 0.95     amount = 1 } | Starts a production line for the specified equipment for the current scope. |  | 1.0 |
| add_design_template_bonus | name = Name. uses = The amount of times the discount can be used. cost_factor = Discount. equipment = Can be equipment type and archetype. | add_design_template_bonus = {   name = air_equipment   uses = 1   cost_factor = 0.75   equipment = small_plane_airframe   equipment = medium_plane_airframe   equipment = large_plane_airframe } | Add free bonus design discount to given types with a set of uses. | The value for uses and cost_factor can either be an absolute value or a script constant. Can use several equipment types, where 1 is mandatory. | 1.15 |
| add_equipment_bonus | project = <>Optional, special project scope for using special project name. If not set, the name will be used. name = Name. bonus = { ... }Bonus. | add_equipment_bonus = {   project = FROM   bonus = {     armor = { # Type of equipment       armor_value = 3       soft_attack = 3       instant = yes     }     small_plane_naval_bomber_airframe = {       air_range = 0.1       naval_strike_attack = 0.1     }   } } | Adds the specified equipment bonuses to the country. As description the given loc key or the name of given special project will be used. Same usage as in Ideas/National spirits. |  | 1.15 |
| set_equipment_version_number | type = Equipment type. version = Version to set. | set_equipment_version_number = {   type = small_plane_airframe_1   version = 4 } | Changes current version number for a given equipment type to N. The next equipment variant created from that type will have version number N+1. | Set "Variant max version" to specified version. Provides no tooltip. | 1.16 |

### Military

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| destroy_ships | type = The type of ship to destroy. count =  or allThe amount to destroy. | destroy_ships = {     type = destroyer     count = all } | Destroys the specified type and amount of ships controlled by the current scope. |  | 1.5 |
| transfer_navy | target = The target country. | transfer_navy = {     target = GER } | Transfers the current scope navy to the specified country. |  | 1.5 |
| transfer_ship | type = The type of ship to transfer. target = The target country. prefer_name = Name of ship in origin navy that will preferably be transferred to target navy. Optional. exclude_refitting = Determines whether ships that are being refitted will be transferred. Optional. | transfer_ship = {     prefer_name = "HMS Achilles"     type = light_cruiser     target = NZL     exclude_refitting = no } | Transfers the specified type of ship from the current scope to the specified country. |  | 1.4 |
| create_ship | type = The type of ship to create. equipment_variant = The equipment variant to use. creator = The country that created this ship. Optional. name = Name of the ship. Optional. amount = The amount of ships to create. Optional, defaults to 1. | FRA = {     create_ship = {         type = ship_hull_submarine_1         equipment_variant = "S Class"         creator = ENG         name = "My ship name"     } } | Create a ship from another country and assign it to the reserve fleet. If not set, it will be the scoped country. |  | 1.9 |
| add_mines | Add mines to a strategic region for the current country. | add_mines = { region = 42 amount = 100 } | Add mines to a strategic region. |  | 1.6 |
| add_ace | name = The name of the ace. surname = The surname of the ace. callsign = The callsign of the ace. type = The ace type. is_female = The gender of the ace. | add_ace = {     name = "Amelia"     surname = "Earhart"     callsign = "Revenant"     type = fighter_genius     is_female = yes } | Adds an ace for the current scope. | Ace types found in /Hearts of Iron IV/common/aces/*.txt. | 1.0 |
| unlock_tactic | Tactic to unlock. | unlock_tactic = tactic_masterful_blitz | Unlocks the specified combat tactic for the country. |  | 1.17 |

### Doctrine

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_doctrine_cost_reduction | name = Optional tooltip showing why the doctrine has reduced cost in the doctrine menu. cost_reduction = Percentage of cost reduced. uses = Number of times the cost reduction can be used. category = Which doctrine category the cost reduction will apply to. (Ex: land_doctrine, air_doctrine.) | add_doctrine_cost_reduction = { 	cost_reduction = 0.5 	uses = 2 	category = land_doctrine } | Adds a limited use cost reduction for doctrines. | For a general doctrine cost reduction, see "_doctrine_cost_factor" in Modifiers. | 1.11 |
| add_mastery | amount = Amount of mastery to add. folder = Optional - will filter by tracks in the specified folder. grand_doctrine = Optional - will filter by tracks in folders with the specified grand doctrine. sub_doctrine = Optional - will filter by tracks with the specified subdoctrine. track = Optional - will filter by tracks of the specified type. index = Optional - will filter by the track index within the folder (0-indexed). | add_mastery = {     amount = 100     # FILTERS:     folder = land     grand_doctrine = mobile_warfare     sub_doctrine = mobile_infantry     track = infantry     index = 1 } | Adds doctrine mastery. | You can use flexible filters to have this effect apply to all tracks that match the specified folder, grand doctrine, subdoctrine or specific track. If a certain filter is not present, it will be counted as a pass. For example, you can add mastery to all active tracks in all folders by not specifying any filters at all. | 1.17 |
| add_daily_mastery | amount = Amount of mastery to add per day. days = Number of days to apply the daily mastery gain for. name = Loc key - will be used in descriptions to show the source of the mastery gain. folder = Optional - will filter by tracks in the specified folder. grand_doctrine = Optional - will filter by tracks in folders with the specified grand doctrine. sub_doctrine = Optional - will filter by tracks with the specified subdoctrine. track = Optional - will filter by tracks of the specified type. index = Optional - will filter by the track index within the folder (0-indexed). | add_daily_mastery = {     amount = 0.5     days = 90     name = CHI_military_affairs_commission_sea     # FILTERS:     folder = land     grand_doctrine = mobile_warfare     sub_doctrine = mobile_infantry     track = infantry     index = 1 } | Adds doctrine mastery daily for a certain duration. | You can use flexible filters to have this effect apply to all tracks that match the specified folder, grand doctrine, subdoctrine or specific track. If a certain filter is not present, it will be counted as a pass.	For example, you can add mastery to all active tracks in all folders by not specifying any filters at all. | 1.17 |
| add_mastery_bonus | bonus = Bonus factor, e.g. 0.1 = +10% days = Number of days to apply the bonus mastery gain for. name = Loc key - will be used in descriptions to show the source of the mastery gain. folder = Optional - will filter by tracks in the specified folder. grand_doctrine = Optional - will filter by tracks in folders with the specified grand doctrine. sub_doctrine = Optional - will filter by tracks with the specified subdoctrine. track = Optional - will filter by tracks of the specified type. index = Optional - will filter by the track index within the folder (0-indexed). | add_mastery_bonus = {     bonus = 0.5     days = 90     name = CHI_military_affairs_commission_sea     # FILTERS:     folder = land     grand_doctrine = mobile_warfare     sub_doctrine = mobile_infantry     track = infantry     index = 1 } | Get a bonus to doctrine mastery gain for a certain duration. | You can use flexible filters to have this effect apply to all tracks that match the specified folder, grand doctrine, subdoctrine or specific track. If a certain filter is not present, it will be counted as a pass. For example, you can add mastery to all active tracks in all folders by not specifying any filters at all. | 1.17 |
| set_grand_doctrine | Grand doctrine id. | set_grand_doctrine = mobile_warfare | Activate (unlock and assign) the specified grand doctrine. |  | 1.17 |
| set_sub_doctrine | Subdoctrine id. OR sub_doctrine = Subdoctrine id. folder = Optional, in case you need to specify the folder. track = Optional, in case you need to specify the track index within the folder. Note that this is the track index (starting with 0) among ALL the tracks in the folder, not just the ones that match the subdoctrine. So in a case where a grand doctrine has the tracks: 'infantry - armor - armor - operations', you would use track = 1 to refer to the first armor track, and track = 2 to refer to the second armor track. | set_sub_doctrine = mobile_infantry set_sub_doctrine = {     sub_doctrine = mobile_infantry     folder = land     track = 1 } | Activate (unlock and assign) the specified subdoctrine. | By default, the subdoctrine is assigned to the first matching track that the system can find. However, you can also specify a specific folder and track index to assign the subdoctrine to, in case the same track appears in multiple folders, or multiple times in the same folder. | 1.17 |

### Intelligence

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| create_intelligence_agency | name = The name of the intelligence agency. (Optional) icon = The icon of the intelligence agency. (Optional) | create_intelligence_agency = {     name = "A.G.E.N.C.Y"     icon = GFX_intelligence_agency_logo_agency } create_intelligence_agency = yes | Creates an Intelligence Agency. | Both parameters are not required, thus you can call the effect with just create_intelligence_agency = yes. This will check if any specific intelligence agency cosmetics should be used for the nation, and if not it uses the default. | 1.9 |
| upgrade_intelligence_agency | Allows to unlock automatically an intelligence agency upgrade | upgrade_intelligence_agency = upgrade_form_department upgrade_intelligence_agency = | Unlocks an Intelligence Agency Upgrade. | Upgrades can be found in common/intelligence_agency_upgrades | 1.9 |
| add_decryption | target = Towards which country to add decryption. amount = How much decryption to add in flat numbers. ratio = <0-1>How much decryption ratio to add. | add_decryption = {     target = GER     amount = 300 } add_decryption = {     target = GER     ratio = 0.5 } | Adds decryption towards the target country | target and ratio arguments are mutually exclusive. | 1.9 |
| add_intel | target = Towards which country to add intelligence. civilian_intel = How much civilian intel to add. army_intel = How much army intel to add. navy_intel = How much navy intel to add. airforce_intel = How much airforce intel to add. | add_intel = {     target = GER     civilian_intel = 3     army_intel = 2     navy_intel = 1     airforce_intel = 2 } | Adds the specified amount of intel towards the specified country. | If an intel argument is left out, 0 is assumed. | 1.9 |
| add_operation_token | tag = Towards which country to add a token on. token = Which token to add. | add_operation_token = {     tag = GER     token = token_test } | Adds an operation token towards the country, allowing access to more intel or applying a targeted modifier. | Operation tokens are defined in /Hearts of Iron IV/common/operation_tokens/*. | 1.9 |
| remove_operation_token | tag = Towards which country to remove a token from. token = Which token to remove. | remove_operation_token = {     tag = GER     token = token_test } | Removes an operation token from the country. | Operation tokens are defined in /Hearts of Iron IV/common/operation_tokens/*. | 1.9 |
| capture_operative | operative = Which operative to capture. ignore_death_chance = Whether to ignore the death chance on capture (no by default). | capture_operative = {     operative = PREV     ignore_death_chance = yes } capture_operative = PREV | Captures the specified operative. | Operatives can be referred to by using tags that refer to scopes | 1.9 |
| create_operative_leader | bypass_recruitment = Whether the operative is directly added to the list of available operatives or needs to be recruited. available_to_spy_master = Whether the operative can be recruited by the spy master. bypass_recruitment should be set to no. portrait_tag_override = If selecting a random portrait, create one that is from the specified country rather than the current country. name = The name of the operative. GFX = The graphical reference of the picture of the leader, defined as a sprite within any /Hearts of Iron IV/interface/*.gfx file. nationalities = {  }The nationalities of the operative. traits = {  }The traits the leader spawns with. gender = The gender of the operative. Defaults to random. | create_operative_leader = { 	name = "Jacques Duclos" 	GFX = GFX_portrait_jacques_duclos 	traits = { operative_infiltrator operative_natural_orator } 	bypass_recruitment = no 	available_to_spy_master = yes 	nationalities = { FRA POL } } | Creates an operative for the current scope with the specified attributes. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt. All arguments aside from bypass_recruitment are optional. Must use a spriteType for the portrait, a direct link as in "gfx/leaders/TAG/filename.dds" will not work. | 1.9 |
| free_operative | The operative to be freed. | free_operative = PREV | Frees the specifies operative. | Operatives can be referred to by using tags that refer to scopes | 1.9 |
| free_random_operative | captured_by = The country that captured the operative. all = Whether to free all operatives or not (Defaults to no). | free_random_operative = { 	captured_by = POL 	all = yes } | Frees one random captured operative or all of them. |  | 1.9 |
| kill_operative | operative = The operative that is killed. | kill_operative = {     operative = PREV } kill_operative = PREV | Kills the targeted operative. | Operatives can be referred to by using tags that refer to scopes | 1.9 |
| turn_operative | operative = The operative that is turned. | turn_operative = {     operative = PREV } turn_operative = PREV | Turns the targeted operative against their own country, transferring them to the current country. | Operatives can be referred to by using tags that refer to scopes. This counts as the operative dying and will trigger the corresponding On action. Logs an error if used against your own operative. | 1.9 |
| steal_random_tech_bonus | category = The category to steal from. See /Hearts of Iron IV/common/technology_tags/* for list. folder = naval_folderThe folder to steal from. See /Hearts of Iron IV/common/technology_tags/* for list. ahead_reduction = The reduction to the ahead of time penalty. bonus = The bonus to research speed. base_bonus = The backup bonus if no tech is available. instant = Whether to instantly give a tech instead of a bonus or not. No by default. dynamic = Changes between instant and non-instant based on type. No by default. name = The name of the bonus. target = The country to steal from. uses = How many times the bonus can be used. | steal_random_tech_bonus = {     category = air_equipment     folder = naval_folder     ahead_reduction = 0.8     bonus = 1.2     base_bonus = 1.1     dynamic = yes     name = LOC_KEY     target = POL     uses = 2 } | Steals a random tech bonus from the specified country. | If a country does not have a tech to be stolen, a random bonus will be applied by using base_bonus as a base. | 1.9 |

### Characters

These are the character-related effects in the country scope. For effects in character scope, see [§ Character scope](#Character scope).

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_nationality | target_country =  / The target country. character =  The character to transfer. | set_nationality = {     target_country = TZN     character = OMA_sultan } | Switches the specified character to the specified country. | If you wish to change the nationality of a specific character, and the country getting the effect doesn't have the character recruited already, use the every_possible_country = {     limit = { has_character = ID }     random_character = {         limit = { is_character = ID }         set_nationality = TAG     } } command to call them up. Only necessary in 1.11 and beyond. | 1.11 |
| retire_character |  | retire_character = GER_Character_Token | Retires the character, removing every role they hold and making them disappear from the game. | Country scope only. The character cannot be re-recruited after retiring. | 1.11 |
| set_character_name | character = The character to modify. name = The new name of the character. | set_character_name = { 	character = my_character 	name = my_name } | Sets the new name for the target character. | Can also be used in character scope. | 1.11 |
| character_list_tooltip | limit = {  }Triggers that must be fulfilled to show up in the list. random_select_amount = Upper bound on the characters that may be shown. | character_list_tooltip = { 	limit = {         has_character_flag = SOV_targeted_for_purge_flag     }     random_select_amount = 4 } | Displays a list of every character meeting the specified limitation and recruited by the current country. |  | 1.11 |
| add_trait | character = The character to modify. slot =  Slot of the character. Necessary for advisors. ideology =  Ideology type of the character. Necessary for country leaders. trait = The trait to add. | add_trait = {      character = TAG_jane_smith      slot = political_advisor      trait = really_good_boss } add_trait = {      character = TAG_my_leader      ideology = liberalism      trait = field_of_gar } | Adds the specified country leader trait to the character. | Can also be used in character scope. Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. The character slot can be the character's name or id. Using name is recommended because 1.11 made id obsolete. | 1.11 |
| remove_trait | character = The character to modify. slot =  Slot of the character. Necessary for advisors. ideology =  Ideology type of the character. Necessary for country leaders. trait = The trait to remove. | remove_trait = {     character = TAG_jane_smith     slot = political_advisor     trait = really_good_boss } remove_trait = {      character = TAG_my_leader      ideology = liberalism      trait = field_of_gar } | Removes the specified trait from the character. | Can also be used in character scope. Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. The character slot can be the character's name or id. Using name is recommended because 1.11 made id obsolete. | 1.11 |

#### Unit leaders

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| create_corps_commander | name = The name of the leader. picture = OR portrait_path = OR gfx = The graphical reference of the picture of the leader. skill = The skill of the leader. attack_skill = The attack skill of the leader. defense_skill = The defense skill of the leader. planning_skill = The planning skill of the leader. logistics_skill = The logistics skill of the leader. traits = {  }The traits the leader spawns with. female = The gender of the leader. legacy_id = The legacy ID used for the unit leader. Optional. | create_corps_commander = { 	name = "Jean de Lattre de Tassigny" 	picture = "Portrait_France_Jean_de_Lattre_de_Tassigny.dds" 	traits = { trickster brilliant_strategist } 	skill = 4 	attack_skill = 4 	defense_skill = 2 	planning_skill = 4 	logistics_skill = 3 } | Creates a commander for the current scope with the specified attributes. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt. Deprecated, recommended to use add_corps_commander_role instead when possible. The created corps commander will not be able to have a portrait if assigned to be a minister via officer corps, causing errors. | 1.0 |
| create_field_marshal | name = The name of the leader. picture = OR portrait_path = OR gfx = The graphical reference of the picture of the leader. skill = The skill of the leader. attack_skill = The attack skill of the leader. defense_skill = The defense skill of the leader. planning_skill = The planning skill of the leader. logistics_skill = The logistics skill of the leader. traits = {  }The traits the leader spawns with. female = The gender of the leader. legacy_id = The legacy ID used for the unit leader. Optional. | create_field_marshal = { 	name = "Maurice Gamelin" 	portrait_path = "GFX_portrait_FRA_maurice_gamelin" 	traits = { defensive_doctrine } 	skill = 2 	attack_skill = 1 	defense_skill = 3 	planning_skill = 2 	logistics_skill = 1 } | Creates a field marshal for the current scope with the specified attributes. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt. Deprecated, recommended to use add_field_marshal_role instead when possible. The created field marshal will not be able to have a portrait if assigned to be a minister via officer corps, causing errors. | 1.0 |
| create_navy_leader | name = The name of the leader. picture = OR portrait_path = OR gfx = The graphical reference of the picture of the leader. skill = The skill of the leader. attack_skill = The attack skill of the leader. defense_skill = The defense skill of the leader. maneuvering_skill = The maneuvering skill of the leader. coordination_skill = The coordination skill of the leader. traits = {  }The traits the leader spawns with. female = The gender of the leader. legacy_id = The legacy ID used for the unit leader. Optional. | create_navy_leader = { 	name = "François Darlan" 	gfx = "GFX_portrait_FRA_francois_darlan" 	traits = { superior_tactician } 	skill = 3 	attack_skill = 2 	defense_skill = 4 	maneuvering_skill = 3 	coordination_skill = 2 } | Creates a naval leader for the current scope with the specified attributes. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt. Deprecated, recommended to use add_naval_commander_role instead when possible. The created admiral will not be able to have a portrait if assigned to be a minister via officer corps, causing errors. | 1.0 |
| remove_unit_leader | The id of the unit leader. | remove_unit_leader = 70 | Removes the specified unit leader by their legacy ID. | Does not work with the character ID. Instead, remove_unit_leader_role within the scope of the character is recommended when possible. | 1.0 |
| add_corps_commander_role | character = The character to modify. <...>Army leader role definition | add_corps_commander_role = {     Character = GER_Character_token     skill = 4     attack_skill = 2     defense_skill = 3     planning_skill = 3     logistics_skill = 5 } | Sets the specified character to also act as a corps commander. | Can also be used in character scope. | 1.11 |
| add_field_marshal_role | character = The character to modify. <...>Army leader role definition | add_field_marshal_role = {   character = GER_Character_token   skill = 4   attack_skill = 2   defense_skill = 3   planning_skill = 3   logistics_skill = 5 } | Sets the specified character to also act as a field marshal. | Can also be used in character scope. | 1.11 |
| add_naval_commander_role | character = The character to modify. <...>Navy leader role definition | add_naval_commander_role = {   Character = GER_Character_token   skill = 4   attack_skill = 2   defense_skill = 3   planning_skill = 3   logistics_skill = 5 } | Sets the specified character to also act as an admiral. | Can also be used in character scope. | 1.11 |
| show_unit_leaders_tooltip | The character whose name is to be shown. | show_unit_leaders_tooltip = TAG_my_leader | Shows the name of the specified character as a tooltip. |  | 1.11 |

#### Country leaders

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| create_country_leader | name = The name of the leader. desc = The description of the leader.picture = The graphical reference to the leader portrait.expire = When the leader dies in history.ideology = The sub-ideology of the country leader. Does not accept regular ideologies. female = The gender of the leader.Traits scopeThe trait to add. Can add multiple. | create_country_leader = { 	name = AFG_mohammed_zahir_shah 	desc = "POLITICS_MOHAMMED_ZAHIR_SHAH_DESC" 	picture = GFX_AFG_mohammed_zahir_shah 	expire = "1965.1.1" 	ideology = despotism 	traits = { 	} } |  | The portrait uses a spriteType, defined within /Hearts of Iron IV/interface/*.gfx. Sub-ideologies are defined in /Hearts of Iron IV/common/ideologies. Deprecated. Recommended to use add_country_leader_role instead when possible. | 1.0 |
| add_country_leader_role | character = The character to modify. country_leader = { ... }Country leader role definition promote_leader = Will promote the leader to be the leader of the assigned party. Optional, defaults to false. | add_country_leader_role = {     character = GER_character_token     promote_leader = yes     country_leader = {         ideology = fascism_ideology         expire = "1965.1.1.1"         traits = { war_industrialist }     } } | Sets the specified character to also act as a country leader, promoting to the party leader if specified. | Can also be used in character scope. Does absolutely nothing if the character already has a country leader role in the ideology group. | 1.11 |
| promote_character | The character to promote. OR character = The character to promote. ideology = The ideology type used by the country leader role. | promote_character = GER_erwin_rommel promote_character = {     character = GER_erwin_rommel     ideology = nazism } | Promotes a character to the leader of their political party. | Can also be used in character scope. If the character has multiple country leader roles, specifying the ideology type is mandatory. Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. | 1.11 |
| remove_country_leader_role | character = The character to modify. ideology = The ideology type of the character. | remove_country_leader_role = {     character = GER_Character_Token     ideology = socialism } | Removes a country leader role from a character. | Can also be used in character scope. Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. | 1.11 |
| kill_ideology_leader | Ideology. | kill_ideology_leader = communism | Kills the country leader of the designated ideology for the current scope. |  | 1.9 |
| retire_ideology_leader | Ideology. | retire_ideology_leader = fascism | Retires and removes the country leader of the ideology party for the current scope. |  | 1.9 |
| kill_country_leader | Boolean. | kill_country_leader = yes | Kills the country leader for the current scope. |  | 1.0 |
| retire_country_leader | Boolean. | retire_country_leader = yes | Retires and removes the country leader as head of their party for the current scope. |  | 1.0 |
| set_country_leader_ideology | The government to set. | set_country_leader_ideology = socialism | Changes the country leader's government type for the current scope. | Creates no tooltip. | 1.0 |
| set_country_leader_description | ideology = The ideology of the country leader, optional. desc = The new description. | set_country_leader_description = { 	ideology = neutrality 	desc = LOC_KEY } | Changes the country leader's description. | Must use a localisation key from any /Hearts of Iron IV/localisation/*.yml file, putting the description in quotes will not work. Localisation for more info | 1.9.1 |
| set_country_leader_name | ideology = The ideology of the country leader, optional. name = The new name. | set_country_leader_name = { 	ideology = neutrality 	name = LOC_KEY } | Changes the country leader's name. |  | 1.9.1 |
| set_country_leader_portrait | ideology = The ideology of the country leader, optional. portrait = The new portrait. | set_country_leader_portrait = { 	ideology = neutrality 	portrait = GFX_IMAGE_NAME } | Changes the country leader's portrait. | The portrait must be defined in /Hearts of Iron IV/interface/*.gfx | 1.9.1 |
| add_country_leader_trait | The trait to add. | add_country_leader_trait = nationalist_symbol | Adds the specified trait to the current country's country leader. | Traits are found in /Hearts of Iron IV/common/country_leader/*.txt files. | 1.0 |
| remove_country_leader_trait | The trait to remove. | remove_country_leader_trait = nationalist_symbol | Removes the specified trait from the current scope's country leader. | Traits are found in /Hearts of Iron IV/common/country_leader/*.txt files. | 1.0 |
| swap_ruler_traits | Similar to swap_ideas. Removes one trait and adds another. | swap_ruler_traits = { remove =  add =  } | Swaps traits. | Use swap_country_leader_traits in character scope. | 1.6 |

#### Advisors

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| activate_advisor | The character to activate. | activate_advisor = GER_character_token_air_chief | Hires an advisor, placing them into their respective slot. |  | 1.11 |
| deactivate_advisor | The character to deactivate. | deactivate_advisor = GER_character_token_air_chief | Dismisses an advisor from their respective slot, leaving it empty. |  | 1.11 |
| add_advisor_role | character = The character to modify. advisor = { ... }Advisor role definition activate = Will activate the advisor (add them directly when the command is run to the countries government). Optional, defaults to false. | add_advisor_role = {     character = GER_Character_token     activate = yes     advisor = {         slot = air_chief         cost = 50         idea_token = GER_character_token_air_chief         traits = {             air_chief_ground_support_2         }     } } | Sets the specified character to also act as an advisor, activating if specified. | Can also be used in character scope. Trigger and effect blocks (such as allowed and on_add) cannot be added within advisor definitions created this way. | 1.11 |
| remove_advisor_role | character = Specifies the character if the effect is executed in country scope. slot = The slot where to remove the advisor slot from. | remove_advisor_role = {   character = "SOV_genrikh_yagoda"   slot = political_advisor } | Removes the specified advisor role from the character. | Can also be used in character scope. | 1.11 |
| set_can_be_fired_in_advisor_role | character = The character to modify. slot = The slot of the character to modify. value = The value to set. | set_can_be_fired_in_advisor_role = {     character = BHR_important_advisor     value = no } | Changes the can_be_fired attribute of the advisor, preventing the player from dismissing the advisor. | Can also be used in character scope. | 1.12.8 |

#### Scientists

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_scientist_role | character =  / The character to modify. <...>Scientist role definition | add_scientist_role = {   character = my_character / var:my_char_var / PREV   scientist = {     desc = desc_loc_key     traits = { scientist_trait_token ... }     skills = { specialization_token = 2 ... }   } } | Adds the scientist role to a character. | The scientist role format is the same as in the character DB. Except the visible trigger, a scientist role created via effect cannot have triggers. Can also be used in character scope. | 1.15 |
| remove_scientist_role | character =  / | remove_scientist_role = {   character = my_character / var:my_char_var / PREV } | Remove the scientist role from a character. | Can also be used in character scope. | 1.15 |
| generate_scientist_character | portrait = Optional, random portrait by default. portrait_tag_override =  / Optional, accepts variable and keyword, only relevant if using random portrait, by default use country in scope. gender = Optional, by default random gender. skills = { ??? }Optional array, same format as in scientist role in character DB, by default all skills are at 1. traits = {  }Optional array. | generate_scientist_character = {   portrait = GFX_portrait   portrait_tag_override = CHI   gender = male   skills = {     specialization_token = 2   }   traits = { trait_token } } | Generate a new character with a scientist role and recruit it in the country in scope. |  | 1.15 |

### MIOs

These are the MIO-related effects in the country scope. For effects in [military industrial organisation](/Military industrial organisation) scope, see [§ MIO scope](#MIO scope).

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| show_mio_tooltip | MIO to display. | show_mio_tooltip = my_mio | Displays a tooltip that shows the name of the MIO and its initial trait (if present). | Doesn't change the availability of the MIO directly. | 1.13 |
| unlock_military_industrial_organization_tooltip | / MIO to unlock. | unlock_military_industrial_organization_tooltip = mio:my_mio_token unlock_military_industrial_organization_tooltip = var:my_mio_var | Display a tooltip saying the MIO is made available (aka unlocked). |  | 1.13 |
| unlock_mio_policy_tooltip | Policy to display. OR policy = Policy to display. show_modifiers = Whether the trait's modifiers should be shown in the tooltip. Defaults to true. | unlock_mio_policy_tooltip = my_policy_1 unlock_mio_policy_tooltip = {     policy = my_policy_2     show_modifiers = no } | Displays a tooltip that says that the policy is made available. | Doesn't change the availability of the policy directly. | 1.13 |
| add_mio_policy_cost | policy = Policy to modify. value = Amount in political power to add. | add_mio_policy_cost = {     policy = my_policy     value = 10 } | Modifies the base cost of a MIO policy. | The base amount is capped at 0 from below. | 1.13 |
| set_mio_policy_cost | policy = Policy to modify. value = Amount in political power to set. | set_mio_policy_cost = {     policy = my_policy     value = 100 } | Modifies the base cost of a MIO policy. | Cannot be negative. | 1.13 |
| add_mio_policy_cooldown | policy = Policy to modify. value = Amount in days to add. | add_mio_policy_cooldown = {     policy = my_policy     value = 10 } | Modifies the base length of a MIO policy cooldown. | The base amount is capped at 0 from below. | 1.13 |
| set_mio_policy_cooldown | policy = Policy to modify. value = Amount in days to set. | set_mio_policy_cooldown  = {     policy = my_policy     value = 100 } | Modifies the base length of a MIO policy cooldown. | Cannot be negative. | 1.13 |

### Special Projects

These are special project related effects in the country scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| complete_special_project | sp:Project to complete. OR project = sp:Project to complete. scientist = Optional, default to current scientists on the project. state = Optional, default to current state of the project. iteration_output = {  }Optional, can be a single reward or reward = option. show_modifiers = Optional, default = yes. | complete_special_project = sp:sp_naval_midget_submarine complete_special_project = {   project = sp:sp_naval_midget_submarine   scientist = ITA_curio_bernardis   state = my_state   iteration_output = {     my_reward     my_other_reward     my_third_reward = my_option_1   }   show_modifiers = no } | Complete a special project for the country in scope. This effect will not take into account the current state of the project tree and will allow you to unlock a project even if the one before is not unlocked. Since the project is not completed within a facility, the facility state and scientist effects are NOT applied. | project, scientist, state accepts variables and keywords. | 1.15 |
| add_breakthrough_points | specialization = The specialization e.g. specialization_land. value = The amount of specialization breakthrough points to add. | add_breakthrough_points = {   specialization = specialization_land   value = 3 } add_breakthrough_points = {   specialization = all   value = 1 } | Add breakthrough points to one specialization or all for a country scope. |  | 1.15 |
| add_breakthrough_progress | specialization = The specialization e.g. specialization_land. value = The amount of specialization breakthrough progress to be added. | add_breakthrough_progress = {   specialization = specialization_land   value = 3 } add_breakthrough_progress = {   specialization = all   value = sp_breakthrough_progress.medium } | Add breakthrough progress to one specialization or all for a country scope. | The value can either be an absolute value or a script constant. | 1.15 |

### Career profile

These are career profile related effects in the country scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| career_profile_step_missiolini | Boolean. | career_profile_step_missiolini = yes | Step completed Mussolini missions by one for the career profile. |  | ??? |

### History

These effects can **only be used within history files**, failing when used outside. However, they're considered effects anyway rather than history arguments, as they can be used in if statements.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| recruit_character |  | recruit_character = GER_Character_token | Initially assigns the specified character to the current country. |  | 1.11 |
| generate_character | token_base = Mandatory, acts as the character token. name = The name used for the character. Generates a random name if not set. | generate_character = {     token_base = army_chief_defensive_1     name = funny_name     advisor = {         slot = air_chief         cost = 50         idea_token = GER_character_token_air_chief         traits = {             air_chief_ground_support_2         }         allowed = {             always = yes         }     } } | Generates a character for current country. | If used to create an advisor, the idea token of the advisor role will be the token_base and idea_token (defaulting to the slot if the idea token is not set) concatenated, with an underscore as a separator. In the provided example, the idea token will be army_chief_defensive_1_GER_character_token_air_chief; if idea_token wasn't present, it'd be army_chief_defensive_1_air_chief. | 1.11 |
| set_oob | The name of the file used for the order of battle without the .txt extension. | set_oob = BHR_1936 | Sets the order of battle to be used for the current country's divisions, overriding every other non-naval and non-air order of battle. | Orders of battle are defined in /Hearts of Iron IV/history/units/*.txt files. | 1.0 |
| set_naval_oob | The name of the file used for the order of battle without the .txt extension. | set_naval_oob = BHR_1936_naval_legacy | Sets the order of battle to be used for the current country's divisions, overriding every other naval order of battle. | Orders of battle are defined in /Hearts of Iron IV/history/units/*.txt files. | 1.0 |
| set_air_oob | The name of the file used for the order of battle without the .txt extension. | set_air_oob = ITA_1936_air_bba | Sets the order of battle to be used for the current country's divisions, overriding every other air order of battle. | Orders of battle are defined in /Hearts of Iron IV/history/units/*.txt files. | 1.12 |
| set_keyed_oob | key = The key used for the file. name = The name of the file used for the order of battle without the .txt extension. | set_keyed_oob = {     key = naval     name = BHR_1936_mtg } | Sets the order of battle to be used for the current country's divisions, overriding every other keyed order of battle that uses the same key. | Orders of battle are defined in /Hearts of Iron IV/history/units/*.txt files. | 1.0 |

### Variable

These are variable related effects in the country scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| get_highest_scored_country_temp | scorer = <???>Id that is used in country scorer. varVariable name that the result will be stored. (default is highest_scored_country) | get_highest_scored_country_temp = {   scorer = scorer_id   var = var_name } | Calculates the highest scored country that is defined in a country scorer and sets it to a variable. |  | ??? |
| get_sorted_scored_countries_temp | scorer = <???>Id that is used in country scorer. array = A name to store sorted countries as a temp array (default to sorted_country_list) scores = Corresponding score temp array for countries stored in array (default to country_list_scores) | get_sorted_scored_countries_temp = {   scorer = scorer_id   array = array_name   scores = array_name } | Calculates & sorts all countries in a country scorer and stores them and their scores in temp arrays. |  | ??? |
| get_supply_vehicles | var = Variable name to set. type = Can be truck or train. need = Default no. If yes, gets the number of needed vehicles. | get_supply_vehicles = {   var = trucks_needed   type = truck   need = yes } | Sets a variable to the number of supply vehicles in stockpile or that are needed. |  | ??? |
| get_supply_vehicles_temp | var = Variable name to set. type = Can be truck or train. need = Default no. If yes, gets the number of needed vehicles. | get_supply_vehicles_temp = {   var = trucks_needed   type = truck   need = yes } | Sets a temp variable to the number of supply vehicles in stockpile or that are needed. |  | ??? |

## State scope

The effects here must be used within a **state** scope.

### General

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| state_event | id = The event to fire. days =  / Fires the event in the specified number of days. Optional.hours =  / Fires the event in the specified number of hours. Optional.random =  / Adds a random number (between 0 and random, inclusive) of hours to the scheduled fire time. Optional.random_days =  / Adds a random number (between 0 and random_days, inclusive) of days to the scheduled fire time. Optional. | state_event = {     id = my_event.1     days = 10     random = 50     random_days = 10     trigger_for = controller } | Fires the specified event for the current state. | Where triggers do not need to be repeatedly checked random can be a performance light alternative to mean_time_to_happen for scheduling events. Using days =  /  or hours may still be bugged and will not fire the event. | 1.0 |
| set_state_flag | An unique string to identify the state flag with. OR flag = The flag to set. days = Sets the flag to last for the specified amount of days. Optional. value = The new value of the flag on the scale from -2 147 483 648 to 2 147 483 647. | set_state_flag = my_flag set_state_flag = {     flag = my_flag     days = 123     value = 1 } | Defines a state flag. | No tooltip is shown. The flag in this effect is used in the meaning of 'boolean flag', used to store information. | 1.0 |
| clr_state_flag | The unique string of a state flag to clear. | clr_state_flag = my_flag | Clears a defined state flag. | No tooltip is shown. | 1.0 |
| modify_state_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_state_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. | 1.3 |
| set_state_name | Defines the new name. | set_state_name = "Funland" | Changes the current state's name to the specified name. |  | 1.3 |
| reset_state_name | Boolean. | reset_state_name = yes | Resets any changes to the current state's name. |  | 1.3 |
| add_claim_by | / The country to add the claim for. | add_claim_by = SOV | Adds a claim for the specified country on the current scope. |  | 1.0 |
| remove_claim_by | / The country to remove the claim for. | remove_claim_by = SOV | Removes a claim by the specified country on the current scope. |  | 1.0 |
| add_core_of | / The country to add the core for. | add_core_of = SOV | Adds a core for the specified country on the current scope. |  | 1.0 |
| remove_core_of | / The country to remove the core for. | remove_core_of = SOV | Removes a core for the specified country on the current scope. |  | 1.0 |
| set_demilitarized_zone | Boolean. | set_demilitarized_zone = yes | Makes the current scope a demilitarized zone. |  | 1.0 |
| set_state_category | The category to change to. | set_state_category = rural | Changes the current state category to the specified category. | Categories are found in /Hearts of Iron IV/common/state_category/*.txt | 1.3 |
| add_state_modifier | Modifier scope  = Adds a modifier to the state. | add_state_modifier = {     modifier = {         local_resources = 2.0     } } | Adds a modifier to the current state. |  | 1.3 |
| add_manpower | / The amount to add. | add_manpower = 10000 | Adds the specified amount of total population to the current state. | Note that when using negative manpower it will, besides reducing the population, also add directly to the recruitable manpower of the state. Which will increase your manpower | 1.0 |
| add_resource | type = The resource to add. amount =  / The amount to add. | add_resource = {     type = oil     amount = 100 } | Adds the specified resource in the specified amount to the current state. | Can also be used in country scope. | 1.0 |
| set_border_war | Boolean. | set_border_war = yes | Enables Border War status for the current state. | Used for the state-based border wars, represented with orange stripes, see § Border wars for the border wars that simulate combat on a border between two countries. On the end of the border war, the on_border_war_lost on action is fired for the state that where the border war was lost. | 1.0 |
| create_unit | division = The division string. owner = The owner of the division. prioritize_location = If possible, this province within the state gets used. Optional. allow_spawning_on_enemy_provs = yesAllows the units to be created on provinces owned by the division owner's enemy. Defaults to false. count = The amount of units to create. Defaults to 1. id = The ID to identify the unit. Only used in delete_unit. country_score = { ... }A MTTH block deciding the province in the state where the division should spawn, evaluates in the scope of the controller. Defaults to prioritising owner's controlled provinces first and then owner's allies. divisional_commander_xp = give the division commander experience on unit creation The following arguments go within division = "":  name = \"\"The name of the division. division_template = \"\"The template to be used by the division. start_experience_factor = Experience of the division, with 0 being none and 1 being full training. Defaults to 1. start_equipment_factor = Equipment stockpile of the division. Defaults to 1. start_manpower_factor = Manpower stockpile of the division. Defaults to 1. force_equipment_variants = {  = { owner = \"\" amount =  version_name = \"\" } }Forces a certain type of equipment to be used. Multiple equipment types can be added by adding multiple  = {} lines. | create_unit = {     division = "name = \"Infantry Division\" division_template = \"Infantry Division\" start_experience_factor = 0.5"     owner = GER } create_unit = {     division = "name = \"Artie\" division_template = \"Artillery Division\" start_manpower_factor = 0.3"     owner = BHR     count = 3     allow_spawning_on_enemy_provs = yes     country_score = {         base = 3         modifier = {             factor = 2             tag = OMA         }     }     id = 123 } create_unit = {    division = "name = \"Tank division\" division_template = \"Tank Division\" start_manpower_factor = 1 force_equipment_variants = { medium_tank_chassis_2 = { owner = \"USA\" amount = 100 version_name = \"M4 Sherman\" }}"    owner = USA    count = 1 } | Adds the specified division to the current state. | The division string must be on one line. A linebreak in the middle of division = "..." will break the effect and result in no units being spawned. Can only be used within a state scope, such as capital_scope. The effect will do nothing when put into a country's scope.  Equipment factor cannot be set to zero. If set to zero, it will be treated as a 1. Created equipment will be the latest available to the country. | 1.3 |
| teleport_armies | limit = {  }The condition that must be true for the owner of the armies for them to teleport. to_state_array = The state array the armies will get teleported to.to_province = The province the armies will get teleported to.to_state = The state the armies will get teleported to. | teleport_armies = {     limit = {         has_war_together_with = ROOT     }     to_state_array = owned_controlled_states } | Teleports all armies in the specified state if the owner of the armies meets the condition. | Only define one of to_state_array, to_state, or to_province. If none is specified, it defaults to the capital. | 1.9 |
| add_province_modifier | static_modifiers = {  }The list of modifiers.province = The province to apply the modifiers to.provinces = {}Scope for selecting multiple provinces. The following arguments have to go inside it:id = The ID of the province. Multiple can be specified.all_provinces = yesSelects all provinces to which the limitations apply. The following arguments require it: limit_to_coastal = yes Limits the selection of provinces to only coastal ones.limit_to_border = yes Limits the selection of provinces to only ones bordering a different country.limit_to_naval_base = yes Limits the selection of provinces to only ones that have a naval base.limit_to_victory_point = yes Limits the selection of provinces to only ones that have a victory point, or a city, in them.days =  Will be temporary if specified, can be variable | add_province_modifier = { 	static_modifiers = { mod_modifier_1 mod_modifier_2 } 	province = 1234 }add_province_modifier = { 	static_modifiers = { mod_modifier_1 mod_modifier_2 } 	province = { 		id = 1234 		id = 4321         days = 7  	}  }add_province_modifier = { 	static_modifiers = { mod_modifier_1 mod_modifier_2 } 	province = { 		all_provinces = yes 		limit_to_coastal = yes 		limit_to_border = yes 		limit_to_naval_base = yes 		limit_to_victory_point = yes 	}  } | Adds a province modifier to the specified provinces in this state. | Province modifiers are defined in /Hearts of Iron IV/common/modifiers/*.txt | 1.6 |
| remove_province_modifier | static_modifiers = {  }The list of modifiers.province = The province to apply the modifiers to.provinces = {}Scope for selecting multiple provinces. The following arguments have to go inside it:id = The ID of the province. Multiple can be specified.all_provinces = yesSelects all provinces to which the limitations apply. The following arguments require it: limit_to_coastal = yes Limits the selection of provinces to only coastal ones.limit_to_border = yes Limits the selection of provinces to only ones bordering a different country.limit_to_naval_base = yes Limits the selection of provinces to only ones that have a naval base.limit_to_victory_point = yes Limits the selection of provinces to only ones that have a victory point, or a city, in them. | remove_province_modifier = { 	static_modifiers = { mod_modifier_1 mod_modifier_2 } 	province = 1234 }remove_province_modifier = { 	static_modifiers = { mod_modifier_1 mod_modifier_2 } 	province = { 		id = 1234 		id = 4321 	}  }remove_province_modifier = { 	static_modifiers = { mod_modifier_1 mod_modifier_2 } 	province = { 		all_provinces = yes 		limit_to_coastal = yes 		limit_to_border = yes 		limit_to_naval_base = yes 		limit_to_victory_point = yes 	}  } | Removes a province modifier to the specified provinces in this state. | Province modifiers are defined in /Hearts of Iron IV/common/modifiers/*.txt | 1.6 |
| add_victory_points | Add victory points to a province | add_victory_points = { 	province = 1234 	value = 10 } | Adds victory points to a province. | Accepts negative values | 1.10 |
| set_victory_points | Set the victory points of a province | set_victory_points = { 	province = 1234 	value = 10 } | Sets the number of victory point in a province. | Accepts negative values | 1.10 |
| set_state_province_controller | controller = The new controller of the province. limit = {  }The triggers that must be fulfilled by the province's current controller to be transferred to the new controller. | set_state_province_controller = {     controller = POL     limit = {         OR = {             tag = GER             is_in_faction_with = GER         }     } } | Changes the controller of all provinces within that state controlled by countries that meet triggers to the specified country. |  | 1.9 |
| transfer_state_to | Country to transfer the state to. | transfer_state_to = JAM | Sets owner and controller of the state to the given country |  | 1.11 |
| set_state_owner_to | Country to set the owner (but not the controller) of the state to. | set_state_owner_to = JAM | Sets the owner of the state to the given country | Use transfer_state_to unless the control specifically shouldn't be given. | 1.11 |
| set_state_controller_to | Country to set the controller (but not the owner) of the state to. | set_state_controller_to = ITA | Sets the controller of the state to the given country |  | 1.11 |
| add_contested_owner | / Country to add contest to state. | add_contested_owner = GER | Adds a contested owner to a state. The effect can be used either from a country or a state scope and accepts the other as parameter. | Can also be used in country scope. | 1.15 |
| remove_contested_owner | / Country to remove contest to state. | remove_contested_owner = GER | Removes a contested owner to a state. The effect can be used either from a country or a state scope and accepts the other as parameter. | Can also be used in country scope. | 1.15 |
| strategic_province_location | = | strategic_province_location = {     defensible_coastline = 10124 } | Add a strategic location to a province using state scope. The available strategic locations are defined in strategic_locations and are specified with a province id. | Can contain multiple strategic locations. | 1.17 |
| strategic_state_location | = | strategic_state_location = {     favorable_approach = 11932 } | Add strategic locations to a state in scope. The available strategic locations are defined in strategic_locations. | Can contain multiple strategic locations. | 1.17 |

### Buildings

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_extra_state_shared_building_slots | / The amount of slots to add or remove. | add_extra_state_shared_building_slots = 2 | Changes the number of shared building slots for the current state. | Shared buildings slots being the ones used for multiple building types, such as military or civilian factories. This is in contrast to non-shared slots, such as those used by radio stations or air bases, which only can be changed globally with technologies. Note: When using a variable and a saved event target, must be used as "saved_event.var_name" because "event_target:saved_event.var_name" will not work. | 1.0 |
| add_building_construction | type = The building to add. level =  / The level to set the building to. instant_build = Defines whether the buildings are instantly built. province = Defines the exact province to add provincal buildings in. Can be used as a scope. Province scope all_provinces = Affect all provinces within the state that meet each limit. Used in the province scope. id = Affect the specified province ID. Used in the province scope, will apply for each province if inserted multiple times. limit_to_coastal = Affect only coastal provinces within the selection. Used in the province scope. limit_to_naval_base = Affect only provinces that have naval bases built. Used in the province scope. limit_to_border = Affect only provinces that lie on a border between countries. Used in the province scope. limit_to_border_country = Affect only provinces that border a specific other country. Used in the province scope. limit_to_victory_point = /Affect only provinces that meet the victory point amount prerequisite. If yes is used in place of a number, any amount of victory points works. Used in the province scope. limit_to_supply_node = Affect only provinces that have a supply node. Used in the province scope. level = Affect only provinces with buildings level below, at or above the specified level. Used in the province scope. | add_building_construction = {     type = arms_factory     level = 5     instant_build = yes }  add_building_construction = {     type = bunker     level = 10     instant_build = yes     province = {         all_provinces = yes         limit_to_border = yes         limit_to_victory_point > 1     } }  add_building_construction = {     type = bunker     level = 1     instant_build = yes     province = 2999 } | Starts construction in the current state for the specified building. | For provincial buildings, must be done within the scope of the state that contains the province even if done on a specific province. If the controller country doesn't have an order of battle assigned within the history file, the buildings will not show up within the production menu until a recalculation of buildings, such as by changing consumer goods or reloading a savefile. Can only be used within a state scope, such as random_owned_controlled_state. The effect will do nothing when put into a country's scope.  For the list of building IDs present in the base game, see Building modding#Types. | 1.0 |
| set_building_level | type = The building to add. level =  / The level to set the building to. instant_build = Defines whether the buildings are instantly built. province = Defines the exact province to add provincal buildings in. Can be used as a scope. Province scope all_provinces = Affect all provinces within the state that meet each limit. Used in the province scope. id = Affect the specified province ID. Used in the province scope, will apply for each province if inserted multiple times. limit_to_coastal = Affect only coastal provinces within the selection. Used in the province scope. limit_to_naval_base = Affect only provinces that have naval bases built. Used in the province scope. limit_to_border = Affect only provinces that lie on a border between countries. Used in the province scope. limit_to_border_country = Affect only provinces that border a specific other country. Used in the province scope. limit_to_victory_point = /Affect only provinces that meet the victory point amount prerequisite. If yes is used in place of a number, any amount of victory points works. Used in the province scope. limit_to_supply_node = Affect only provinces that have a supply node. Used in the province scope. level = Affect only provinces with buildings level below, at or above the specified level. Used in the province scope. | set_building_level = {     type = infrastructure     level = 10     instant_build = yes }  set_building_level = {     type = bunker     level = 3     province = {         all_provinces = yes         limit_to_border = yes         level < 3     } } | Sets the specified building to the current state (or provinces within the state). | The province scope is used for provincal level buildings.  You can limit the construction to victory points using : limit_to_victory_point > 5 (only build province buildings on province with VP over 5 ) limit_to_victory_point = yes (only build province buildings on province with VP) For provincial buildings, must be done within the scope of the state that contains the province even if done on a specific province. | 1.4 |
| damage_building | type = The building to damage. tags = The buildings with this tag to damage. tags = {  }The buildings with these tags to damage. repair_speed_modifier =  Repair will be x% slower until building is fully repaired damage = The amount of damage to inflict. province =  / The province to target for provincal buildings. | damage_building = {   type = infrastructure   damage = 1 } damage_building = {   tags = dam_building   damage = 1   repair_speed_modifier = -0.8   province = 3488 } | Damages a building in a targeted state or province. | The health of buildings is determined by the value attribute in a building's definition. This is multiplied by their level to get their total health. Can also be used in country scope. | 1.3 |
| remove_building | type = The building to remove. tag = The buildings with this tag to remove. tag = {  }The buildings with these tags to remove. level =  / The levels to remove. | remove_building = {     type = arms_factory     level = 5 } remove_building = {     tag = facility     level = 1 } | Removes the specified building in the current state. For shared buildings level determines the amount, whereas for the others it is the actual level. |  | 1.0 |
| construct_building_in_random_province | = Building to build. | 65 = {     construct_building_in_random_province = {         land_facility = 1     } } | Set building level in a random province of state scope. |  | 1.15 |

### Resistance and compliance

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_compliance | / The amount to add. | add_compliance = 30 | Adds compliance to the specified state. |  | 1.9 |
| add_resistance | / The amount to add. | add_resistance = 30 | Adds resistance to the specified state. |  | 1.9 |
| add_resistance_target | / The amount to add. | add_resistance_target = 30 | Increases resistance target in the specified state. |  | 1.9 |
| add_resistance_target | id = The ID of the target increase.amount = /The amount to increase the resistance target by.occupied = Will only apply the increase if the the occupied country is the specified scope.occupier = Will only apply the increase if the the occupier is the specified scope.days = /If set, the resistance target will only be increased for the specified amount of days.tooltip = The tooltip to show in the resistance target tooltip. | add_resistance_target = {     id = 123     amount = 30     occupied = ENG     occupier = GER     days = 365     tooltip = my_localisation_key } | Increases resistance target in the specified state. |  | 1.9 |
| cancel_resistance | Boolean. | cancel_resistance = yes | Cancels resistance activity for the current state. |  | 1.9 |
| force_disable_resistance | The target country. | force_disable_resistance = GER | Disables resistance for the scoped state when the occupier is the specified country. |  | 1.9 |
| force_disable_resistance | clear = If set to yes, will clear resistance.occupier = Resistance will be disabled if the occupier is the specified scope.occupied = Resistance will be disabled if the occupied country is the specified scope. | force_disable_resistance = {     clear = yes     occupier = GER     occupied = ENG } | Disables resistance for the scoped state when the occupier is the specified country. |  | 1.9 |
| force_enable_resistance | The target country. | force_enable_resistance = GER | Enables resistance for the scoped state when the occupier is the specified country. | Does not start resistance by itself, only removes the checks forcefully disabling it. Use with start_resistance in order to immediately start resistance. | 1.9 |
| force_enable_resistance | clear = If set to yes, will clear resistance.occupier = Resistance will be enabled if the occupier is the specified scope.occupied = Resistance will be enabled if the occupied country is the specified scope. | force_enable_resistance = {     clear = yes     occupier = GER     occupied = ENG } | Enables resistance for the scoped state when the occupier is the specified country. | Does not start resistance by itself, only removes the checks forcefully disabling it. Use with start_resistance in order to immediately start resistance. | 1.9 |
| remove_resistance_target | / The id of the resistance target to remove. (Must be set with add_resistance_target) | remove_resistance_target = 30 | Removes a set resistance target increase in the specified state. | Has no tooltip. | 1.9 |
| set_compliance | / The amount to set the compliance to. | set_compliance = 30 | Sets compliance in the specified state. |  | 1.9 |
| set_resistance | / The amount to set the resistance to. | set_resistance = 30 | Sets resistance in the specified state. | The resistance should be enabled in the state, either via start_resistance or through the in-game process. Occassionally it may take a tick for resistance to start after the controllership change, so it's preferable to do so on states that are given to the country immediately before this gets executed, such as if this is executed in country history. | 1.9 |
| start_resistance | /Whether to start resistance or not. If using a country as the parameter, the state will only start resistance if occupied by the target country. | start_resistance = POL start_resistance = yes | Starts resistance in the specified state. | If used on a state that normally can't start resistance, use alongside with force_enable_resistance. | 1.9 |
| set_garrison_strength | <0-1>The new garrison strength. | set_garrison_strength = 0.5 | Sets the strength of the garrison in the specified state. |  | 1.9 |
| set_occupation_law | The new occupation law enacted by the previous scope or default_law. | GER = {   every_controlled_state = {     set_occupation_law = military_governor_occupation   } }# Changes GER's occupation law for every controlled state. | Sets the occupation law of the state. | PREV will be the country for whom the occupation law will be changed. If PREV is not a country, nothing changes. If PREV doesn't occupy the state, nothing happens until it does. If using default_law, resets to the law set by the country's occupation. Can also be used in country scope. | 1.12 |

### Raids

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| raid_reduce_project_progress_ratio | Value to reduce. | raid_reduce_project_progress_ratio = 0.1 | Reduce progress to the special project in state. Root scope is raid instance scope. The input value is a ratio of the total needed progress to complete the special project, i.e. a decimal number between 0 and 1. |  | 1.15 |

## Character scope

The effects here must be used within a **character** scope.

### General

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| set_character_flag | An unique string to identify the character flag with. OR flag = The flag to set. days = Sets the flag to last for the specified amount of days. Optional. value = The new value of the flag on the scale from -2 147 483 648 to 2 147 483 647. | set_character_flag = my_flag set_character_flag = {     flag = my_flag     days = 123     value = 1 } | Defines a character flag. | No tooltip is shown. The flag in this effect is used in the meaning of 'boolean flag', used to store information. | 1.11 |
| set_character_name | The name to use. | set_character_name = GER_my_cool_flag | Changes the character's name to the specified localisation key's value. |  | 1.11 |
| modify_character_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_character_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. | 1.11 |
| clr_character_flag |  | clr_character_flag = | Clears a character flag |  | 1.11 |
| retire | Boolean> | retire = yes | Retires the current character (removing them). |  | 1.5 |
| set_nationality | / The target country. | set_nationality = GER | Switches the current character to the specified country, giving them the character. | If you wish to change the nationality of a specific character, and the country getting the effect doesn't have the character recruited already, use the every_possible_country = {     limit = { has_character = ID }     random_character = {         limit = { is_character = ID }         set_nationality = TAG     } } command to call them up. Only necessary in 1.11 and beyond. | 1.5 |
| set_portraits | character = The character name. Optional if in character scope. Army scope: small = The sprite used as an advisor. large = The sprite used as a general. Character scope:large = The sprite used as a country leader. | set_portraits = {     character = my_character     army = { small ="MySmallCharacterGFX" }     civilian = { large ="MyLargeCharacterGFX" } } | Changes the specified portraits of a character. | Sprites are defined within /Hearts of Iron IV/interface/*.gfx files. | 1.11 |
| add_trait | slot =  Slot of the character. Necessary for advisors. ideology =  Ideology type of the character. Necessary for country leaders. trait = The trait to add. | add_trait = {     slot = political_advisor     trait = really_good_boss } add_trait = {     ideology = liberalism     trait = field_of_gar } | Adds the specified country leader trait to the character. | Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. The character slot can be the character's name or id. Using name is recommended because 1.11 made id obsolete. | 1.11 |
| remove_trait | slot =  Slot of the character. Necessary for advisors. ideology =  Ideology type of the character. Necessary for country leaders. trait = The trait to remove. | remove_trait = {     slot = political_advisor     trait = really_good_boss } remove_trait = {     ideology = liberalism     trait = field_of_gar } | Removes the specified trait from the character. | Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. The character slot can be the character's name or id. Using name is recommended because 1.11 made id obsolete. | 1.11 |
| add_corps_commander_role | <...>Army leader role definition | add_corps_commander_role = {     skill = 4     attack_skill = 2     defense_skill = 3     planning_skill = 3     logistics_skill = 5 } | Sets the specified character to also act as a corps commander. |  | 1.11 |
| add_field_marshal_role | <...>Army leader role definition | add_field_marshal_role = {   skill = 4   attack_skill = 2   defense_skill = 3   planning_skill = 3   logistics_skill = 5 } | Sets the specified character to also act as a field marshal. |  | 1.11 |
| add_naval_commander_role | <...>Navy leader role definition | add_naval_commander_role = {   skill = 4   attack_skill = 2   defense_skill = 3   planning_skill = 3   logistics_skill = 5 } | Sets the specified character to also act as an admiral. |  | 1.11 |
| add_country_leader_role | character = The character to modify. country_leader = { ... }Country leader role definition promote_leader = Will promote the leader to be the leader of the assigned party. Optional, defaults to false. | add_country_leader_role = {     character = GER_character_token     promote_leader = yes     country_leader = {         ideology = fascism_type         expire = "1965.1.1.1"         traits = { war_industrialist }     } } | Sets the specified character to also act as a country leader, promoting to the party leader if specified. | Does nothing if the character already has a country leader role in the ideology group. | 1.11 |
| promote_character | Boolean. OR The ideology type used by the country leader role. | promote_character = yes promote_character = liberalism | Promotes a character to the leader of their political party. | If the character has multiple country leader roles, specifying the ideology type is mandatory. Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. | 1.11 |
| remove_country_leader_role | ideology = The ideology type of the character. | remove_country_leader_role = {     ideology = socialism } | Removes a country leader role from a character. | Ideology type refers to a sub-type of an ideology group assigned to characters, commonly referred to as sub-ideologies in community jargon. | 1.11 |
| add_advisor_role | advisor = { ... }Advisor role definition activate = Will activate the advisor (add them directly when the command is run to the countries government). Optional, defaults to false. | add_advisor_role = {     activate = yes     advisor = {         slot = air_chief         cost = 50         idea_token = GER_character_token_air_chief         traits = {             air_chief_ground_support_2         }     } } | Sets the specified character to also act as an advisor, activating if specified. | Trigger and effect blocks (such as allowed and on_add) cannot be added within advisor definitions created this way. | 1.11 |
| remove_advisor_role | slot = The slot where to remove the advisor slot from. | remove_advisor_role = {   slot = political_advisor } | Removes the specified advisor role from the character. |  | 1.11 |
| add_scientist_role | <...>Scientist role definition | add_scientist_role = {   scientist = {     desc = desc_loc_key     traits = { scientist_trait_token ... }     skills = { specialization_token = 2 ... }   } } | Adds the scientist role to a character. | The scientist role format is the same as in the character DB. Except the visible trigger, a scientist role created via effect cannot have triggers. Can also be used in country scope. | 1.15 |
| remove_scientist_role |  | remove_scientist_role = yes | Remove the scientist role from a character. | Can also be used in country scope. | 1.15 |
| add_scientist_level | level =  / Level to add. specialization = Specialization to add. | add_scientist_level = {   level = 2   specialization = specialization_nuclear } | Add levels to a special project specialization for a scientist character in scope. |  | 1.15 |
| injure_scientist_for_days | / Amount of days to apply injure. | injure_scientist_for_days = 12 | Injure a scientist for x amount of days to a scientist character in scope. |  | 1.15 |
| add_scientist_trait | Trait to add. | add_scientist_trait = my_trait_token | Add a trait to a scientist character in scope. |  | 1.15 |
| add_scientist_xp | experience =  / Expierience to add. specialization = Specialization to add. | add_scientist_xp = {   experience = 2   specialization = specialization_nuclear } | Add experience to a special project specialization for a scientist character in scope. |  | 1.15 |
| set_can_be_fired_in_advisor_role | slot = The slot of the character to modify. value = The value to set. | set_can_be_fired_in_advisor_role = {     slot = political_advisor     value = no } | Changes the can_be_fired attribute of the advisor, preventing the player from dismissing the advisor. |  | 1.12.8 |

### Unit leaders

These can only be used with characters of the unit leader type.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| unit_leader_event | id = The event to fire. days =  / Fires the event in the specified number of days. Optional.hours =  / Fires the event in the specified number of hours. Optional.random =  / Adds a random number (between 0 and random, inclusive) of hours to the scheduled fire time. Optional.random_days =  / Adds a random number (between 0 and random_days, inclusive) of days to the scheduled fire time. Optional. | unit_leader_event = {     id = my_event.1     days = 10     random = 50     random_days = 10 } | Fires the specified event for the owner of the current unit leader. | Uses a special interface displaying the current unit leader portrait. Where triggers do not need to be repeatedly checked random can be a performance light alternative to mean_time_to_happen for scheduling events. | 1.5 |
| set_unit_leader_flag | An unique string to identify the unit leader flag with. | set_unit_leader_flag = my_flag | Defines a unit leader flag. | Deprecated. Use set_character_flag instead. No tooltip is shown. | 1.5 |
| clr_unit_leader_flag | The unique string of a unit leader flag to clear. | clr_unit_leader_flag = my_flag | Clears a defined unit leader flag. | Deprecated. Use clr_character_flag instead. No tooltip is shown. | 1.5 |
| modify_unit_leader_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_unit_leader_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. Deprecated. Use modify_character_flag instead. | 1.5 |
| promote_leader | Boolean | promote_leader = yes | Promotes the current unit leader to Field Marshal (if Commander). |  | 1.5 |
| demote_leader | Boolean | demote_leader = yes | Demotes the current unit leader to Commander (if Field Marshal). |  | 1.5 |
| add_unit_leader_trait | The trait to add. | add_unit_leader_trait = old_guard | Adds the specified trait to the current unit leader. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt files. | 1.0 |
| remove_unit_leader_trait | The trait to remove. | remove_unit_leader_trait = old_guard | Removes the specified trait from the current unit leader. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt files. | 1.0 |
| add_random_trait | The trait to add. | add_random_trait = { old_guard brilliant_strategist inflexible_strategist } | Adds a random trait from the list to the character. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt files. | 1.5 |
| add_timed_unit_leader_trait | The trait to add. days = The duration of the trait. | add_timed_unit_leader_trait = {     trait = wounded     days = 90 } | Adds the specified trait to the current unit leader for the specified duration. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt files. | 1.5 |
| replace_unit_leader_trait | trait = The trait to replace. replace = The new trait to add. | replace_unit_leader_trait = {     trait = old_guard     replace = brilliant_strategist } | Replaces the specified trait with the new trait. | Traits are found in /Hearts of Iron IV/common/unit_leader/*.txt files. Warning: This effect is extremely buggy. It does not properly replace traits and is crash prone. Use remove_unit_leader_trait and add_unit_leader_trait instead. | 1.5 |
| remove_exile_tag | Remove the exile tag on an army leader, making them no longer be considered exile leaders. | remove_exile_tag = yes | Removes a leaders exile tag. |  | 1.6 |
| gain_xp |  | gain_xp = 5 | Adds experience to the current unit leader, promoting to the next skill level if applicable. | Cannot be used with negatives. | 1.9 |
| remove_unit_leader |  | remove_unit_leader = yes | Removes the current unit leader. |  | 1.0 |
| remove_unit_leader_role | Boolean. | remove_unit_leader_role = yes | Removes every unit leader role from the character |  | 1.11 |

### Country leaders

These can only be used with characters of the country leader type.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_country_leader_trait | The trait to add. OR: ideology = The sub-ideology of the country leader role to which the trait is added. trait = The trait to add. | add_country_leader_trait = nationalist_symbol add_country_leader_trait = {     ideology = marxism     trait = anti_communist } | Adds the specified trait to the current character. | Traits are found in /Hearts of Iron IV/common/country_leader/*.txt files. The former only if the character has one country leader role. | 1.11 |
| remove_country_leader_trait | The trait to remove. OR: ideology = The sub-ideology of the country leader role to which the trait is added. trait = The trait to remove. | remove_country_leader_trait = nationalist_symbol remove_country_leader_trait = {     ideology = marxism     trait = anti_communist } | Removes the specified trait from the current character. | Traits are found in /Hearts of Iron IV/common/country_leader/*.txt files. The former only if the character has one country leader role. | 1.11 |
| swap_country_leader_traits | remove = Trait to remove add = Trait to add ideology = Sub-ideology of the leader where to swap traits. | swap_country_leader_traits = {     remove = nationalist_symbol     add = anti_communist     ideology = marxism } | Swaps traits of the current character. | Use swap_ruler_traits in country scope. | 1.11 |

### Combat

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| supply_units | / The amount of hours of supply. | supply_units = 24 | Adds the specified amount of hours of supply to troops led by the current unit leader. |  | 1.5 |
| add_max_trait | The amount to add. | add_max_trait = 1 | Adds the specified amount of assignable trait slots to the current unit leader. |  | 1.5 |
| add_skill_level | The skill to add. | add_skill_level = 1 | Adds skill to the current unit leader. |  | 1.5 |
| add_logistics | How many skill levels to add. | add_logistics = 1 | Adds logistics skill to the current unit leader. |  | 1.5 |
| add_planning | How many skill levels to add. | add_planning = 1 | Adds planning skill to the current unit leader. |  | 1.5 |
| add_defense | How many skill levels to add. | add_defense = 1 | Adds defense skill to the current unit leader. |  | 1.5 |
| add_attack | How many skill levels to add. | add_attack = 1 | Adds attack skill to the current unit leader. |  | 1.5 |
| add_coordination | How many skill levels to add. | add_coordination = 1 | Adds coordination skill to the current navy leader. |  | 1.5 |
| add_maneuver | How many skill levels to add. | add_maneuver = 1 | Adds maneuver skill to the current navy leader. |  | 1.5 |
| add_temporary_buff_to_units | combat_offense = The bonus to grant. Optional. combat_breakthrough = The bonus to grant. Optional.combat_defense = The bonus to grant. Optional.combat_entrenchment = The bonus to grant. Optional.org_damage_multiplier = The bonus to grant. Optional.str_damage_multiplier = The bonus to grant. Optional.war_support_reduction_on_damage = The bonus to grant. Optional.cannot_retreat_while_attacking = The bonus to grant. Optional.cannot_retreat_while_defending = The bonus to grant. Optional.days = The duration of the buff. Optional.tooltip = The tooltip to display for the buff. | add_temporary_buff_to_units = {     combat_offense = 0.25     combat_breakthrough = 0.25     org_damage_multiplier = -1.0     str_damage_multiplier = 0.25     war_support_reduction_on_damage = 0.2     cannot_retreat_while_attacking = 1.0 				     days = 7     tooltip = ABILITY_FORCE_ATTACK_TOOLTIP } | Adds the specified combat buff to the current unit leader. |  | 1.5 |

### Operatives

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_nationality | The country to set the nationality to. | add_nationality = GER | Adds the nationality to the current operative. |  | 1.9 |
| capture_operative | captured_by = By which country to get captured. ignore_death_chance = Whether to ignore the death chance on capture (no by default). | capture_operative = {     captured_by = POL     ignore_death_chance = yes } | Makes the current operative be captured by a specific country. |  | 1.9 |
| force_operative_leader_into_hiding |  | force_operative_leader_into_hiding = yes | Forces the current operative into hiding. |  | 1.9 |
| free_operative | captured_by = The country that captured the operative. | free_operative = { captured_by = POL } | Frees the current operative. |  | 1.9 |
| harm_operative_leader | How much to harm the operative. | harm_operative_leader = 12 | Harms the current operative. | The value is subject to modifiers. | 1.9 |
| kill_operative | killed_by = The country that'll kill the operative. | kill_operative = { killed_by = POL } | Kills the current operative. |  | 1.9 |
| turn_operative | turned_by = The country to which the operative defects. | turn_operative = {     turned_by = PREV } | Turns the current operative against their own country, transferring them to the specified country. | This counts as the operative dying and will trigger the corresponding On action. Logs an error if used against your own operative. | 1.9 |
| operative_leader_event | id = The event to fire. days =  / Fires the event in the specified number of days. Optional.hours =  / Fires the event in the specified number of hours. Optional.random =  / Adds a random number (between 0 and random, inclusive) of hours to the scheduled fire time. Optional.random_days =  / Adds a random number (between 0 and random_days, inclusive) of days to the scheduled fire time. Optional.originator = The originator of the event. Optional, defaults to owner of operative.recipient = The recipient of the event. Optional, defaults to owner of operative.set_from = Sets the scope of FROM in scripted localization. Optional.set_from_from = Sets the scope of FROM.FROM in scripted localization. Optional.set_root = Sets the scope of ROOT in scripted localization. Optional. | operative_leader_event = {     id = my_event.1 	originator = POL 	recipient = GER     days = 10     random = 50     random_days = 10 	set_from = ENG 	set_root = SOV 	set_from_from = FRA } | Fires the specified event for the operative. | Uses a special interface displaying the current operative portrait. Where triggers do not need to be repeatedly checked random can be a performance light alternative to mean_time_to_happen for scheduling events. | 1.9 |

## Division scope

The effects here must be used within a **division** scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| destroy_unit | Boolean. | destroy_unit = yes | Destroys the currently-scoped division. |  | 1.12 |
| add_history_entry | key = The name of the entry. subject = ""Logged entry. Never shown to the player. allow = Whether a medal can be awarded to the division over the history entry. | add_history_entry = {     key = my_history_entry     subject = "Test entry"     allow = no } | Creates an entry within the command history of a division. |  | 1.12 |
| change_division_template | The name of the division. | change_division_template = {     division_template = "New template" } | Changes the template of the division to the specified one. |  | 1.12 |
| add_random_valid_trait_from_unit | Character to grant the trait to. | add_random_valid_trait_from_unit = FROM | Adds a random valid unit trait to a unit leader. | Only possible to use if the division scope is the same as the ROOT scope. | 1.12 |
| add_unit_medal_to_latest_entry | unit_medals = The medal to add. | add_unit_medal_to_latest_entry = {     unit_medals = my_medal } | Adds the specified medal to the latest entry within the unit's history. |  | 1.12 |
| add_divisional_commander_xp | Experience to add. | add_divisional_commander_xp = 10 | Adds the specified amount of experience to the divisional commander. |  | 1.12 |
| reseed_division_commander | The seed to use. | reseed_division_commander = 760 | Re-randomises the division commander using the given seed. | Does not have a tooltip. | 1.12 |
| promote_officer_to_general | Boolean. | promote_officer_to_general = yes | Promote the officer of the division to a general. |  |  |
| set_unit_organization | The level to set to. | set_unit_organization = 0.3 | Changes the organisation of the unit. | On the scale from 0 to 1. | 1.13 |

## MIO scope

The effects here must be used within a **military industrial organisation** scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_mio_funds | Funds to add. | add_mio_funds = 1000 | Adds funds to the MIO. | If the amount goes above the "Size Up" limit, the MIO will automatically gains sizes. The amount of funds is capped at 0 from below. | 1.13 |
| set_mio_funds | Amount to set. | set_mio_funds = 1000 | Sets the funds of a MIO to the certain level. | If the amount goes above the "Size Up" limit, the MIO will automatically gains sizes. Cannot be negative. | 1.13 |
| add_mio_funds_gain_factor | Amount to add. | add_mio_funds_gain_factor = 0.1 | Changes the base multiplier to MIO's funds. | The multiplier is capped at 0 from below. | 1.13 |
| set_mio_funds_gain_factor | Amount to set. | set_mio_funds = 0.1 | Changes the base multiplier to MIO's funds. | Cannot be negative. | 1.13 |
| add_mio_size | Amount to add. | add_mio_size = 2 | Adds sizes to the MIO. | Funds will not be changed by the effect. Cannot be negative. | 1.13 |
| add_mio_size_up_requirement_factor | Amount to add. | add_mio_size_up_requirement_factor = 0.1 | Changes the base multiplier to the requirement to size up a MIO. | The multiplier is capped at 0 from below. | 1.13 |
| set_mio_size_up_requirement_factor | Amount to set. | set_mio_size_up_requirement_factor = 0.1 | Changes the base multiplier to the requirement to size up a MIO. | Cannot be negative. | 1.13 |
| add_mio_task_capacity | Amount to add. | add_mio_task_capacity = 2 | Changes the base maximum task capacity of the MIO. | If the capacity is reduced to below the amount of assigned tasks, they'll be turned allowed. The base amount is capped at 0 from below. Doesn't instantly apply. | 1.13 |
| set_mio_task_capacity | Amount to set. | set_mio_task_capacity = 2 | Changes the base maximum task capacity of the MIO. | If the capacity is reduced to below the amount of assigned tasks, they'll be turned allowed. Cannot be negative. Doesn't instantly apply. | 1.13 |
| add_mio_research_bonus | Amount to add. | add_mio_research_bonus = 0.3 | Changes the base research bonus of the MIO. | The base amount is capped at 0 from below. | 1.13 |
| set_mio_research_bonus | Amount to set. | set_mio_research_bonus = 0.3 | Changes the base research bonus of the MIO. | Cannot be negative. | 1.13 |
| set_mio_name_key | The new name. | set_mio_name_key = mio_new_name | Changes the name of the MIO. | May also refer to a scripted localisation definition, which'll be evaluated in MIO's scope. | 1.13 |
| set_mio_icon | The new sprite. | set_mio_icon = GFX_new_mio_icon | Changes the MIO's icon. |  | 1.13 |
| add_mio_design_team_assign_cost | Amount to add. | add_mio_design_team_assign_cost = 0.3 | Changes the base political power cost of the MIO to assign research. | The base amount is capped at 0 from below. | 1.13 |
| set_mio_design_team_assign_cost | Amount to set. | set_mio_design_team_assign_cost = 0.3 | Changes the base political power cost of the MIO to assign research. | Cannot be negative. | 1.13 |
| add_mio_industrial_manufacturer_assign_cost | Amount to add. | add_mio_industrial_manufacturer_assign_cost = 0.3 | Changes the base political power cost of the MIO to assign production lines. | The base amount is capped at 0 from below. | 1.13 |
| set_mio_industrial_manufacturer_assign_cost | Amount to set. | set_mio_industrial_manufacturer_assign_cost = 0.3 | Changes the base political power cost of the MIO to assign production lines. | Cannot be negative. | 1.13 |
| add_mio_design_team_change_cost | Amount to add. | add_mio_design_team_change_cost = 0.3 | Changes the base experience cost of the MIO to assign to equipment by a percentage. | The base amount is capped at 0 from below. Rounded down, e.g. 0.3 with a cost of 5 should result in 6.5, but becomes 6 instead. | 1.13 |
| set_mio_design_team_change_cost | Amount to set. | set_mio_design_team_change_cost = 0.3 | Changes the base experience cost of the MIO to assign to equipment by a percentage. | Cannot be negative. Rounded down, e.g. 0.3 with a cost of 5 should result in 6.5, but becomes 6 instead. | 1.13 |
| unlock_mio_trait_tooltip | Trait to display. OR trait = Trait to display. show_modifiers = Whether the trait's modifiers should be shown in the tooltip. Defaults to true. | unlock_mio_trait_tooltip = my_trait_1 unlock_mio_trait_tooltip = {     trait = my_trait_2     show_modifiers = no } | Displays a tooltip that says that the trait is made available. | Doesn't change the availability of the trait directly. | 1.13 |
| complete_mio_trait | Trait to complete. OR trait = Trait to complete. show_modifiers = Whether the trait's modifiers should be shown in the tooltip. Defaults to true. | complete_mio_trait = my_trait_1 complete_mio_trait = {     trait = my_trait_2     show_modifiers = no } | Completes the specified MIO trait. | Automatically adds 1 size to the MIO. No checks are placed on the trait. | 1.13 |
| set_mio_flag | An unique string to identify the MIO flag with. OR flag = The flag to set. days = Sets the flag to last for the specified amount of days. Optional. value = The new value of the flag on the scale from -2 147 483 648 to 2 147 483 647. | set_mio_flag = my_flag set_mio_flag = {     flag = my_flag     days = 123     value = 1 } | Defines a MIO flag. | No tooltip is shown. | 1.13 |
| clr_mio_flag | The unique string of a country flag to clear. | clr_mio_flag = my_flag | Clears a defined MIO flag. |  | 1.13 |
| modify_mio_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_mio_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. | 1.13 |

## Contract scope

The effects here must be used within a **contract** scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| cancel_purchase_contract | Boolean. | cancel_purchase_contract = yes | Cancels the current purchase contract. |  | 1.13 |

## Raid scope

The effects here must be used within a **raid** scope.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_raid_history_entry |  | add_raid_history_entry = yes/no | Add history entry to a raid. |  | 1.15 |
| raid_add_unit_experience | Can use either an explicit value or a variable | raid_add_unit_experience = 0.2 | Will give experience to any type of unit assigned to the raid, e.g. divisions or air wings. | The value defines the progress towards the max level, e.g. 0.2 = gain 20% of the experience needed to reach max level. | 1.15 |
| raid_damage_units | An unique string to identify the project flag with. OR damage = The amount of strength and organization damage taken. org_damage = The amount of organization damage taken. str_damage = The amount of strength damage taken plane_loss = The amount of planes lost ratio = optional, default no | # Apply 50% damage to units raid_damage_units = { 	damage = 0.5 	ratio = yes }  # Apply 10 strength loss and 20 organization loss to units raid_damage_units = { 	org_damage = 20 	str_damage = 10 }  # Lose 40% of all planes raid_damage_units = { 	plane_loss = 0.4 	ratio = yes }  # Lose 5 planes raid_damage_units = { 	plane_loss = 5 } | Damage is applied to ground units while damage to plane is defined as the amount of planes lost. | If 'ratio = yes', then all damage / losses are applied as a fraction of the current amount. For units, damage can be defined through one value 'damage' or separately through 'org_damage' and 'str_damage' | 1.15 |

## Special Project scope

The effects here must be used within a **special project** scope. Special projects must always be pre-pended with `sp:` when used a a scope or value.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| add_project_progress_ratio | remove or add between -1 and 1 proect progress | sp:my_project = {   add_project_progress_ratio = 0.1   add_project_progress_ratio = var:my_var } | Add progress to the project's prototype phase. | The input value is a ratio of the total needed progress to complete the special project. | 1.15 |
| complete_prototype_reward_option | prototype_reward = The protypereward to compete prototyp_reward_option = my_optionIf multiple choice use the given one, use default one if not set. Optional. show_modifiers = Yes if the effects of the prototype reward should be shown (default no) | complete_prototype_reward_option = { 	prototype_reward = my_reward 	prototyp_reward_option = my_option 	show_modifiers = yes } | Complete a prototype reward option for the project in scope | The effect will respect the fire only once and allowed property of prototype rewards. | 1.15 |
| set_project_flag | An unique string to identify the project flag with. OR flag = The flag to set. days = Sets the flag to last for the specified amount of days. Optional. value = The new value of the flag on the scale from -2 147 483 648 to 2 147 483 647. | set_project_flag = my_flag set_project_flag = {     flag = my_flag     days = 123     value = 1 } | Defines a project flag. | No tooltip is shown. | 1.15 |
| clr_project_flag | The unique string of a country flag to clear. | clr_project_flag = my_flag | Clears a defined project flag. |  | 1.15 |
| modify_project_flag | flag = The flag to modify. value = The value to add to the flag. Defaults to 0. days = The amount of days that the flag should last for before being cleared. Optional, defaults to permanent. | modify_mproject_flag = {     flag = my_flag     value = 3 } | Adds an integer value to a flag. | The flag must be already set. | 1.15 |

## Other scopes

The effects here must be used within a scope that's specified within the notes.

| Name | Parameters | Examples | Description | Notes | Version Added |
| --- | --- | --- | --- | --- | --- |
| execute_operation_coordinated_strike | amount = How many times the operation will get executed within the days set in the operation. | execute_operation_coordinated_strike = {     amount = 12 } | All prepared Port Strike and Strategic Bombing in the target region will execute multiple times without air defence being able to intercept them. | Can only be used within operations. | 1.9 |

## Flow control

These scopes are used within effect scopes to control the execution of effects.

### If statements

An [if statement](http://en.wikipedia.org/wiki/Conditional_(computer_programming)#If–then(–else)) allows an execution of effects to only be done if certain [triggers](/Triggers) are met. Conditional statements are represented with the `if = { ... }` effect. `limit = { ... }` inside of the if statement serves as a [trigger block](/Triggers) that defines the conditions when it should be executed, and everything else directly inside of `if = { ... }` is interpreted as the effects that should be executed if the condition is true.
For example, the following will add 10% [/Stability](/Stability)[Stability](/Stability) to the country this is executed on if it has positive [/Government#Political_power](/Government#Political power)[Political Power](/Government#Political power) and below 90% stability:

```
if = {
    limit = {
        has_political_power > 0
        stability < 0.9
    }
    add_stability = 0.1
}
```

If the limit is not met, then none of the effects inside will be executed. If it is, then each one will be. If the limit is omitted, it defaults to being always true.
**The effects must be inside of the if statement to be tied to the limit**. For example, this will always give 100 [/Government#Political_power](/Government#Political power)[Political Power](/Government#Political power), regardless of what country is played:

```
if = {
    limit = { tag = BHR }
}                         # Closes if = { ... }. Since no effects are inside, this means that the if statement does absolutely nothing
add_political_power = 100 # Outside of if = { ... }, so it will always give 100 political power, even if not playing as BHR
```

Optionally, `else_if = { ... }` (with `limit = { ... }` serving in a similar fashion) and `else = { ... }` can be added. If the initial limit within `if = { ... }` is false, it moves on to the next `else_if = { ... }`, checking the limit there. If the limit there is false, then it moves on to the next one, until hitting an end or an `else = { ... }`.
Two variants exist: nested and unnested. In the first case, the `else_if` or `else` is put directly inside of the preceding `if` or `else_if`, while in the second case it's put *right after*. In case of overlap, unnested if statements are preferred. Here is an example using unnested if statements:

```
if = {
    limit = {
        stability < 0.3 # If stability is below 30%, add 30%.
    }
    add_stability = 0.3
}
else_if = {
    limit = {
        stability < 0.6 # Otherwise, if it's below 60% (i.e. 30-59%), add 20%
    }
    add_stability = 0.2
}
else = {
    add_stability = 0.1 # If there's 60-100% stability, add 10%
}
```

Within the tooltip, only effects that would be executed are shown. The effects within an unfulfilled if statement (or an `else`/`else_if` that's not read due to the if statement being met) will be hidden from the player, and so will the trigger. In order to avoid player confusion, [custom effect tooltips can be used to tell the player what this effect block would do](#Effect tooltips), such as being used within an `else`.

### Random effects

If you want an effect to have a random chance to be done or have nothing happen otherwise, the `random = { ... }` block is the simplest way to accomplish that:

```
random = {
    chance = 80
    add_stability = 0.4
    add_war_support = 0.3
}
```

This in particular will have an 80% chance to add 40% stability and 30% war support and, accordingly, a 20% chance to do nothing. The chance here is on the scale from 0 to 100.
If you want the game to choose between effect blocks, random_list can be used instead. For example, if you wanted an effect to randomly given the player one out of four bonuses, you'd do the following:

```
random_list = {
    10 = {
        add_stability = 0.5
    }
    10 = {
        add_manpower = 10000
    }
    10 = {
        add_war_support = 0.5
    }
    10 = {
        army_experience = 100
    }
}
```

The number is not the chance, but the weight for each option, as they don't have to add up to 100 or any number. An option with the weight of 20 is twice as likely to be picked as the option with the chance of 10, for instance. In total, the probability for an option to be picked is equal to the weight of the option divided by the sum of all weights.
It is also possible to use modifiers (akin to [MTTH blocks](/MTTH)) to affect the weight of each possible random effect or to use [variables](/Variables) as chances.

```
random_list = {
    30 = {
        modifier = {
            factor = 1.3
            has_country_flag = inward_perfect_flag
        }
        add_stability = 0.5
    }
    25 = {
        add_manpower = 10000
    }
    20 = {
        add_war_support = 0.5
    }
    my_variable = { # Taking "my_variable" as the variable's name, both "var:my_variable" and "my_variable" are valid options, left up to the developer's preference.
        army_experience = 100
    }
}
```

If the country flag inward_perfect_flag is set, it'll multiply the above chance of 30 by 1.3 to get 39. Meanwhile, `my_variable` will take the value of the according temp variable or the current scope's variable as the weight of the option.
Note that if you want to create a repeatable decision including a random list, by default the same decision will pick the same random result every time it is triggered in a game. You can reverse this behaviour by including the following line in the decision block:

```
fixed_random_seed = no
```

**This is only for decisions**. Elsewhere, random seed is unfixed by default, making this argument unnecessary to set to "no".

### Tooltip manipulation

*See also: [Localisation](/Localisation)*

The "tooltip" in this case refers to the text shown to the player in-game that explains what the effect block changes within the game, such as "**+50** [/Government#Political_power](/Government#Political power)[Political Power](/Government#Political power)".
There are 3 ways to edit the tooltip within an effect block:

- `hidden_effect = { ... }` is used in order to hide the effects within from the tooltip, making their execution not get shown to the player.
- `effect_tooltip = { ... }` is, instead, used in order to put the effects into the tooltip without actually executing them.
- `custom_effect_tooltip = my_localisation_key` is used in order to put an arbitrary paragraph of text as an effect that will get executed.

For example, this sample [focus' completion reward](/National focus modding) utilises all three:

```
completion_reward = {
    hidden_effect = {
        every_subject_country = { country_event = my_event.1 }
    }
    custom_effect_tooltip = send_event_to_subjects_tt
    effect_tooltip = {
        add_political_power = 100
    }
    custom_effect_tooltip = reject_war_tt
}
```

In this case, send_event_to_subjects_tt and reject_war_tt are localisation keys defined within any /Hearts of Iron IV/localisation/english/*_l_english.yml file encoded with UTF-8-BOM, assuming the English language.

```
send_event_to_subjects_tt: "Sends a demand to our every subject.\nIf they agree, we get the following for each subject:"
 reject_war_tt: "If they reject the demand, we gain a wargoal against them."
```

In-game, this will appear as such:

Effect:
Sends a demand to our every subject.
If they agree, we get the following for each subject:
Political Power: **+100**
If they reject the demand, we gain a wargoal against them.

Noticably, the effect that fires the country event gets hidden from the tooltip. After completing the focus, the only thing that happens is that every subject country receives an event with the ID of `my_event.1`, the country does not immediately gain 100 political power.

## Meta effects

Meta effects allow you to use non-dynamic effects (the ones that do not accept modifiers and can only use static tokens or constant values) as if they were accepting variables.

```
add_equipment_to_stockpile = {
    type = infantry_equipment_2
    amount = eq_amount
}
```

In the effect shown above, amount of equipment added is dynamic and can be set using the variable "eq_amount". However, this effect does not let you use a variable as equipment type. You can not store "infantry_equipment_2" in a variable and use it here.
However, meta effects will let you use variables and scripted localization within them to build effects as if they were texts and run them. Let's make previous effect accept equipment type and equipment level as variables stored in "eq_type" and "eq_level".

```
set_variable = { eq_type = 1 } # Sets the equipment type to "1", which determines the equipment given using scripted localisation, included below
set_variable = { eq_amount = 10 } # Sets the amount of equipment given to 10
set_variable = { eq_level = 2 } # Sets the equipment level to 2, which is used directly in the meta effect, no scripted localisation required

meta_effect = { # The actual meta effect. This can go anywhere you need it: in a decision, in a scripted effect, in a scripted GUI click effect, etc...
    text = {
        add_equipment_to_stockpile = {
            type = [EQ_TYPE]_[EQ_LEVEL]
            amount = eq_amount
        }
    }
    EQ_LEVEL = "[?eq_level|.0]" # Gets the "eq_level" variable and saves it as "EQ_LEVEL" for the meta effect to use
    EQ_TYPE = "[This.GetEquipmentName]" # Gets the equipment type from scripted localisation, included below, based on the "eq_type" variable, and saves it as "EQ_TYPE" for the meta effect to use
}
```

```
# The scripted localization for the "eq_type" variable, which goes in a scripted localisation file
defined_text = { # Since the "eq_type" variable in this example is equal to 1, the equipment given by the effect is "artillery_equipment"
    name = GetEquipmentName
    text = {
        trigger = {
            check_variable = { eq_type = 0 }
        }
        localization_key = "infantry_equipment"
    }
    text = {
        trigger = {
            check_variable = { eq_type = 1 }
        }
        localization_key = "artillery_equipment"
    }
}
```

As you can see, we have created a meta_effect that takes two arguments. These arguments will be used replacing the parameters [EQ_TYPE] and [EQ_LEVEL] inside the meta effect. EQ_LEVEL will be replaced by [?eq_level|.0] which is the integer value of eq_level (in this case 2.000 becomes 2). EQ_TYPE is a bit more complicated, it is being replaced by a scripted localization. This scripted localization will check eq_type variable and depending on its value it will return the key token for the equipment. If it is 0, it will return "infantry_equipment". If it is 1, it will return "artillery_equipment".
So the final result is [EQ_TYPE] is being replaced by "artillery_equipment" and [EQ_LEVEL] is being replaced by "2" and in the end our effect will be built as:

```
add_equipment_to_stockpile = {
    type = artillery_equipment_2
    amount = eq_amount
}
```

which will give you 10 artillery_equipment_2.
debug = yes can be added to meta effects. Which will print the final effect to game.log when the effect is executed and make debugging easier.

## Scripted effects

Scripted effects serve a similar purpose to [functions](https://en.wikipedia.org/wiki/Subroutine) in that they can be defined in /Hearts of Iron IV/common/scripted_effects/*.txt and then used elsewhere as a shortened version. **A scripted effect will never run by itself** and requires being used as an effect elsewhere to be executed. Alongside that, the game allows the creation of custom console commands, which are scripted effects.

A scripted effect is defined simply as

```
scripted_effect_name = {

}
```

This example can be used as an effect in regular code as `scripted_effect_name = yes`.
Scripted effects can be accessed in console by typing `e scripted_effect_name` to run them.
To create a custom console command, the scripted effect's name should begin with `d_`. The console command itself does not include `d_`, so `d_test_command` would be run in console as `test_command`
In custom console commands, the country running the command is FROM, while ROOT is the selected country, state, or character. Anything entered after the console command, separated by spaces like `test_command 123 321 GER` is added to the 'args' temp [array](/Arrays). An example of a scripted effect which will transfer every state entered as an argument to the country that runs the console command is

```
d_transfer_states = {
	for_each_scope_loop = {
		array = args
		FROM = {
			transfer_state = PREV
		}
	}
}
```

used like `transfer_states 123 321`

### Useful scripted effects

These scripted effects are defined in base game and might be useful to keep in the mod to cut down on the amount of code. As scripted effects, all of these use a boolean value as argument.

| Name | Scope | Example | Description | Notes |
| --- | --- | --- | --- | --- |
| instantiate_collaboration_government | Country | instantiate_collaboration_government = yes | Creates a collaboration government, with the current scope as overlord. | The target of the collaboration government is stored in the country_to_initiate temp variable. |
| add_potential_special_forces_tree | Country | add_potential_special_forces_tree = yes | Adds 1 special forces branch specialism |  |
| upgrade_economy_law | Country | upgrade_economy_law = yes | Switches the economy law one level towards total mobilisation. | If already on total mobilisation, adds 150 Political Power. Must be adjusted manually for new laws. |
| gain_random_agency_upgrade | Country | gain_random_agency_upgrade = yes | Grants a random available intelligence agency upgrade. | Only results in an agency being created if one doesn't exist. |
| add_ruling_to_dem | Country | add_ruling_to_dem = yes | All of the ruling party's popularity gets added to the Democratic ideology group. | Requires manual adjustment if new ideologies are added. See also: add_ruling_to_fas, add_ruling_to_com, add_ruling_to_neu |
| remove_any_country_role_from_character | Character | remove_any_country_role_from_character = yes | Removes all advisor roles from the current scope. | Requires manual adjustment if new slots are added. |
| increase_state_category | State | increase_state_category = yes | Changes the state category to the next one that contains more building slots. | Has no effect on small islands, megalopolises, or large_city (Dense Urban Region). city (Urban Region) gets upgraded straight to Metropolis, skipping large_city. |
| lerp | Any | lerp = yes | Creates the lerp_result regular variable with Failed to parse (SVG (MathML can be enabled via browser plugin): Invalid response ("Math extension cannot connect to Restbase.") from server "https://en.wikipedia.org/api/rest_v1/":): {\displaystyle result := a + (b-a)\cdot x} | Failed to parse (SVG (MathML can be enabled via browser plugin): Invalid response ("Math extension cannot connect to Restbase.") from server "https://en.wikipedia.org/api/rest_v1/":): {\displaystyle a} , Failed to parse (SVG (MathML can be enabled via browser plugin): Invalid response ("Math extension cannot connect to Restbase.") from server "https://en.wikipedia.org/api/rest_v1/":): {\displaystyle b} , and Failed to parse (SVG (MathML can be enabled via browser plugin): Invalid response ("Math extension cannot connect to Restbase.") from server "https://en.wikipedia.org/api/rest_v1/":): {\displaystyle x}  are stored as lerp_a, lerp_b, lerp_x temp variables. Failed to parse (SVG (MathML can be enabled via browser plugin): Invalid response ("Math extension cannot connect to Restbase.") from server "https://en.wikipedia.org/api/rest_v1/":): {\displaystyle x}  is clamped between 0 and 1. |
| store_core_states_on_game_start | Country | store_core_states_on_game_start = yes | Stores the current core states of the current scope in an array in ROOT's scope. | The created array will be named core_states_at_game_start. Intended to be called in country history only once. |

**[Modding](/Modding)**
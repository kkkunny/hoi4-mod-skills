# Sound modding

Sound definitions are found in /Hearts of Iron IV/sound. A sound file should be saved as a **WAV** file in Stereo, as a mono channel, at 44100Hz and as a 32-bit float.

All of the sound definition files must be saved as **.asset** files.

## Sound

A **sound** entry is used to define a sound. It follows this format:

```
sound = {
    name =
    file =

    always_load =
    volume =
}
```


**name** is the name of the sound definition that is referred to by other files.

**file** is the path to the sound file, relative to the Hearts of Iron IV **sound** folder.

**always_load** defines whether the sound is always loaded.

**volume** defines the volume of the sound.

## Sound Effect

A **soundeffect** entry is used to define a soundeffect. It follows this format:

```
soundeffect = {
    name =
    falloff =
    sounds = {
        sound =
        weighted_sound = {
            sound =
            weight = int
        }
    }

    loop =
    is3d =
    random_sound_when_looping =

    max_audible =
    max_audible_behaviour =

    volume =
    fade_in =
    fade_out =

	looping_delay_random_offset =
    delay_random_offset = {


    }

	looping_playbackrate_random_offset =
    playbackrate_random_offset = {


    }

    volume_random_offset = {


    }

    prevent_random_repetition =
}
```

**name** is the name of the soundeffect.

**falloff** is the falloff entry used by the sound effect.

**sounds** is the sound entries used by the sound effect.

**loop** defines whether the soundeffect loops.

**is3d** defines whether the sound effect should utilise 3D sound.

**random_sound_when_looping** defines whether the soundeffect picks a random sound when looping, rather than picking iteratively.

**max_audible** defines the maximum amount of instances of the sound effect in one moment.

**max_audible_behaviour** defines what happens when more than *max_audible* instances happens. It is always **fail**.

**volume** defines the volume of the sound effect.

**fade_in** defines the fade in duration for the sound effect.

**fade_out** defines the fade out duration for the sound effect.

**looping_delay_random_offset** ???

**delay_random_offset** defines the minimum and maximum random offset to the delay between sound loops.

**looping_playbackrate_random_offset** ???

**playbackrate_random_offset** defines the minimum and maximum random offset to the playback rate between sound loops.

**volume_random_offset** defines the minimum and maximum random offset to the volume between sound loops.

**prevent_random_repetition** prevents the same sounds from playing right after it was played

## Falloff

Falloff entries define the falloff attributes for sound. They are added in sound effects, and follow this format:

```
falloff = {
    name =

    min_distance =
    max_distance =
    height_scale =
}
```

**name** is the name of the falloff entry.

**min_distance** is the minimum distance before falloff is applied, i.e. max volume

**max_distance** is the maximum distance before no sound is heard.

**height_scale** is a scalar for the height between the sound source and the player camera.

## Categories

Sound effects can be placed in sound categories that apply a specific compressor to the sounds. The categories follow this format:

```
category = {
    name =
    soundeffects = {

    }
    compressor = {
        enabled = yes
        pregain =
        postgain =
        ratio =
        threshold =
        attacktime =
        releasetime =
    }
}
```

**name** is the name of the soundeffect category.

**soundeffects** is a list of the sound effects belong to the category.

**compressor** is the compressor to use for the category, see below for more information on the attributes.

## Compressors

There are two global compressors: **master_compressor** used for sounds and **music_compressor** used for music. Individual compressors can be defined for sounds within categories.

The compressors use the following format:

```
 = {
    pregain =
    postgain =
    ratio =
    threshold =
    attacktime =
    releasetime =
}
```

**[Modding](/Modding)**
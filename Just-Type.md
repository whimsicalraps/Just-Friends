# Just Type

monome's *Teletype* (and since, *crow*) excites us. Writing simple scripts away from a computer; executing tiny morsels of musical composition. Just Type is a suggestion for how these ideas can be extended & deeply integrated with elements of synthesis.

At it's most basic, Just Type is a set of invisible patch cords. But more than a cloaking device, it extends the base functionality of Just Friends into more complex territory. Every output can be driven with varying velocity, and the INTONE relationship can be altered away from the default harmonic structure.

Beyond these general-purpose modifications, Just Type brings two entirely new modalities. *Synthesis* allows explicit or automatic polyphonic control over each channel. *Geode* instead pursues rhythmic manipulation of striated repetitions, creating polymetric bursts with dynamic decay.

Enough! Just. Type.

*[Jump to the Teletype reference](#teletype-reference)*

*[Jump to the crow reference](#crow-reference)*


## Install?

Just Type has shipped on every new module made since late 2017. A substantial update was released in July 2020, opening up a host of new features, and fixing many shortcomings of the original formulation.

Get the [latest release](https://github.com/whimsicalraps/Just-Friends/releases/latest) and you'll be all set to type!

## On reading this docs

Each command has a description of the names & syntax for usage. Both Teletype & crow syntax is displayed and will take the following form:

TT
* `JF.COMMAND value` (set)
* `<- JF.COMMAND` (get)

crow
* `ii.jf.command( value )` (set)
* `ii.jf.get( 'command' )` (get)

Each command has 1 or 2 forms listed. These can be of 2 types:
* 'setters' tell Just Friends to do something without expecting a response.
* 'getters' ask Just Friends to return a value.

Some commands are 'set' only, while others and 'get' only, but many have both functionalities. What's important is to recognize which option you need in your script.

Furthermore, getters work quite differently on Teletype vs crow. For Teletype, the getter will query the value and return it directly. On crow, the response to a `get` call will come through the `ii.jf.event( event, value )` function which you must add to your script. For an example, you can call `ii.jf.help()` and crow will show you what this event function should look like.

Some of these commands have not been implemented on Teletype and are marked with *(proposed)* to indicate they are not currently accessible.


## The basics: Remote control

These commands allow remote control over Just Friends. Imagine a set of invisible patch cables connected to the TRIGGERS and RUN jacks.

#### Triggers

TT: `JF.TR channel state`
crow: `ii.jf.trigger( channel, state )`

Create a TRIGGER event on the `channel` with the provided `state`.

`channel`
* 1 is IDENTITY, and 6 is 6N
* 0 creates a TRIGGER on *all* channels (hardware normalization is ignored)

`state`
* 1 is *high* (5V). All non-zero values treated as high.
* 0 is *low* (0V)

Only *sustain* cares about the 'low' triggers (the others modes will simply ignore this message).

#### Run Mode

TT: `JF.RMODE mode`
crow: `ii.crow.run_mode( mode )`

Set the RUN state of Just Friends when no physical jack is present.

`mode`
* 1 activates RUN mode. All non-zero values are treated as high
* 0 deactivates RUN. If a physical jack is present, RUN stays high.


#### Run Voltage

TT get/set:
`JF.RUN` `JF.RUN volts`

crow
* set: `ii.jf.run( volts )`

Send a 'voltage' to the RUN input.

`volts`
* The voltage to be virtually sent to the RUN jack. Adds to the physical state
* on TT use `JF.RUN V x` to set to x volts.
* Range is -5 to +5

*Requires JF.RMODE 1 to have been executed*.


## Extended behaviour


## Modal Personality


### Synthesis


### Geode








## TODO #########

NEW Geode now responds to RUN voltages in all modes
NEW ii 'pitch' command sets the denoted voice's pitch without retriggering its envelope
NEW ii 'address' command allows to use a secondary i2c address for 2 Just Friends devices on a single ii bus
NEW support for ii 'getters' so that JF's hardware state can be queried remotely
NEW Geode now accepts 'float' ii commands from crow in range (1.0 .. 20.0)











# Teletype Reference

### Set values or call actions:

`JF.TR channel state` Set trigger *channel* to *state*
`JF.RMODE mode` Set RUN state to *mode*
`JF.RUN volts` Set RUN voltage to *volts*
`JF.SHIFT volts` Transpose frequency / speed by *volts* (v/8)
`JF.VTR channel level` Trigger *channel* with velocity set by *level*
`JF.TUNE channel numerator denominator` Alter the INTONE relationship to IDENTITY
`JF.MODE state` Activates *Synthesis* or *Geode*

*Synthesis*

`JF.NOTE pitch level` Play a note, dynamically allocated to a voice
`JF.VOX chanel pitch level` Play a note on a specific voice
`JF.GOD state` If *state*, retune to A=432Hz (default A=440Hz)

*Geode*

`JF.NOTE divs repeats` Play a sequence, dynamically allocated to a channel
`JF.VOX chanel divs repeats` Play a sequence on a specific *channel*
`JF.TICK divs` Clock *Geode* with a stream of ticks at *divs* per measure
`JF.TICK bpm` Set timebase for *Geode* with a static *bpm*
`JF.QT divs` Quantize *Geode* events to *divs* of the timebase

*Proposed commands, yet to be implemented as of Teletype 3.2*

`JF.ADDR index` Set all connected Just Friends to ii address *index*
`JF.PITCH channel pitch` Same as `JF.VOX` but doesn't trigger the envelope

### Get values from Just Friends:

*Proposed getters, yet to be implemented as of Teletype 3.2*

* `JF.TR channel` Returns true if the channel is in motion
* `JF.RMODE` Returns the state of run_mode (ignores the jack)
* `JF.RUN` Returns the current RUN value (ignores the jack)
* `JF.SHIFT` Returns the current transpose setting
* `JF.MODE` Returns 1 if *Synthesis* or *Geode* are active
* `JF.TICK` Returns the current *Geode* tempo in beats per minute
* `JF.GOD` Returns 1 if god mode is active
* `JF.QT` Returns the number of *divisions* quantize is set to
* `JF.SPEED` Returns the current *shape* (0) or *sound* (1) switch position
* `JF.TSC` Returns the current *MODE* switch state (1/2/3)
* `JF.RAMP` Returns the current state of the *RAMP* knob
* `JF.CURVE` Returns the current state of the *CURVE* knob
* `JF.FM` Returns the current state of the *FM* knob
* `JF.TIME` Returns the current state of the *TIME* knob + cv
* `JF.INTONE` Returns the current state of the *INTONE* knob + cv

# crow reference

### Set values or send commands:

```lua
ii.jf.trigger( channel, state ) -- Set trigger *channel* to *state*
ii.jf.run_mode( mode ) -- Set RUN state to *mode*
ii.jf.run( volts ) -- Set RUN voltage to *volts*
ii.jf.transpose( pitch ) -- Transpose frequency / speed by *pitch* (v/8)
ii.jf.vtrigger( channel, level ) -- Trigger *channel* with velocity set by *level*
ii.jf.retune( channel, numerator, denominator ) -- Alter the INTONE relationship to IDENTITY
ii.jf.mode( mode ) -- Activates *Synthesis* or *Geode*
ii.jf.address( index ) -- Set all connected Just Friends to ii address *index*
```

*Synthesis*

```lua
ii.jf.play_note( pitch, level ) -- Play a note, dynamically allocated to a voice
ii.jf.play_voice( channel, pitch, level ) -- Play a note on a specific voice
ii.jf.pitch( channel, pitch ) -- Same as play_voice but doesn't trigger the envelope
ii.jf.god_mode( state ) -- If *state*, retune to A=432Hz (default A=440Hz)
```

*Geode*

```lua
ii.jf.play_note( divs, repeats ) -- Play a sequence, dynamically allocated to a channel
ii.jf.play_voice( channel, divs, repeats ) -- Play a sequence on a specific *channel*
ii.jf.tick( divs ) -- Clock *Geode* with a stream of ticks at *divs* per measure
ii.jf.tick( bpm ) -- Set timebase for *Geode* with a static *bpm*
ii.jf.quantize( divisions ) -- Quantize *Geode* events to *divisions* of the timebase
```

### Request values and states:

```lua
ii.jf.get( 'trigger', channel )` -- Returns true if the channel is in motion
ii.jf.get( 'run_mode' ) -- Returns the state of run_mode (ignores the jack)
ii.jf.get( 'run' ) -- Returns the current RUN value (ignores the jack)
ii.jf.get( 'transpose' ) -- Returns the current transpose setting
ii.jf.get( 'mode' ) -- Returns 1 if *Synthesis* or *Geode* are active
ii.jf.get( 'tick' ) -- Returns the current *Geode* tempo in beats per minute
ii.jf.get( 'god_mode' ) -- Returns 1 if god mode is active
ii.jf.get( 'quantize' ) -- Returns the number of *divisions* quantize is set to
ii.jf.get( 'speed' ) -- Returns the current *shape* (0) or *sound* (1) switch position
ii.jf.get( 'tsc' ) -- Returns the current *MODE* switch state (1/2/3)
ii.jf.get( 'ramp' ) -- Returns the current state of the *RAMP* knob
ii.jf.get( 'curve' ) -- Returns the current state of the *CURVE* knob
ii.jf.get( 'fm' ) -- Returns the current state of the *FM* knob
ii.jf.get( 'time' ) -- Returns the current state of the *TIME* knob + cv
ii.jf.get( 'intone' ) -- Returns the current state of the *INTONE* knob + cv
```

Receive requested values (only the names you are querying need to be included in this list):

```lua
ii.jf.event = function( e, value )
   if e.name == 'trigger' then
       -- handle trigger response
           -- e.arg: first argument, ie channel
           -- e.device: index of device
   elseif e.name == 'run_mode' then
   elseif e.name == 'run' then
   elseif e.name == 'transpose' then
   elseif e.name == 'mode' then
   elseif e.name == 'tick' then
   elseif e.name == 'god_mode' then
   elseif e.name == 'quantize' then
   elseif e.name == 'speed' then
   elseif e.name == 'tsc' then
   elseif e.name == 'ramp' then
   elseif e.name == 'curve' then
   elseif e.name == 'fm' then
   elseif e.name == 'time' then
   elseif e.name == 'intone' then
   end
end
```

lovemidi
========

lovemidi is a project to give L�VE a midi i/o interface. lovemidi is based on luamidi and the newest rtmidi library.

L�VE/Lua compatibility
======================

* The current version is compatible with L�VE 0.9.1, L�VE 0.8.0, L�VE 0.72 and Lua 5.1.x
* For x86 choose luamidi.dll
* For win64 choose luamidi.dll_64 and rename to luamidi.dll

The current binary is compiled with VS2012 (as L�VE 0.9.1). Other L�VE/Lua could bring different Visual Studios runtimes. If the library does not work, you have to install the Visual Studio 2012 [Runtime](http://www.microsoft.com/en-US/download/details.aspx?id=30679).

Example
=======

Output (send Midi Data to Output-Port 0)
```lua
-- initialize the library
local midi = require "luamidi"

-- count output-ports
print("Midi Output Ports: ", midi.getoutportcount() )

-- play a note on output-port 0 on channel 1
-- port, note, [vel], [channel]
midi.noteOn(0, 60, 50, 1)

-- deinitialize library
midi.gc()
```

Input (receive Midi Data from Input-Port 0)
```lua
-- initialize the library
local midi = require "luamidi"

-- look for available input ports
print("Midi Input Ports: ", midi.getinportcount())

if midi.getinportcount() > 0 then
	table.foreach(midi.enumerateinports(), print)
	print( 'Receiving on device: ', luamidi.getInPortName(0))
	print()

	local a, b, c, d = nil
	while true do
		-- recive midi command from input-port 0
		-- command, note, velocity, delta-time-to-last-event (just ignore)
		a,b,c,d = midi.getMessage(0)
		
		if a ~= nil then
			-- look for an NoteON command
			if a == 144 then
				print('Note turned ON:	', a, b, c, d)
			-- look for an NoteOFF command
			elseif a == 128 then
				print('Note turned OFF:', a, b, c, d)
			end
		end
	end
end

-- deinitialize library
midi.gc()
```

For more advanced examples please look into the tests/ folder.

Installation
============

Just add the right luamidi.dll (for L�VE x86) or luamidi.dll_64 (rename to luamidi.dll) (for L�VE win64) to your project.

TODO
====

Still todo is to update the current makefile to create binaries for linux and macosx.

References
==========

* lovemidi uses [luamidi](https://github.com/dwiel/luamidi) (older rtmidi, less compatibility)
* luamidi used [rtmidi](https://github.com/thestk/rtmidi) for cross platform midi compatibilty Linux/MacOSX/Windows


License
=======
Copyright (c)'2014 Florian Fischer

lovemidi uses the same license as rtMidi and luamidi (MIT)

LEGAL AND ETHICAL:

The RtMidi license is similar to the the MIT License, with the added "feature" that modifications be sent to the developer.

    RtMidi: realtime MIDI i/o C++ classes
    Copyright (c) 2003-2014 Gary P. Scavone

    Permission is hereby granted, free of charge, to any person
    obtaining a copy of this software and associated documentation files
    (the "Software"), to deal in the Software without restriction,
    including without limitation the rights to use, copy, modify, merge,
    publish, distribute, sublicense, and/or sell copies of the Software,
    and to permit persons to whom the Software is furnished to do so,
    subject to the following conditions:

    The above copyright notice and this permission notice shall be
    included in all copies or substantial portions of the Software.

    Any person wishing to distribute modifications to the Software is
    asked to send the modifications to the original developer so that
    they can be incorporated into the canonical version.  This is,
    however, not a binding provision of this license.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
    EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
    MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
    IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR
    ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF
    CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
    WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

# GoBlync

goblync is a Go library for interacting with [BlyncLight](http://www.embrava.com/) devices.  This library is compatible with BlyncLight
v3.

## Building

Based on the dependency on Go [HID](https://github.com/boombuler/hid), the following operating systems are supported targets (as used by $GOOS environment variable)

* darwin (uses native IOKit framework)
* linux (uses libusb 1.0+)
* windows (uses native Windows HID library)

```
go get github.com/boombuler/hid
go build
```

For building on Windows, see the potential quirks listed [here](https://github.com/boombuler/hid).

## Usage

```
package main

import (
	"github.com/davidehringer/goblync"
	"time"
)

func main() {

	light := blync.NewBlyncLight()
	time.Sleep(time.Second * 2)
	//flash all lights in order based on USB path
	light.FlashOrder()

	// set all lights connected to red with music
	light.SetColor(blync.Red,0)
	light.Play(52,0)
	time.Sleep(time.Second * 5)
	light.StopPlay(0)

	//set only first blync light to blue
	light.SetColor(blync.Blue,1)
	time.Sleep(time.Second * 5)

	//reset all blynclights
	light.Reset(0)


	for i := 0; i < 256; i++ {
		light.SetColor([3]byte{byte(i), 255 - byte(i), 0x00},0)
		time.Sleep(13 * time.Millisecond)
	}
	light.SetBlinkRate(blync.BlinkMedium,0)
	time.Sleep(time.Second * 5)
	light.Close() //Close always affects all lights
}

```

### Note on multiple BlyncLights.
Blync lights are ordered based on USB ID.  You can use system info based on your OS tools, or call light.flashOrder().

When calling subsequent blynclight functions use 0 to interact with all, or specific ID starting as 1 based on relative order.
# ps4

A golang library to read from a Playstation 4 controller. This uses evdev, so it is **Linux Only**.

# Usage

```golang
pacakge main

import (
	"context"
	"fmt"

	"github.com/mrasband/ps4"
)

func main() {
	inputs, err := ps4.Discover()
	if err != nil {
		fmt.Printf("Error discovering controller: %s\n", err)
		os.Exit(1)
	}

	var device *ps4.Input
	for _, input := range inputs {
		if input.Type == ps4.Controller {
			device = input
			break
		}
	}

	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	events, _ := ps4.Watch(ctx, device)
	for e := range events {
		fmt.Printf("%+v\n", e)
	}
}
```

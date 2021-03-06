+++
title = "Button"
description = "React to button presses without polling"
picture = "/img/button.jpg"
+++

# Overview

[GPIO pins](/device/gpio/) can be leveraged to read button presses, similar to
[detecting motion](/device/hc-sr501/).

The [gpio.PinIn.WaitForEdge()](https://periph.io/x/periph/conn/gpio#PinIn)
function permits a edge detection without a busy loop.


# Example

~~~go
package main

import (
    "fmt"
    "log"

    "periph.io/x/periph/conn/gpio"
    "periph.io/x/periph/conn/gpio/gpioreg"
    "periph.io/x/periph/host"
)

func main() {
    // Load all the drivers:
    if _, err := host.Init(); err != nil {
        log.Fatal(err)
    }

    // Lookup a pin by its number:
    p, err := gpioreg.ByName("16")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s: %s\n", p, p.Function())

    // Set it as input, with an internal pull down resistor:
    if err = p.In(gpio.Down, gpio.BothEdges); err != nil {
        log.Fatal(err)
    }

    // Wait for edges as detected by the hardware, and print the value read:
    for {
        p.WaitForEdge(-1)
        fmt.Printf("-> %s\n", p.Read())
    }
}
~~~

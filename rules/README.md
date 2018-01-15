# KSS Reference variables

KSS means Kicad Style Sheet. It is very similar to CSS syntax.

The main difference is that selectors can be regular expressions.

## Selectors

Selectors are not case sensible.

### Pin selector

Example :

```CSS
VDD {
    class: "power+";
}
```

[google](google.fr)

All pins named `VDD` or `Vdd` will be putted in the class named `power+`.

|Selector|Example|Example description|
|--------|-------|-------------------|
|name|GND|All pin named GND|
|(name1\|name2)|(GND\|VSS)         |All pin named GND or VSS|
|name[12]      |VDD[AB]            |All pin named VDDA or VDDB|
|name[1-3]     |VDD[A-C]           |All pin named VDDA or VDDB or VDDC|
|name[1-3]*    |VDD[A-C]*          |All pin named VDD or VDDA or VDDB or VDDC|
|\[fb\]\[1-3\]+|\[PR\]\[A-L\][0-9]+|Match all gpio RA1, PF8, ...|

For more examples, take a look to kss files in this directory or look at regular expression reference guide.

Capture parts is possible in selector to reuse value in properties. To capture a part of text, just put it in parentheses. And to reuse it, call it with a backslash  with the id of captured part. Example :

```CSS
\[PR\]([A-L])[0-9]+ {
    class: "gpio\1";
}
```

Letter after P or R is captured and putted in `\1`. So a pin named `RB1` will be classed in class `gpioB` and `RL12` in `gpioL`.

### Class selector

Class selector are similar to pin selector but start by a point.

Example :

```CSS
.gpioA {
    position: left;
    sort: asc;
}
```

All pins classed in `.gpioA` are moved on the left of the final component with pin sorted ascended.

## Properties

At the moment, only a few set of properties are implemented.

### Pin properties

#### class [text, text with capture]

class gives the name of the name of the destinantion class.

If the selector contains capture(s), captured parts could be used to set the destination class.

default: "default"

### Class properties

#### position [top, bottom, left, right, aside]

position gives the position of pins on the final component. aside means left or right, depending of the residual place.

default: aside

#### sort [none, asc, desc]

If sort is different of none, sets the order of pin sorting.

default: none

#### sortpattern [regexp]

The sortpattern regular expression gives the part of pin that will be used to sort pins.

default: ".*" (the whole pin name)
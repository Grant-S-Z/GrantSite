+++
title = "Use plantuml in org"
author = ["Grant"]
lastmod = 2023-11-21T14:39:58+08:00
tags = ["Emacs", "org", "plantuml"]
categories = ["cs"]
draft = false
+++

## What is plantuml? {#what-is-plantuml}

<https://plantuml.com/zh/>


## Plantuml samples in org {#plantuml-samples-in-org}

There are still many other samples waiting for my encounters.


### Sequence Diagram {#sequence-diagram}

```nil
@startuml

scale 2560*1600

title Sequence Diagram

actor User
box "Multichannel Spectrometer Program" #LightBlue
participant miniMCA
participant Usb
participant ThdbDA
participant ThdbAD
participant PulseHeight
participant Spectrum
participant Ram

== Before User ==

ThdbDA --> ThdbAD: Analog Signal DA_DA (AD_DA)
ThdbAD --> PulseHeight: Digital Signal ad_data
PulseHeight --> Spectrum: pulse_height and pulse_indicator

== User Start ==

User -> miniMCA:
miniMCA -> Usb: Start (usb_read_data)
Usb -> Spectrum: cmd_start
Spectrum -> Ram: address_a and data_a
Ram -> Usb: address_b and q_b
Usb -> miniMCA: usb_write_data
miniMCA -> User:

@enduml
```


### Module Diagram {#module-diagram}

```nil
@startuml

scale 2560*1600

title Module Diagram

node final01
node ThdbDA
node ThdbAD
node PulseHeight
node Spectrum
node Usb
node isp1362
node hex
node ram
node pll

final01 -- ThdbDA
final01 -- ThdbAD
final01 -- PulseHeight
final01 -- Spectrum
final01 -- Usb
final01 -- isp1362
final01 -- hex
final01 -- ram
final01 -- pll

@enduml
```

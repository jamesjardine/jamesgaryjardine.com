---
title: "Danger of USB Devices"
date: "2014-08-01T16:45:14"
slug: "danger-of-usb-devices"
categories: ["Uncategorized"]
tags: ["awareness", "rubber duck", "Security", "security awareness", "teensy", "thumb drive", "usb", "user"]
---

There has been lots of news recently regarding a flaw in USB devices that could lead to an unwanted attack. USB stands for Universal Serial Bus and is often associated with thumb drives used to store data. There are many other uses for USB devices, such as your mouse and keyboard. There are even monitors that connect via USB.

Each USB device has a microchip that contains instructions on how it will work. In addition, USB devices are separated into different classes; for example Human Input Devices (HID) for keyboards and mass storage devices like thumb drives. The computer looks at this classification to determine how to handle the device.

From a visual perspective, it is not possible to determine how a USB device will be treated because it is the firmware on the device that contains this information. A good example we have used for years is the teensy device, or rubber ducky. This device looks like a regular thumb drive, but when plugged in, acts like a HID (keyboard).

The concern here is that in an enterprise, there are often controls that block mass storage devices to load onto the end user computers. There are multiple reasons to block these devices. First, the company is trying to stop a malicious user from stealing a bunch of data using a tiny thumb drive. Remember the movie “The Recruit” where they stole info using a thumb drive? Another reason these devices are blocked is that they could contain malicious software ( malware). Block the device, block the malware.

The concern we often see with these controls is that it is difficult to block the human input device because we need a keyboard and a mouse. The idea of the rubber duck is that you, the attacker, can program a sequence of keystrokes onto the device and when it is plugged in it will execute the keystrokes. This technique allows attackers to bypass many controls and possibly gain unauthorized access to the system.

One can disassemble a rubber duck and see the sd card to see it is not a real thumb drive. The issue with this recent news is that regular thumb drives could be recoded to work as a HID without your knowledge. We haven’t seen many details of how to pull this off remotely, however we should be cautious anytime we plug a device into our system.

I will not stop using USB devices anytime soon. Diligence and user awareness are important here.

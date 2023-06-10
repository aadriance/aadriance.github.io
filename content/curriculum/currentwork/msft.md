+++
title = "Microsoft"
date = 2020-11-11
description = "Windows Kernel Developer on the Windows Hardware Error Architecture (WHEA)."
weight = 0

[extra]
heroimage = "/images/cv/MSLogo.png"
+++

## Notable Technical Projects:
- Implementation of Downstream Port Containment in the Windows PCI driver.
- Extension of Downstream Port Containment support for E1.s drive surprise removal.
- Windows support for AMD's MCAx (Extended Machine Check Architecture).
- Porting of platform specific hardware reliability features from firmware code into kernel code.
- Porting of Windows hardware reliability features for Arm platforms.
- Documentation champion, running local "documentation days" to improve documentation culture.

WHEA? Yes! We even have our own [MSDN pages](https://docs.microsoft.com/en-us/windows-hardware/drivers/whea/). WHEA owns [BSOD code 0x124](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/bug-check-0x124---whea-uncorrectable-error). If you see a BSOD 0x124 it means your hardware has slightly lost its mind and the Windows kernel is making a controlled emergency landing.  Sadly you won't be reaching your destination this boot session, but all your files will get to ride those fine inflatable slides off the side of the plane and return home safely for when you next boot your machine. We don't always crash your machine, we also implement recovery mechanisms to keep your machine alive.

Usually things are not the fault of hardware, but [ECC](https://en.wikipedia.org/wiki/ECC_memory) exists for a reason.  When hardware does flip bits, it's preferable to contain the damage and bring the system down in a controlled fashion to prevent executing random instructions. Additionally, when considering cloud scale, a one-in-a-million event is a daily occurrence. You can even use memory errors to attack machines, and try to induce them using hair dryers! I'm serious, this [paper](https://www.cs.princeton.edu/~appel/papers/memerr.pdf) by Sudhakar Govindavajhala titled "Using Memory Errors to Attack a Virtual Machine" features a figure with a heat lamp attached to a desktop to induce memory errors.

I love working on WHEA.  It gives me a a huge range of things to work on.  I've written PCI driver code, assembly code, implemented support for ACPI features, and much more.  WHEA covers a wide range of work in the kernel components of Windows.
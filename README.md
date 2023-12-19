# Management Client for NEC M-Series and ME-Series video displays

The NEC M-Series and ME-Series video displays support remote management either
over a TCP/IP connection or over a serial port. This is an async Python library
implementing that protocol, currently only over TCP/IP.

In particular, this library should support the following models:

- NEC M431
- NEC M491
- NEC M551
- NEC M651
- NEC ME431
- NEC ME501
- NEC ME551
- NEC ME651

It might support other models if they follow the same protocol, but this
library was developed based on documentation focused only on those models.

This library has been tested primarily against an ME551, initially running
firmware R2.908 and then later running firmware R4.005. The author has no other
hardware to test on and so is assuming others are compatible with
[the protocol documentation supplied by NEC](https://assets.sharpnecdisplays.us/documents/usermanuals/external_control_m_me_series.pdf).

## Command Line Tool

This package contains a simple command line tool that's mainly just an example
of how to perform some of the supported operations, but might also be of some
real use.

The tool returns its own usage information if run without any arguments:

```
python -m necme
```

For example, to retrieve the model number, serial number, and current firmware
version of a monitor accessible from a controller at 192.0.2.1:

```
python -m necme 192.0.2.0 info
```

## Library

The main purpose of this package is to provide a library for controlling a
display from other software.

The typical way to use it is to first instantiate a controller object by
connecting to the display in question:

```python
ctrl = await Controller.connect_tcpip('192.0.2.1')
```

If connecting succeeds, and if you know that the given IP address refers to
the controller of only a single display (which is typical when using TCP/IP),
you can then probe for the id of the connected monitor and obtain a
communication channel for that monitor id:

```python
monitor = await ctrl.probe_open_one_monitor()
```

You can then perform operations against that monitor. For example:

```python
serial_no = await monitor.read_serial_no()
```

## License

Copyright (c) 2023 Martin Atkins

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

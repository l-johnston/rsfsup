![PyPI](https://img.shields.io/pypi/v/rsfsup?style=plastic)
# `rsfsup`
Python interface to the Rohde & Schwarz FSUP Signal Source Analyzer

## Installation
```linux
$ pip install rsfsup
```  

## Usage
It is possible to configure the instrument and read the trace data with this library.
The first example shows reading the spectrum analyzer trace in a context manager.

```python
>>> from rsfsup import CommChannel
>>> with CommChannel("<ip address>") as fsup:
...     data = fsup.spectrum.read()
>>> import matplotlib.pyplot as plt
>>> plt.plot(*data)
[<matplotlib.lines.Line2D at ...>]
>>> plt.show()
```  

The next example shows switching to SSA mode to measure phase noise. This one also shows
using the CommChannel directly, which is useful in interactive sessions where features
of the instrument can be accessed using tab completion.

```python
>>> from rsfsup import CommChannel
>>> cc = CommChannel("<ip address>")
>>> fsup = cc.get_instrument()
>>> fsup.mode = "SSA"
>>> data = fsup.ssa.read()
>>> import matplotlib.pyplot as plt
>>> plt.semilogx(*data)
>>> plt.show()
>>> cc.close()
```

It is possible to change the resolution bandwith and video bandwidth.

```python
In [1]: from rsfsup import CommChannel
In [2]: cc = CommChannel("<ip address>")
In [3]: fsup = cc.get_instrument()
In [4]: fsup.spectrum.bandwidth.resolution_bandwidth
Out[4]: '50.0 kHz'
In [5]: fsup.spectrum.bandwidth.video_bandwidth
Out[5]: '50.0 kHz'
In [6]: fsup.spectrum.bandwidth.rbw_vbw_ratio
Out[6]: 1.0
In [7]: fsup.spectrum.bandwidth.rbw_vbw_ratio = 0.1
In [8]: fsup.spectrum.bandwidth.rbw_vbw_ratio
Out[8]: 0.1
In [9]: fsup.spectrum.bandwidth.resolution_bandwidth
Out[9]: '50.0 kHz'
In [10]: fsup.spectrum.bandwidth.video_bandwidth
Out[10]: '500.0 kHz'
In [11]: fsup.spectrum.bandwidth.video_bandwidth = "3 MHz"
In [12]: fsup.spectrum.bandwidth.video_bandwidth
Out[12]: '3.0 MHz'
```

Supported features:
- Spectrum analyzer
    - Configuration
    - Markers
    - Trigger
    - Read trace, frequency and time domain
- Phase noise (PLL Cross correlation)
- File system management

## Documentation
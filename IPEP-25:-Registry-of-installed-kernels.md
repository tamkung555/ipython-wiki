<table>
<tr><td> Status </td><td> Active </td></tr>
<tr><td> Author </td><td> Thomas Kluyver &lt;thomas@kluyver.me.uk&gt;</td></tr>
<tr><td> Created </td><td> April 8, 2014</td></tr>
<tr><td> Updated </td><td> April 9, 2014</td></tr>
<tr><td> Discussion </td><td>  </td></tr>
<tr><td> Implementation </td><td>  </td></tr>
</table>

#Abstract

IPython's messaging protocol, originally designed to allow multiple frontends, has proven invaluable for supporting kernels in different languages, such as [Julia](https://github.com/JuliaLang/IJulia.jl), [Haskell](https://github.com/gibiansky/IHaskell) and [R](https://github.com/takluyver/IRkernel/), and we are rapidly adapting the protocol to be more language agnostic. However, using an alternative kernel requires a long, awkward command line argument, and you must start one notebook server for each kernel you wish to use.

This IPEP describes a way for available kernels to advertise themselves to IPython through a registry, and the changes to IPython's API to allow querying the registry and launching kernels. It does not cover the UI which will allow users to start different kernels.

#Registry locations

The registry lives in two directories on the filesystem: a per-user directory and a systemwide directory. If both contain a kernel with the same name, the user directory has priority.

The per-user directory is located in `$IPYTHONSTARTUP/kernels/` (typically `~/.ipython/kernels/`)

The systemwide directory is located in:
* Unix systems: `/usr/share/ipython/kernels`
* Windows systems: ???

#Kernel directories

Each kernel in the registry is represented by a directory in one of the locations described above. The name of this directory is the kernel's name, and is used in the API to list and select the kernel.

A number of files should be present in the directory:

* kernel.json: contains important information about the kernel; described below.
* icons: TBD
* custom.js, custom.css (optional): JS and CSS to tweak the interactive notebook UI. These are provided so kernels can work around limitations in the UI, but they should be used no more than necessary, and be treated as a stopgap measure while the core UI is improved to better accommodate different languages.
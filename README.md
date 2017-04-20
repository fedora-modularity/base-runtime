# Base Runtime
The base application runtime and hardware abstraction layer.

## Scope
A project closely linked to the Modularity Initiative, Base Runtime
is about the defining the common shared package and feature set of the
operating system, providing both the hardware enablement layer and the
minimal application runtime environment other modules can build upon.
Besides the Base Runtime module itself, the project also focuses on
delivery of additional low-level modular content, such as distribution
management and configuration utilities, basic system services or
infrastructure build environments.

With the Modularity infrastructure gradually becoming available, it is
time to focus on the modular content as well. The goal of the Base Runtime
project is to define the minimal shared core of the operating system
that would light up the hardware (where applicable) and provide runtime
environment for other, more complex and feature-rich modules.  The idea
is to deliver this functionality as a set of modules or a module stack.

Besides the Base Runtime module itself, the team is responsible for a
number of additional low level modules that define the platform, such
as software management and configuration tools or buildroots.

## Modules
The team currently focuses on development of two separate modules.

* `base-runtime`, which provides a fairly minimal set of runtime environment packages, suitable for creation of container base images as well as bootable bare metal installations for all supported architectures; the module also provides the minimal buildroot, although that might change in the future
* `bootstrap`, which provides a self-hosting build environment for the `base-runtime` module; a special-purpose, unsupported module, it shouldn't be used by anyone but the Base Runtime team

## API
This section documents all the source and binary packages included in
the Base Runtime along with brief reasoning why the particular component
was included or excluded.

The component list is based on the output of
[whatpkgs](https://github.com/fedora-modularity/whatpkgs) ran against
Fedora 26 dist-git branch point metadata.  The input for teh depsolver
can be found in the `toplevel-binary-packages.txt` file.

Decisions whether a particular subpackage should be included or not is
based on a small set of basic rules:

* if the package is in the toplevel set, it's included, obviously
* if the package is in the runtime dependency chain of a toplevel package, it's also included, also obviously
* if the package provides development environment for another package provided by the module, it should be included
* if including the package doesn't require any additional components to be added to the module, it should be included; avoid package splits and filtering where possible, even at the cost of adding new components to the set, provided the new components are small, stable, and have minimal dependency chains; use common sense
* no interpreted languages bindings unless necessary (Python 3 bindings are tolerable as the interpreter is part of the set)

Also keep in mind that having a package available in the Base Runtime set
doesn't mean it has to be installed on the system or be part of the Base
Runtime-based base container image.  It means the packages are available
in the repository, supported and can be installed the the user wishes
to do so.

See the `api.*` file in this repository for per-architecture APIs or the
modulemd files for the resulting merged API and filters.  This document will be
updated with a generated, human-readable summary as well.  Stay tuned.

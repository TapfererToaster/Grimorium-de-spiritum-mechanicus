# Abstraction
VMs use [[Virtualization#Abstraction|abstraction]] to be independent of the hardware.
# Physical vs. Virtual Machines
Virtual Machines are the *virtualization* of computer resources like CPU and memory, meaning they are software emulations of the physical hardware.
Historically you needed a dedicated physical machine with an dedicated OS for every application you needed, meaning you needed a individual machines to run an Email server, database, file server, etc.

With VMs you can run multiple applications on one physical machine, called the [[Virtualization#Hypervisors|hypervisor]]. Each VM shares the underlying hardware resources, so the more powerful the hardware the more VMs you can create


# Anisa-LPIC-1-Jan-2023-Fridays-HW6

# MohammadALi Heidari-LPIC1-Fridays-HW6

## Task 1: 
I searched a lot and couldn't find anything that I think it's correct but the closest thing I found to the wanted concept is `swap tendency` 

It is calculated by the kernel itself and its formula is `swap_tendency = mapped_ratio/2 + distress + vm_swappiness`

The `distress` value is a measure of how much trouble the kernel is having freeing memory. The first time the kernel decides it needs to start reclaiming pages, distress will be zero. If more attempts are required, that value goes up, approaching a max value of 100.

The `mapped_ratio` value is an approximate percentage of how much of the system's total memory is mapped within a given memory zone.

## Task 2: Meaning of numbers and characters before each file in `/etc/rc`

The `/etc/rcN.d` scripts are always run in ASCII sort order. The scripts have names of the form:

`[KS][0-9][0-9]*`

Files that begin with `K` are run to terminate (kill) a system service. Files that begin with `S` are run to start a system service.

The numbers specify the order in which the scripts should be executed because they run in ASCII sort order.

## Task 3: unit types; snapshot? scope? slice?

`Snapshot` units are similar to target units in that they do not actually do anything themselves and their only purpose is to reference other units. Snapshots can be used to save or rollback the state of all services and units of the system.

systemd snapshots have two intended use cases: To allow a user to temporarily enter a specific state such as “Emergency Shell”, terminating current services, and provide an easy way to return to the state before, pulling up all services again that got temporarily pulled down.

A `slice` unit is a concept for hierarchically managing resources of a group of processes. This management is performed by creating a node in the Linux Control Group (cgroup) tree. Units that manage processes (primarily scope and service units) may be assigned to a specific slice. For each slice, certain resource limits may be set that apply to all processes of all units contained in that slice. Slices are organized hierarchically in a tree. The name of the slice encodes the location in the tree. The name consists of a dash-separated series of names, which describes the path to the slice from the root slice. The root slice is named -.slice. Example: foo-bar.slice is a slice that is located within foo.slice, which in turn is located in the root slice -.slice.

A group of hierarchically organized units. Slices do not contain processes, they organize a hierarchy in which scopes and services are placed. The actual processes are contained in scopes or in services. In this hierarchical tree, every name of a slice unit corresponds to the path to a location in the hierarchy. The dash `-` character acts as a separator of the path components. For example, if the name of a slice looks as follows:
`parent-name.slice`
it means that a slice called `parent-name.slice` is a subslice of the `parent.slice`. This slice can have its own subslice named `parent-name-name2.slice`, and so on.

A group of externally created processes. `Scopes` encapsulate processes that are started and stopped by arbitrary processes through the `fork()` function and then registered by systemd at runtime. For instance, user sessions, containers, and virtual machines are treated as scopes.

Scope units are not configured via unit configuration files, but are only created programmatically using the bus interfaces of systemd. They are named similar to filenames. A unit whose name ends in `.scope` refers to a scope unit. Scopes units manage a set of system processes. Unlike service units, scope units manage externally created processes, and do not fork off processes on its own.

The main purpose of scope units is grouping worker processes of a system service for organization and for managing resources.

## Task 4: Displaying state of being active or inactive of system units by 0 & 1:

`systemctl is-active UNIT_NAME | sed 's/\bactive\b/1/ ; s/inactive/0/'`

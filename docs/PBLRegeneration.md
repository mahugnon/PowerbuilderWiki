# :information_desk_person: :poop: PBL regeneration with Powerbuilder Orcascr170

Powerbuilder binary files **.pbl**  represents  Powerbuilder libraries and can only be read by the Powerbuilder IDE.
For a  versioned Powerbuilder project, only sources code are up to date(file extension **.sru,srf,srd,srw .etc.**), but **pbl** are not. As consequence, when you run  just cloned or checkedout project, an  outdated version is run.

One need to refresh the project to update library files **.pbl**. This can be done with the Powerbuilder IDE or from the command-line using **Orcascript** command-line tool provided by Powerbuilder.

Here I present a way to refresh **pbl** using command-line. This can be used as a post-commit script or simply CI build step( a step before unit tests running).
Regenerating binary files **pbl**  for version controlling in Powerbuilder is not obvious and quite tricky.
The first thing to know is that when you are versioning a Powerbuilder project, the IDE PB17 creates a folder named ws_objects where it puts all the source grouped in a directory corresponding to the library that the source belongs to.
For each of these library folders, a file  **libraryName.pbg**. A  **libraryName.pbg** contains the list of Powerbuilder objects (**classes** in java world) that a library contains.
By default **libraryName.pbg** files are saved with **utf-8 bom** encoding.  
For the PBL regenerating to work,

- :white_check_mark: you need  to first copy **.pbg** files  from **ws_objects\libraryName** to **ws_objects**;

- :white_check_mark: override these  **libraryName.pbg** files by correcting source files path. Because by default, it suppose that pbg files and sources files are in the same directory;

- :white_check_mark: you need to change **libraryName.pbg** file encoding from **utf-8 bom** to **utf-8**.
Finally, you should edit the following script and run it

```Powerbuilder
start session
scc debug true
scc set connect property logfile "path_to_log_file.log" 
scc connect offline
scc set target "path_to_target_file.pbt" importonly
scc refresh target 3pass
scc close
end session
```

Powerbuilder 19 R3 is suppose to fix all these issues. Find the officiel description [:point_right: here](https://docs.appeon.com/pb2019r3/whats_new/Source_control_enhancements.html)

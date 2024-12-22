Cockpitdecks discover about its computer host and its extensions through a global configuration file called its environment.

The environment consists of a few attributes that tells Cockpitdecks about things it cannot discover on its own.

1. Simulator type and location
2. Network communication hosts and ports
3. Extensions to Cockpitdecks

Most of the above information can be communicated to Cockpitdecks either through operating system environment variables or in the global configuration file.

Recall that Cockpitdecks can run either on the same host as the simulation software, or on a separate host connected through the local area network to the host running the simulator.

# Simulator Type and Location

If the simulation software is located on the same host, the `XP_HOME` environment variable is the operating system folder path to the home directory of the software.

If the simulation solftware runs on another computer, XP_HOME does not need to be set or must be set to None.

# Network Communication

# Cockpitdecks Extensions

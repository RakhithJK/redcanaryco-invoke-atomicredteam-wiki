Invoke-AtomicRedTeam is a PowerShell module to execute tests as defined in the [atomics folder](https://github.com/redcanaryco/atomic-red-team/tree/master/atomics) of Red Canary's Atomic Red Team project. The "atomics folder" contains a folder for each Technique defined in the [MITRE ATT&CK™ Framework](https://attack.mitre.org/matrices/enterprise/). Inside of each of these "T#" folders you'll find a **yaml** file that defines the attack procedures for each atomic test as well as an easier to read markdown (**md**) version of the same data.

* Executing atomic tests may leave your system in an undesirable state. You are responsible for understanding what a test does before executing.

* Ensure you have permission to test before you begin.

* It is recommended to set up a test machine for atomic test execution that is similar to the build in your environment. Be sure you have your collection/EDR solution in place, and that the endpoint is checking in and active.

Invoke-AtomicRedTeam installation and use instructions can be found on the index to the right (in the sidebar).
You can also find an in-depth 2 hour webcast [here](https://www.youtube.com/watch?v=d_E-hfKQ5Hw) with 11 hands-on labs [here](https://docs.google.com/document/d/1c8_WRHp68Py9kyMYqMrs6aQ6ppcfLouV8jQ07UY27yE/edit).

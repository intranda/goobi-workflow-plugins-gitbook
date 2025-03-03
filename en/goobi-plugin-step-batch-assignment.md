---
description: >-
  Step plugin for assigning the process to an existing or new batch
---

# Assign batch

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_batch_assignment
Repository               | [https://github.com/intranda/goobi-plugin-step-batch-assignment](https://github.com/intranda/goobi-plugin-step-batch-assignment)
Licence              | GPL 2.0 or newer 
Last change    | 25.02.2025 11:03:03


## Introduction
This documentation explains the plugin for assigning a single process to a batch. This assignment is made directly from an accepted task. A new batch can either be created there or selected from a list of existing waiting batches. 

## Installation
To be able to use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-ZZZ-base.jar
/opt/digiverso/goobi/plugins/GUI/plugin-step-ZZZ-gui.jar
/opt/digiverso/goobi/config/plugin_intranda_step_ZZZ.xml
```

Once the plugin has been installed, it can be selected within the workflow for the respective workflow steps. Please note that two workflow steps must be scheduled in the workflow:

- One workflow step is used by the user as the step in which the batch assignment takes place. 
- Another workflow step serves as a kind of ‘waiting zone’ in which all processes already assigned to a batch remain and only switch to the subsequent step when the batch is complete. 
 
A workflow could therefore look like the following example:

![Example of a workflow structure](images/goobi-plugin-step-batch-assignment_screen1_en.png)

To use the plugin, it must be selected in the first of the two steps:

![Configuration of the workflow for using the plugin](images/goobi-plugin-step-batch-assignment_screen2_en.png)

## Overview and functionality
Once the user has accepted the task, they can first decide in the plugin whether a new batch should be created or whether a selection should be made from the existing batches that are still waiting. If the user wants to define a new batch, they can define the title for the batch here and, if required, also enter properties that were defined via the configuration:

![Create a new batch with defined properties for the process](images/goobi-plugin-step-batch-assignment_screen3_en.png)

Alternatively, the user can select a batch from the list of currently waiting batches. Once the desired batch has been selected, the task can be completed as normal.

![Selection from the list of waiting batches](images/goobi-plugin-step-batch-assignment_screen4_en.png)

After assignment to an existing or newly created batch, the workflow for the process moves on to the subsequent workflow step, which can be regarded as a kind of ‘waiting zone’. All processes in a batch initially remain there and do not yet pass through the subsequent steps. 

If the user decides in the workflow step of a process that the batch with this process is now complete, he can click on the button for ‘Close batch’. This opens a dialogue window in which a batch docket can be downloaded and the batch can be closed:

![Closing a batch](images/goobi-plugin-step-batch-assignment_screen5_en.png)

By closing the batch, the workflow step for waiting for all processes in the batch to be complete is finished and the workflow step of the currently open workflow step is also finished. This means that all processes assigned to a batch simultaneously switch to the next subsequent workflow step so that they can be processed further together.

![Further progress of the workflow](images/goobi-plugin-step-batch-assignment_screen6_en.png)

## Configuration
The plugin is configured in the file `plugin_intranda_step_batch_assignment.xml` as shown here:

```xml
<config_plugin>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
	-->
    
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        
        <!-- batch is complete step in the workflow that shall be finished once the last process is added to the batch -->
        <batchWaitStep>Waiting for batch completion</batchWaitStep>

        <!-- properties to be editable for new batches -->
        <property>Scanner</property>
        <property>Opening angle</property>

    </config>

</config_plugin>

```

### General parameters 
The `<config>` block can occur repeatedly for different projects or work steps in order to be able to perform different actions within different workflows. The other parameters within this configuration file have the following meanings: 

| Parameter | Explanation | 
| :-------- | :---------- | 
| `project` | This parameter defines which project the current block `<config>` should apply to. The name of the project is used here. This parameter can occur several times per `<config>` block. | 
| `step` | This parameter controls which work steps the `<config>` block should apply to. The name of the work step is used here. This parameter can occur several times per `<config>` block. | 


### Further parameters 
In addition to these general parameters, the following parameters are available for further configuration: 


Parameter               | Explanation
------------------------|------------------------------------
`batchWaitStep`         | Name of the workflow step in which the processes are to remain until the last process is added to the batch
`property`              | Names of those properties of the process that are to be editable when the batch is created and that are to be adopted for all associated processes
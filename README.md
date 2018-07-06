

<b>Ignition SDK Installation and Setup</b>
===================================

<code>Quick Start With ArcheTypes</code>
---------------------------

*In a command prompt type the following command:*

    mvn archetype:generate

--------

You will be presented with an option "Choose a number or apply filter". </br>

<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px;">
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains):
</kbd></br></br>

*Type the following filter:*

    client-designer-gateway-archetype

---------

You will be presented the option.

<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">*1: remote -> com.inductiveautomation.ignitionsdk:client-designer-gateway-archetype*</kbd></br></br>

Simply choose an option:</br>
*The only current option:*

    1

---------

You will then be asked to choose a version.

<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">
Choose com.inductiveautomation.ignitionsdk:client-designer-gateway-archetype version:
</kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">1: 1.0.1 </kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">2: 1.0.2 </kdb></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">3: 1.0.3 </kbd></br></br>


*Simply pick the version you want ( Most likely option 3, the current latest version )*

    3

--------

After these options you will be required to define the module you are planning to create. </br>

This example will create the module *com.companyname.category.testing* version 1.0.0. </br>
*Note: I chose to opt-out of doing SNAPSHOT's*</br>


<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">Define value for property 'groupId': <bold style="color: gray;">com.companyname.category</bold></kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">Define value for property 'artifactId': <bold style="color: gray;">testing</bold></kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">Define value for property 'version' 1.0-SNAPSHOT: : <bold style="color: gray;">1.0.0</bold></kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">Define value for property 'package' com.steritec.modules: : <bold style="color: gray;"><-- Left Blank</bold></kbd></br></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">Confirm properties configuration:</kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">groupId: com.steritec.modules</kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">artifactId: testing</kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">version: 1.0.0</kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">package: com.steritec.modules</kbd></br>
<kbd style="background-color: #000000; color: white; font-style: bold; font-size: 12px">Y: <bold style="color: gray;">Y</bold></kbd></br>
</br></br>

-------

After confirming the configuration setup you should have the correct structure to get started.

Change directory to the "ArtifactID"</br>
*Note: The ArtifactID from the example above was named "testing"*

    cd testing

------

<b>JAVA DOCS</b>
=====
<code>Setup Java Docs</code>
-------------------------------

The archetype generated from the instructions above will produce a POM.xml that references older repository URLs.</br>
I will attempt to walk you through this fix now.


    In the root directory of your module, open the "pom.xml" file.

Change all URL's in the "pluginRepositories" and "repositories" sections:

    FROM:
        http://nexus.inductiveautomation.com:8081/content/repositories/<repo name>
    TO:
        https://nexus.inductiveautomation.com/repository/<repo name>

Example:

```XML
FROM:
    <url>http://nexus.inductiveautomation.com:8081/content/repositories/inductiveautomation-releases</url>

TO:
    <url>https://nexus.inductiveautomation.com/repository/inductiveautomation-releases</url>
```

Save the file and finally, resolve java docs via maven:

    mvn dependency:resolve -Dclassifier=javadoc

--------------------
<b>MODULE INSTALLING</b>
==================
<code>Setup Module Installing</code>
------------------------------------

In order to *install your module quickly* and *without signing* you need to do the following:

    Open Start Menu -> type: notepad -> right click it and 'Run as administrator'

Opening this way because the Gateway may be running and won't let you save changes otherwise.

    Inside Notepad, Click File -> Open and browse to the Ignition "data" folder

Possible directory location:

    C:\Program Files\Inductive Automation\Ignition\data\ignition.config

*Note: If you do not see the file 'ignition.config' </br>
Try changing the file extension filter from "Text Documents (*.txt)" to "All Files (*.*)"

    Find and Select -> ignition.config -> Click "Open"

Find the section marked:

    #********************************************************************
    # Wrapper Java Properties
    #********************************************************************

Find the subsection marked:

    # Java Additional Parameters

Add the following two lines to the bottom of that section:

    wrapper.java.additional.7=-Dignition.allowunsignedmodules=true
    wrapper.java.additional.11=-Dia.developer.moduleupload=true

Finally save the file:

    File -> Save

This will allow you to install your package to the gateway via the following command:</br>

    mvn install

Some Extra Notes: </br>
*- This will both package and install your module.* </br>
*- If you're writing a gateway module, you may need to restart gateway.* </br>
*- You can do this through the "Gateway Control Utility".*


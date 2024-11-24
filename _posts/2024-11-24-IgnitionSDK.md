---
title: Ignition SDK
description: Ignition's Software Development Kit setup guide and resources.
author: Fran_Dujmovic
date: 2024-11-24 16:45:00 +0200
categories: [Ignition SDK Setup]
---
# Environment Setup

## Prerequisites

### Java JDK

You will need a **Java JDK** installed. Be mindful of which version you install:

  - Ignition platform 7 requires a Java 1.8 JDK
  - Ignition platform 8 requires a Java 11 JDK

You have several options for JDKs. Popular options are:
  - [Azul JDK Downloads](https://www.azul.com/downloads/?package=jdk#zulu)
  - [Java JDK Downloads](https://www.oracle.com/java/technologies/downloads/?er=221886)

### Build Syste

You will need to choose and install a build system. Recommended build systems include **Gradle** and **Maven**.

1. **Gradle**
   1. [Download](https://gradle.org/releases/) the latest Gradle distribution.
   2. Create a new directory at **`**C:\Gradle**
   3. Extract your downloaded Gradle build and place **gradle-8-11** file into **C:\Gradle**
   4. Add a system variable for Gradle **This PC > Properties > Advanced System Settings > Environment Variables > Path > Edit** and add **C:\Gradle\gradle-8.1.1\bin** to the **Path** variable.

2. **Maven**

Windows users can install via [Chocolatey](https://chocolatey.org/) (**choco install maven**) or by downloading the installer at the [Maven downloads page](https://maven.apache.org/download.cgi).

# Create a Module

Whether you use Gradle or Maven, you can utilize the provided build tools to generate a new project with a framework that includes the basic structure necessary for your module.

## Pull Tools Repository

The tools repository you choose will depend on your build system. Before pulling a repository, make sure you have [Git](/posts/GitBash/) installed.

**Gradle**
```bash
git clone https://github.com/inductiveautomation/ignition-module-tools
```

**Maven**
```bash
git clone https://github.com/inductiveautomation/ignition-sdk-archetypes
```
Once you have the module tools in hand you can generate a new project through the command line.

## Create a New Project

1. Open the directory containing **Ignition Module Tools**.
2. Navigate one level down into **generator**
3. Open a command prompt in this directory and run the following:
   ```bash
gradlew.bat clean build
```
4. Create a new project:
  ```bash
gradlew.bat  runCli -–console plain
  ```
5. Fill in the following information when prompted:

| **Prompt**          | **Description**                                                                                                                      | **Example**                                    |
| :------------------ | :----------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------- |
| Enter scopes        | The scopes your module will require, including Gateway (G), Client (C), and Designer (D). Note that these values are case sensitive. | GCD                                            |
| Human readable name | A name for your new project.                                                                                                         | New SDK Project                                |
| Root package        | A reverse domain name specific to your organization and project.                                                                     | com.inductiveautomation.ignition.newsdkproject |
| Language            | Language for gradle buildscripts. Possible values are kotlin and groovy. Default: kotlin.                                            | kotlin                                         |

This will create a new project structure for you with seperate directories for each of the **Gateway, Designer and Client scopes**, as well as a **Common and Build directory**. If you receive a **BUILD SUCCESSFUL** message, you can close the command prompt and open your new project in your preferred IDE:

![Desktop View](https://i.ibb.co/tc3NWbR/Intelli-J-Generator-CLI.png){: width="972" height="589" }
_IntelliJ Gradle Compiled Tools Project_

# Build a Module

An Ignition Module consists of an **xml manifest, jar files, and additional resources and meta-information**. When you create a module from an existing **gradlew.bat or Maven Archetype**, your project will have a build directory with the basic tools necessary to compile and build a **.modl file**. Depending on your build system, you may want to further edit the files in your build directory to declare dependencies, update signing settings, or configure tasks. See the Plugins section for configuration settings specific to your build system.

## Compile and Build

When you are ready to build your .modl file, open a command prompt in your project's root directory and run the following command:

**For Gradle:**
```bash
gradlew.bat clean build
```

**For Maven:**
```bash
mvn package
```

You now have a **.modl file** that is ready to install on an **Ignition Gateway**.


# Sign a Module

An Ignition Module must be **signed** before it can be installed on a system that is not in developer mode. Modules can be signed using either a real code signing certificate obtained from a Certificate Authority or using a self-generated and self-signed certificate.

## Getting Started

What you will need:
  - Code signing certificate
  - The full certificate chain, in the correct order, in p7b (PKCS7) format.
  - The [IA module signing tool](https://github.com/inductiveautomation/module-signer).

## Signing your Module

Your certificate and its private key should be stored inside a Java keystore. You'll need this keystore file and the associated certificate chain to run the module signing tool. The certificate will be stored under an alias and be password protected.

Run the signing tool providing the following parameters:

```
java -jar module-signer.jar \ 
    -keystore=<path-to-my-keystore>/keystore.jks \
    -keystore-pwd=<password> \
    -alias=<alias>\
    -alias-pwd=<password> \
    -chain=<pathToMyp7b>/cert.p7b \
    -module-in=<path-to-my-module>/my-unsigned-module.modl \
    -module-out=<path-to-my-module>/my-signed-module.modl
```

Alternatively, you can allow the implementation of **unsigned modules** into Ignition Gateway by editing the following file: 

1. Navigate to **C:\Program Files\Inductive Automation\Ignition\data\ignition.conf**
2. Open the **ignition.conf** file in a text editor (Notepad++ for example)
3. At the end of **Java Additional Parameters**, add the following line:
   ```java
wrapper.java.additional.8=-Dignition.allowunsignedmodules=true
   ```
Ensure that the number is standalone, meaning it doesn't match to or conflict with other existing commands. See example below:

![Desktop View](https://i.ibb.co/3SZ53xy/Java-Additional-Parameters.png){: width="972" height="589" }
_Java Additional Parameters Config section_

# Install a Module

Installing a module you built with the Ignition SDK works the same way as installing any other third-party module.

1. On your Gateway, navigate to **Config > System > Modules**.
2. Click **Install or Upgrade a Module**.
3. Click **Choose File** and select the **.modl file** you want to install.
4. Click **Install**.

You can now launch the **Designer** and use your new module.


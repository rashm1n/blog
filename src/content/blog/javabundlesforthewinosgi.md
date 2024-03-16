---
author: Rashmin Mudunkotuwa
pubDatetime: 2024-03-16T04:42:31Z
modDatetime: 2024-03-16T07:42:31Z
title: Java Bundles FOR THE WIN. Introductions to OSGI
slug: java-bundles-introduction-to-osgi
featured: true
draft: false
image: ../../assets/images/osgipic.jpg
ogImage: ../../assets/images/osgipic.jpg
tags:
  - java
  - osgi
  - monolith
description: Introduction to JAVA OSGI
---

![something](@assets/images/osgipic.jpg)

If you are new to the Java ecosystem, you probably would not have heard of OSGI. But fear not, in this article I will give you a solid understanding about the basics of OSGI and its use cases.

OSGI, short for Open Service Gateway Initiative is a standard for modular and dynamic application development in Java. So here, what does it mean by modular and dynamic ? Modular means “consisting of different parts” and dynamic means “prone to constant change, activity”, so as a whole,we can say that it is a framework which consists of constantly changing and active, different small components.

OSGI standard defines a specification to separate a single Java application to smaller components called “bundles” which can be separately installed, removed, started or updated, but one of the most interesting things about the OSGI initiative is that, all those could be done without restarting your application. So, you can hot-deploy (deploy a component on-the-go) your java components without the hassle of stopping and restarting your entire application, which always takes a lot of time.

Lets assume that you create an application as a single jar file and deploy it. Then, if you want to remove a package from the project and add a new version of another package to the project, you just have to do the changes in the code level and recompile and package the entire project as a brand new jar. Then you would have to stop the running application and redeploy it again. But, if you have used the modular OSGI based approach, you can simply “undeploy” that specific bundle and add a new version of the require bundle while the original application is already running !

# Bundles

The atomic entity in the OSGI specification is the bundle as I mentioned above. A bundle is a tightly coupled, collection of classes and a configuration file. Or to simplify that, a bundle is a jar file combined with a configuration file named MANIFEST.MF which contains the OSGI specific meta data.

The MANIFEST.MF file includes the metadata required by the OSGI runtime to register the jar as a bundle. MANIFEST.MF file specifies the naming of the bundle, the version of the bundle and most importantly the imports and exports which are used by the bundle.

```MF
Bundle-Name: Sample Client
Bundle-SymbolicName: org.rashm1n.Client1
Bundle-Description: An example client bundle
Bundle-Vendor: Apache Felix
Bundle-Version: 1.0.0
Bundle-Activator: example.rashm1n.client
Import-Package: org.osgi.framework
```

Above I have given a simple example of what a MANIFEST.MF file look like. The bundle name is the name of of the bundle/component, this could be a simple human-identifiable name, but the Bundle-SymbolicName must be unique to each bundle and cannot be duplicated. It is used by the OSGI runtime to uniquely differ each bundle. Normally a combination of group id and the artifact id is used to generate this SymbolicName.

Other two important fields given here are the Bundle-Activator and the Import-Package. In the Bundle-Activator field we need to give the name of the class which acts as the entry point to the bundle. That means the class which should be executed/run when that certain bundle is started. You can compare it to a public void main(String[] args) method in a normal Java application, so this Bundle-Activator class is the class which will be executed when the application first starts to run in the OSGI environment.

The Import-Package field defines the external packages/bundles which need to be imported by that certain bundle. Lets imagine that there are 3 bundles named example.rashmin.A, example.rashmin.B and example.rashmin.C. If the bundle A has the other two as “dependancies”, its Import-Package field will be something similar to the following.

```MF
Import-Package: example.rashmin.B, example.rashmin.C, org.osgi.framework
```

Similar to this Import-Package field there is another Export-Package field, which specifies which packages we need to export in our bundle, to be used by other bundles explicitly. I will talk about that in a later article.

# OSGI Implementations

Now that we got a brief understanding about the basics of OSGI. Lets talk about different OSGI implementations. Since OSGI is just an specification, concrete implementation are needed to be made to bring those specifications and standards into real life. And there are several popular OSGI implementations in the market currently.

1. Eclipse Equinox
2. Apache Felix
3. Knopflerfish OSGI

# Eclipse Equinox

In this article I will be talking about the Eclipse Equinox implementation. It implements all the latest specifications of the OSGI specification and is used as a reference implementation too. Lets try a small demonstration with the Equinox framework !

You could download the Equinox framework from this link. You could download the latest version or a previous version. Make sure to download the file named equinox-SDK-x.xx.zip. I will be using a previous version of the equinox sdk for this example. You could download it by the following command

```
wget http://www.eclipse.org/downloads/download.php\?file\=/equinox/drops/R-Neon-201606061100/equinox-SDK-Neon.zip
```

After you have downloaded the zip file, unzip it and you will see several folders and files inside. Now you have to create a separate folder and copy the following files from the <UNZIP_LOCATION>/plugins directory to it.

1. org.eclipse.osgi_XXX.jar
2. org.eclipse.equinox.console_XXX.jar
3. org.apache.felix.gogo.runtime_XXX.jar
4. org.apache.felix.gogo.shell_XXX.jar
5. Then create a folder inside that new folder named configiration, and create a config.ini file inside it with the following contents.

```
osgi.bundles=file\:org.eclipse.equinox.console_<YOUR_VERSION>.jar@start,file:\org.apache.felix.gogo.runtime_<YOUR_VERSION>.jar@start,file:\org.apache.felix.gogo.shell_<YOUR_VERSION>.jar@start
```

In my case the config.ini file would look like this

```
osgi.bundles=file\:org.eclipse.equinox.console_1.1.200.v20150929-1405.jar@start,file:\org.apache.felix.gogo.runtime_0.10.0.v201209301036.jar@start,file:\org.apache.felix.gogo.shell_0.10.0.v201212101605.jar@start
```

After this is done you could go to your OSGI directory from a terminal and enter the following command.

```
java -jar org.eclipse.osgi_<YOUR_VERSION>.jar -console
```

And a ternminal should appear.

There are a set of equinox specific commands which are used in this terminal. You could learn further more about the commands from the Equinox Official docs.

You could enter ss to view all your activated bundles.

![something](@assets/images/osgiimage1.webp)

Here you could see all the four jars we copied are here.

If you take a look at this text. You could notice two important columns, id and state.

Id is the unique id which is given to each installed bundle by the OSGI runtime. We can refer these bundle by this id easily, to start, stop or remove them. Then other column, State depicts the current state of that specific bundle. This can depict any stage of the bundle life-cycle like INSTALLED, RESOLVED, STARTING, ACTIVE, STOPPING or UNINSTALLED.

# Bundle Lifecycle

Each OSGI bundle goes through a certain life-cycle inside the OSGI framework, and there are states for all those life-cycle stages.

1. INSTALLED — Installing the bundle is done.
2. RESOLVED — OSGI Runtime has resolved all its dependancies
3. STARTING -The entry point of the bundle is called, and the bundle is starting
4. ACTIVE — Bundle successfully started and running
5. STOPPING/UNINSTALLING is pretty self-explanatory.

We could control these states of a bundle through the OSGI console, as we like, using commands like,

```
osgi > install file:home/user/service1.jar
osgi > start 5
osgi > stop 5
osgi > uninstall 5
```

(5 is the example OSGI id of the bundle.)

So by utilizing all these. You could make a jar with a MANIFEST.MF file and package it as a bundle and install/start/stop it anytime through the OSGI console.

In this article I have given a brief introduction into the world of OSGI in Java. In the next article I will be making a simple OSGI bundle and deploying it in the OSGI framework.

I hope you all gained something by reading the article. Give a follow if you enjoyed. Cheers.

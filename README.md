<p align="center">
  <img height="300" src="https://www.nevigo.com/articy-importer/unity/media/ad_Importer_Large.png">
</p>

# ArticyImporter for Unreal Engine 4

The Unreal Engine 4 importer allows integrating articy:draft content into Unreal projects and provides a simple C++ and blueprint interface to work with the data. We release this importer as a github open source project that will provide a substantial headstart into incorporating articy:draft data into Unreal projects while still allowing enough flexibility to adjust the importer to personal needs. 

While full support is not guaranteed for this product, we're constantly working on improvemets and would love to hear your suggestions. Feel free to forward us your ideas or even better directly [contribute](https://github.com/ArticySoftware/ArticyImporterForUnreal/blob/master/CONTRIBUTING.md) to the development of the importer.


# Table of contents

* [Goals & Features](https://github.com/ArticySoftware/ArticyImporterForUnreal#goals)
* [Setup](https://github.com/ArticySoftware/ArticyImporterForUnreal#setup)
* [Export from articy:draft](https://github.com/ArticySoftware/ArticyImporterForUnreal#export-project-from-articydraft)
* [Using the plugin](https://github.com/ArticySoftware/ArticyImporterForUnreal#using-the-plugin)



# Features

This importer provides a working foundation for integrating articy:draft content into Unreal Engine projects. You can expect the following features including but not limited to:

 * Everything accessible via **C++** and **Blueprint**
 * **Flow player** for automatic configurable flow traversal as an actor component
 * Automatic import
 * Database with your project data. *Excluding Journeys, Settings, Template constraints* 
 * Uses of Unreal Engine's localization

# Setup

There are a couple of steps needed to get the importer up and running. The Unreal project must be C++ compatible, therefore please ensure that the required tools are installed, such as Visual Studio for Windows or XCode for Mac.

To find out more about how to set up Visual Studio, click [here](https://docs.unrealengine.com/en-US/Programming/Development/VisualStudioSetup/index.html).

Following options are available when first using the importer:

## Create a new project

To create a new project, select one of the templates under the C++ tab during the project creation.


<p align="center">
  <img height="400" src="https://www.nevigo.com/articy-importer/unreal/create_new_cpp_project.png">
</p>

After you have created a new project, close the Unreal editor for now.

## For already existing Blueprint-only projects

Projects that existed prior to using the importer and used only Blueprints can be converted to C++ projects by adding any C++ class that inherits from UObject to the project. It is important that an option <b>other</b> than None gets selected. As an example, we choose the Actor class.

<p align="center">
  <img height="400" src="https://www.nevigo.com/articy-importer/unreal/create_new_cpp_class.jpg">
</p>

<p align="center">
  <img height="400" src="https://www.nevigo.com/articy-importer/unreal/select_actor_as_parent_class.png">
</p>

Click Next, name the class "MyActor" and finish the setup. Unreal Engine should now compile the "MyActor" class. After having compiled, your project now also works with C++.

## Clone/Copy the importer into the Unreal project

*Unreal Engine 4.18 and below use [the UE-4.18 tagged version](https://github.com/ArticySoftware/ArticyImporterForUnreal/releases) of the importer*

Get a copy of the importer and copy it into your projects **Plugins** folder. It is possible that you don't have one if it is a new project, so you can just create it.
Copy the folder **ArticyImporter** into this **Plugins** folder. Your project structure should now look something like this.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/copy_plugin.png">
</p>

## Adjust build configuration

For Unreal to correctly build the importer we need to add it as a dependency, to do that locate the file **Source/\<ProjectName>/\<ProjectName\>Cpp.Build.cs**

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/buildconfigfile.png">
</p>

And open it in your favorite text or code editor.

Now we need to adjust the existing code and make sure that the Importer is a dependency for the project. Locate the `PublicDependencyModuleNames` array and add `"ArticyRuntime"` as an additional dependency. Furthermore you should add the plugin header files to the public include path by adding `PublicIncludePaths.AddRange(new string[] { Path.Combine(ModuleDirectory, "../../Plugins/ArticyImporter/Source/ArticyRuntime/Public") });` below the dependencies.  
If you are working on macOS, you should also add `"Json"` to `PrivateDependencyModuleNames`.

Your file should now look something like this:

```
// Copyright 1998-2017 Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;
using System.IO;

public class MyArticyProject : ModuleRules
{
	public MyArticyProject(ReadOnlyTargetRules Target) : base(Target)
	{
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;

		PublicDependencyModuleNames.AddRange(new string[]
							{
								"Core",
								"CoreUObject",
								"Engine",
								"InputCore",
								"HeadMountedDisplay",
								"ArticyRuntime"
							});

		PublicIncludePaths.AddRange(new string[] { Path.Combine(ModuleDirectory, "../../Plugins/ArticyImporter/Source/ArticyRuntime/Public") });
	}
}
```

Make sure to save the file and close the editor.

## Enable the importer in Unreal

Now you can open your Unreal project and open the Plugins window by selecting Edit->Plugins in the main window menu bar.
Inside this window scroll in the list of groups down until you find the Group **Project** and the sub group **Articy**, click on Articy and enable the ArticyImporter on the right.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/enable_plugin.png">
</p>

After enabling the importer the window will prompt you to restart the editor, please do so to activate it for your project. When the editor restarts it will prompt you to rebuild missing dll files for the importer which you should confirm. This can take a bit depending on the size of the project and the power of your computer but once finished the Unreal editor should be back up again.

## Export project from articy:draft

Now that the importer is running, you are ready to export your data from articy:draft.
Open your articy:draft project and open the export window. Here you will find the Unreal Engine export. Please note that the Unreal export uses [Rulesets](https://www.nevigo.com/help/Exports_Rulesets.html) to choose what and how to export.
When exporting, chose your Unreal projects **Content** folder as the target for the `.articyue4` export file.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/export_options.png">
</p>

After every export, going back to Unreal will trigger the ArticyImporter plugin to automatically parse the new file and import the changes.

Make sure to confirm the prompt after the import to save all assets generated by the importer.

# Using the importer

Now that the importer is installed and your project data is imported you can start working on your project. 
Here are some quick tips at what to look for so you won't go in completely blind:

## Get object

There are many ways to access your objects, check this screenshot for a blueprint code showing you how to access an object by id/technical or directly clone it.
Also make sure to cast the object to the desired type to get access to its properties and its template.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/get_object.png">
</p>

## Accessing properties

Most of the time if you want to access the properties of an objects you queried from the database or got passed by the flow player callbacks (see below) you need to
cast the object to the correct type first. 

If you have an object without a template the type to cast into is easy.
Every built-in class is named as the object in articy:draft with your project name as a prefix. Lets say your project is named `ManiacManfred` so you will find `ManiacManfredFlowFragment`, `ManiacManfredDialogueFragment`, `ManiacManfredLocation`, `ManiacManfredEntity` and alot more. 
All those respective objects have their expected properties, so you will find the `Speaker` property inside the `ManiacManfredDialogueFragment` object etc.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/base_object_property.png">
</p>

> You will also find classes with the `Articy` prefix. Those are the base classes for the generated classes and casting into them works almost the same. This would allow you to create code that is reusable
> independant of any imported project.  

Dealing with templates is a bit more complicated. First thing to understand is that **all your articy:draft templates are new types** inside Unreal. 

The name of your template types also follows a similar structure as the one mentioned before, but utilizing the Templates technical name: `<ProjectName><TemplateTechnicalName>`. So if your project is called *ManiacManfred* and your templates technical name is *Conditional_Zone* your correct type would be called `ManiacManfredConditional_Zone`.
Its also worth mentioning that even if it is a new type, it is still derived from the base type with all its properties.

Accessing is easy once you have cast the object into the correct type, just drag a connection out of the node and search for the type to see all its properties.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/base_object_propertylist.png">
</p>

For templates it works the same way, but you will also find fields for every feature inside your template, so in the case of the `Conditional_Zone` template, there is a `ZoneCondition` field for the feature with the same name.
> Please note: It is possible that the **context sensitive** search does not properly work at this point in blueprint. When you disable it, you should be able to see all the fields inside your object.

Some properties are a bit more complex like reference strips and scripts etc:

* Scripts contain methods to `Evaluate` the underlying script. You can also access the `Expression` which is the original script in text form.
* ReferenceStrips and QueryStrips are just arrays.
* ReferenceSlots, `Speaker`, `Asset` inside the `PreviewImage` etc. are of type `ArticyId`, which can be plugged into `GetObject`.

So to reiterate:

1. Get object and cast to appropriate type.
2. Access the property/feature.
3. If it is a feature you access now the property inside the feature. <br/>
3a. If it is a script method, you can execute it via the `Evaluate` method.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/object_with_script.png">
</p>


## Articy Flow Player

The flow player is used as an automatic traversal engine. 
To set it up you add the Flow player actor component to one of your actors:

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/flow_player_component.png">
</p>

Now you can customize the flow player by setting the necessary options in the Setup section of the details panel. Most interestingly are **Pause On** an **Start On**

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/component_settings.png">
</p>

If you scroll down you will find the components event section. Here you probably want to add events for **On Player Paused** and **On Branches Updated**

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/component_events.png">
</p>

Adding those will create new event nodes in the graph of your current actor and allow you to implement your custom logic. 

To quickly reiterate how to use those: **On Player Paused** is called with the current paused object of the flow traversal, the current spoken dialogue fragment for example; and **On Branches Updated** to create the user choices for the current pause.

Here is an example blueprint implementation for both methods

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/component_basic_event_implementation.png">
</p>

The `ShowPausedObject` method is to display the current pause on the ui. Here is the implementation of that method.


<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/show_paused_text.png">
</p>

`spokenText` is bound to a UI text block.


And the **On Branches Updated** is used to create a vertical list of buttons. How to create those buttons, creating the layout etc. is out of scope of this quick guide but it is important that you
store the branch in every button. When you instantiate the button you should pass in the reference used in the forloop and when the button is clicked you use that branch as the index for the flow player so it knows where to continue.

<p align="center">
  <img src="https://www.nevigo.com/articy-importer/unreal/clicked_branch.png">
</p>

If you want to learn more about the flow player and its events you can read the [unity documentation](https://www.nevigo.com/articy-importer/unity/html/howto_flowplayer.htm) as both implementations are based on the same principles.

# Contributing

We are very grateful for any kind of contribution that you bring to the ArticyImporter, no matter if it is reporting any issues, or by actively adding new features, or fixing existing issues. If you want to know more about how to contribute please check our [Contribution](https://github.com/ArticySoftware/ArticyImporterForUnreal/blob/master/CONTRIBUTING.md) article.

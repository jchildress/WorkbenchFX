# WorkbenchFX
[![Build Status](https://travis-ci.com/FHNW-IP5-IP6/WorkbenchFX.svg?token=8WqsSGJvE4SAqmHHx2Z7&branch=develop)](https://travis-ci.com/FHNW-IP5-IP6/WorkbenchFX)

**The one and only library to build large JavaFX Applications!**

![screenshot of an application created with WorkbenchFX](docs/images/workbenchFX_in_use.png) 

## Table of Contents
- [What is WorkbenchFX?](#what-is-workbenchfx)
- [Advantages](#advantages)
- [Main Features](#main-features)
- [Documentation](#documentation)
- [Basic Structure](#basic-structure)
- [Demos](#demos)
- [Getting Started](#getting-started)
  - [Extending the `WorkbenchModule`](#extending-the-workbenchmodule)
  - [Creating the `Workbench`](#creating-the-workbench)
  - [Optionals](#optionals)
- [ToolbarItem](#toolbaritem)
- [Using Dialogs](#using-dialogs)
- [Prevent module from closing](#prevent-module-from-closing)
- [Team](#team)

## What is WorkbenchFX?
TODO: description

## Advantages
- Less error-prone
- Less code needed
- Easy to learn
- Easy to understand
- Easy to use, especially for developers which have not much experience in working with JavaFX
- A well designed UI, inspired by the material design standards

## Main Features
- Simple and understandable API
- Encapsulating multiple, independent `WorkbenchModules`, and display them in Tabs
- A predefined stylesheet which allows the user to change the styles as he needs to
- The most important features are noted in the picture and the corresponding table below:

![screenshot of the most important features WorkbenchFX provides](docs/images/features.png)

Nr. | Feature | Description
--- | ------- | -----------
1 | `WorkbenchModule` | what is a module? extend from wbmod, put different mods into the wb 
2 | `Pagination` | mods displayed in addpage, pagination, multiple sites
3 | `Tabs` | opening modules in multiple tabs, open/closeable
4 | `NavigationDrawer` | automatically blend in when items
5 | `Toolbar` | 
6 | `ToolbarItems` | when defining custom -> just use the node ctor
7 | `ModuleToolbar` | automatically blend in and out as you like 
8 | `WorkbenchDialog` | providing various default dialog types

## Documentation
This project uses the `asciidoctor` plugin to generate the necessary documentation. Run the following *maven* task:
```Maven
process-resources
```
Afterwards, you will find the documentation in the `target/generated-docs/` subdirectory.

## Basic Structure
WorkbenchFX uses the builder pattern to create the application, since one can use plenty of optional features.
The minimal usage requires only the definition of a custom extension from the `WorkbenchModule`.
Afterwards one can define further functionality calling the equivalent method.

For better illustration, the basic concept of creating a workbench application is shown below:
```Java
Workbench workbench = 
    Workbench.builder( // Using the static method call
        new CustomWorkbenchModule() // Extension of the WorkbenchModule
        ...
    )
    // .toolbarRight(...) // optional usage of additional features eg. navigationDrawer(), modulesPerPage(), etc.
    .build(); // The build call finishes and returns the workbench
```

Notes:
- The result of the `build()` call is a `Control` which can be set in a scene.
- It is also possible to use the default constructor `new Workbench()` and add the `WorkbenchModules` and features afterwards.

## Demos
We created several demos to visualize the capabilities of `WorkbenchFX` in the `workbench-fx-demo` folder:

File | Description
---- | -----------
`CustomDemo.java` | A workbench application which uses all features, to demonstrate the full capability of `WorkbenchFX`
`ExtendedDemo.java` | To demonstrate an application with a lot of `WorkbenchModules`
`SceneBuilderDemo.java` | A proof of concept, if the API also works with `SceneBuilder`
`StadardDemo.java` | Shows the simplest usage of `WorkbenchFX` with only three modules and no optional features  

## Getting started
### Extending the `WorkbenchModule`
An extension of the abstract class `WorkbenchModule` is required in order to create an application:

```Java
public class CustomModule extends WorkbenchModule {
  
}
```

It is then required to call the `super()` constructor and hand over a `String` as name and either an `Image`, `FontawesomeIcon` or `MaterialDesignIcon` as icon for the module:

```Java
public CustomModule() {
  super("My first Workbench module", MaterialDesignIcon.THUMB_UP); // A name and an icon is required
}
```

Cheatsheets for using the icons are available at:
- [materialdesignicons.com](https://materialdesignicons.com/)
- [fontawesome.com](https://fontawesome.com/v4.7.0/)


Furthermore, overriding the `activate()` method is also required.
This method will be called when clicking on the `Tile` to open the module:

```Java
@Override
public Node activate() {
  return new Label("Hello World"); // return here your actual content to display
}
```

The minimal implementation of a custom `WorkbenchModule` finally looks like the code snippet below.
For further information we refer to the `Javadoc`.

```Java
public class CustomModule extends WorkbenchModule {
  public CustomModule() {
      super("My first Workbench module", MaterialDesignIcon.THUMB_UP); // A name and an icon is required
  }
  @Override
  public Node activate() {
      return new Label("Hello World"); // return here your actual content to display
  }
}
```

### Creating the `Workbench`
After extending the `WorkbenchModule`, the application can be created.
To do this, one can access the `WorkbenchBuilder` by calling `Workbench.builder()`, setting the previously written module and build the `Workbench` by calling the `build()` method:

```Java
// Creating the Workbench
Workbench customWorkbench = Workbench.builder( // Accessing the WorkbenchBuilder
    new CustomModule() // Adding the CustomModule
).build(); // Building the Workbench
```

It then can be set into a scene:

```Java
public class CustomDemo extends Application {
  public static void main(String[] args) {
    launch(args);
  }

  @Override
  public void start(Stage primaryStage) {
    
    // Creating the Workbench
    Workbench customWorkbench = Workbench.builder( // Accessing the WorkbenchBuilder
        new CustomModule() // Adding the CustomModule
    ).build(); // Building the Workbench
    
    Scene myScene = new Scene(customWorkbench);
    primaryStage.setScene(myScene);
    primaryStage.setWidth(700);
    primaryStage.setHeight(450);
    primaryStage.show();
  }
}
```

This code snippet results to the following application:

![custom workbench](docs/images/customWorkbench.png)

The default implementation comes with a clickable `Tile` to open the module.
Opening the module, creates a `Tab` with the defined Icon and text and the content returned in the activate() method is displayed in the center.
By clicking on the `+` button, one gets back to the `AddModulePage`.
Closing the opened module is achieved through clicking on the close button in the `Tab`.

### Optionals
The optionals are called after adding the custom modules to the builder:

```Java
Workbench workbench = Workbench.builder(...)
.modulesPerPage(6) // call the optionals
.build();
```

The following parameters are optionally available to further configure the application:

Method (Workbench) | Description
------------------ | -----------
`modulesPerPage()` | Define the amount of `Tiles` which are displayed at the `AddModulePage`
`navigationDrawerItems()` | Allows setting multiple `MenuItems` which are then displayed in the `NavigationDrawer`. The button to open and close the drawer appears alway in the top-left corner
`toolbarLeft()` | Allows setting multiple `ToolbarItems` on the left side of the toolbar on top of the `Tabs`
`toolbarRight()` | Allows setting multiple `ToolbarItems` on the right side of the toolbar on top of the `Tabs`

When the default layout of `Page`, `Tab`, `Tile` or `NavigationDrawer` doesn't fulfill the desired requirements, it is possible to replace them:

Method (Workbench) | Description
------------------ | -----------
`navigationDrawer()` | Allows setting a custom implementation of the `NavigationDrawer` control, which will then be used
`pageFactory()` | Requires a `Callback` function which takes a `Workbench` and then returns a custom implementation of a `Page` control
`tabFactory()` | Requires a `Callback` function which takes a `Workbench` and then returns a custom implementation of a `Tab` control
`tileFactory()` | Requires a `Callback` function which takes a `Workbench` and then returns a custom implementation of a `Tile` control

The `WorkbenchModule` provides also optional functionality. It is possible to add `ToolbarItems` to the toolbar of the module (just like in the workbench):

Method (WorkbenchModule) | Description
------------------------ | -----------
`getToolbarControlsLeft()` | Calling this method returns an `ObservableList` of `ToolbarItems`. Adding items to the list will automatically create a toolbar between the `Tab` and the module content and adds the items on the left side
`getToolbarControlsRight()` | Calling this method returns an `ObservableList` of `ToolbarItems`. Adding items to the list will automatically create a toolbar between the `Tab` and the module content and adds the items on the right side

## ToolbarItem
Definition and behaviour of TBI

<table>
    <tr>
        <th>Syntax</th>
        <th>Outcome</th>
    </tr>
    <tr>
        <td><pre lang="java">
// Label with text
        </td>
        <td><img src="./docs/images/settings/integer_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Label with graphic
        </td>
        <td><img src="./docs/images/settings/integerSlider_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Label with text and graphic
        </td>
        <td><img src="./docs/images/settings/double_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Button with text
        </td>
        <td><img src="./docs/images/settings/doubleSlider_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Button with graphic
        </td>
        <td><img src="./docs/images/settings/boolean_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Button with text and graphic
        </td>
        <td><img src="./docs/images/settings/string_setting.png"></td>
    </tr>
    <tr>
        <td><pre lang="java">
// MenuButton with text
        </td>
        <td><img src="./docs/images/settings/observableList_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// MenuButton with graphic
        </td>
        <td><img src="./docs/images/settings/listProperty_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// MenuButton with text and graphic
        </td>
        <td><img src="./docs/images/settings/favourites_setting.png"/></td>
    </tr>
    <tr>
</table>


## Using Dialogs
Definition and behaviour of Dialogs.
List of all default Dialogs

<table>
    <tr>
        <th>Syntax</th>
        <th>Outcome</th>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/integer_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/integerSlider_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/double_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/doubleSlider_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/boolean_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/string_setting.png"></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/observableList_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Dialog
        </td>
        <td><img src="./docs/images/settings/listProperty_setting.png"/></td>
    </tr>
    <tr>
        <td><pre lang="java">
// Custom Dialog
        </td>
        <td><img src="./docs/images/settings/favourites_setting.png"/></td>
    </tr>
    <tr>
</table>

## Prevent module from closing
// Chapter about using the Dialog to save and close

## Team
- Marco Sanfratello
  - marco.sanfratello@students.fhnw.ch
  - Skype: sanfratello.m@gmail.com 
  - GitHub: Genron

- François Martin
  - francois.martin@students.fhnw.ch 
  - Skype: francoisamimartin
  - GitHub: martinfrancois
  
- Dirk Lemmermann
  - dlemmermann@gmail.com
  - Skype: dlemmermann
  - GitHub: dlemmermann
  
- Dieter Holz
  - dieter.holz@fhnw.ch
  - Skype: dieter.holz.canoo.com
  - GitHub: DieterHolz

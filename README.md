<p align="center">
<b>get_soju</b><br>
Enables live coding in Unity<br>
<!--
    <a href="docs/">Documentation</a> <b>/</b> <a href="https://pjc0247.github.io/UniScript/uniplayground/">Web Playground</a>\
-->
</p>

Remarks
----
This repository does not contains any kinds of source file or references since this project is not an open-source.<br>
However, you can still report an issue or learn how to use thsi product via this `README.md`.

Usage
----
### Setup
Just import the package, everything will be prepared automatically.<br>
No additional coding required.

### Remote
* Make sure you have enabled `Soju/Enable Remote` in the top menu.
* Make sure your PC and remote device connected to same network and can be reachable each other.
* Build the project. (Currently Android and Win32 platforms are supported)
* Click `Soju/Remote` menu to start remote mode.
* Run the binary which you built. If everything goes fine, you may see `Connected` message on the screen.
* Start modifying. In this period, every modification won't be applied to Unity Editor but remote device.
  * You MUST close the remote window to play in the editor.

Boost compliation time
----
If your project has many script files or contains heavy plugins, consider to use [`asmdef`](https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html) to reduce compliation time.<br>
__Soju__ automatically excludes scripts that already compiled into binary using `asmdef`. It would be great help to reduce both runtime & unity compliation time.


Hot reload contents (LUA)
----
It does not support any kind of contents reloading directly, but you may interested in using SOJU connection to deliver contents to mobile devices from the Editor.<br>
Here's a short guide to communicate between remote devices using SOJU connection.

__Soju/Editor/SojuReload.cs__<br>
You can send your contents here:
```cs
if (asset.EndsWith(".cs") == false)
```
```cs
SojuRemoteServer.SendCommand(new YourCustomPacket()
{
	contentsToDliver = WHATEVER_YOU_WANT
});
```

__Soju/SojuRemoteClient.cs__<br>
Will be called in client(remote device) side, update received contents here.
```cs
private void OnMessage(object sender, MessageEventArgs e) {
     /* PROCESS YOR CUSTOM PACKET HERE */
}
```

Limitations
----
* Adding new methods to existing type may lead your game to unexpected behaviour. (It does not mean to break the entire project or Unity Editor itself.)
    * Creating a lambda expression is not working for the same reason.
    * Adding a LINQ cluase with `pred(Action, Func...)` is not working.
    * Modifying existing methods or LINQ works fine.
* Adding new classes is not allowed
* Modifying class structure (field, property) is not allowed

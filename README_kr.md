<p align="center">
<b>get_soju</b><br>
Unity를 위한 라이브 코딩 지원<br>
    <a href="https://youtu.be/gFizNBs0okk">Youtube Demo</a>
</p>

Remarks
----
This repository does not contains any kinds of source file or references since this project is not an open-source.<br>
However, you can still report an issue or learn how to use this product via this `README.md`.

How to Guide
----
### 설정하기
패키지를 임포트하는것만으로 자동으로 활성화되기 때문에 추가적인 코딩이 필요 없습니다.

### 리모트 기능 사용하기
* 메뉴에서 `Soju/Enable Remote` 항목이 체크되었는지 확인해 주세요. 
* __PC__ 와 __스마트폰__ 이 같은 네트워크에 연결되어 있는지 확인해 주세요. (LTE 네트워크에선 작동하지 않을 수 있습니다.)
* 준비가 되었으면 프로젝트를 빌드해 주세요. (현재는 안드로이드와 Win32만 지원합니다)
* 메뉴에서 `Soju/Remote` 를 클릭하여 리모트 모드를 시작합니다.
* 유니티상에서 리모트 윈도우가 표시된 상태에서 빌드된 바이너리를 실행합니다. 만약 정상적으로 연결되었다면 `Connected` 메세지를 확인하실 수 있습니다.
* 코드를 수정하면 리모트 디바이스에 적용됩니다. 이 상태에서는 변경사항이 유니티 에디터에는 적용되지 않고 리모트 디바이스에만 반영됩니다.
  * 유니티 에디터에 변경사항을 적용하기 위해서는 리모트 윈도우를 반드시 종료해주세요.

Tips
----
### Boost compliation time
Most cases, __SOJU__'s compliation is way faster than Unity's. But still can be a glitch in huge projects.<br>
If your project has many script files or contains heavy plugins, consider using [`asmdef`](https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html) to reduce compliation time.<br>
__Soju__ automatically excludes scripts that already compiled into binary using `asmdef`. It would be great help to reduce both runtime & unity compliation time.


### Hot reload contents (LUA)
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

제한사항
----
Since .NET does not support runtime method swapping, there're some limitations which can't be accomplished.

* Adding new methods to existing type may lead your game to unexpected behaviour. (It does not mean to break the entire project or Unity Editor itself.)
    * Creating a lambda expression is not working for the same reason.
    * Adding a LINQ cluase with `pred(Action, Func...)` is not working.
    * Modifying existing methods or LINQ works fine.
* Adding new classes is not allowed
* Modifying class structure (field, property) is not allowed

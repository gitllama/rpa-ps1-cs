# RPA(PowerAutomate Desktop) - powershell script - c# script

## Version確認

特に詳しい説明なしにとりあえず動かしてみる。Versionがわからないと動かないとき困りますからね。

以下はRobin(PowerAutomate Desktopの言語) code

PowerAutomate Desktop(以下PAD)にコピペするとそのまま使える。（いい加減、Import/Export機能くらいつけてほしいところですが。）

```powershell
Scripting.RunPowershellScript Script: $'''Add-Type -TypeDefinition @\'
using System;
using System.Runtime.InteropServices;

public class Class1
{
  public static string Ver()
  {
    return RuntimeInformation.FrameworkDescription;
  }
}
\'@ -Language CSharp

$hash = @{}
$hash[\"ps_var\"] = $PSVersionTable.PSVersion
$hash[\"cs_var\"] = [Class1]::Ver()
ConvertTo-Json $hash''' ScriptOutput=> Out ScriptError=> ScriptError

# [ControlRepository][PowerAutomateDesktop]

{
  "ControlRepositorySymbols": [],
  "ImageRepositorySymbol": {
    "Name": "imgrepo",
    "ImportMetadata": {},
    "Repository": "{\r\n  \"Folders\": [],\r\n  \"Images\": [],\r\n  \"Version\": 1\r\n}"
  }
}

```

powershell scriptを抜き出したもの

```powershell
Add-Type -TypeDefinition @\'
using System;
using System.Runtime.InteropServices;

public class Class1
{
  public static string Ver()
  {
    return RuntimeInformation.FrameworkDescription;
  }
}
\'@ -Language CSharp

$hash = @{}
$hash[\"ps_var\"] = $PSVersionTable.PSVersion
$hash[\"cs_var\"] = [Class1]::Ver()
ConvertTo-Json $hash
```

うちの環境では、以下のような結果

```json
{
    "cs_var":  ".NET Framework 4.8.4470.0",
    "ps_var":  {
                   "Major":  5,
                   "Minor":  1,
                   "Build":  22000,
                   "Revision":  613,
                   "MajorRevision":  0,
                   "MinorRevision":  613
               }
}
```

```dotnet script```からcsxを呼ぶと.Net CoreのVersionが表示されると思うのでそのへんのVersion違いの挙動は注意です。

## Python Scriptを使用する場合

```python
import sys
print(sys.version)
```

---
title: Vscode Omnisharp Timeout Issue
date: 2020-12-10
categories:
- VSCode
tag:
- VSCode
- C#
---

## OmniSharp server load timed out 問題

### 問題

最近在用公司電腦測VSCode開 .Net Core MVC專案，還有Unity的專案都跑下面的Log，並且無法顯示intellisense

```Powershell
[ERROR] Error: OmniSharp server load timed out. Use the 'omnisharp.projectLoadTimeout' setting to override the default delay (one minute).
```

按照說明去改設定也照樣沒用，翻了github的issue，有些人說要裝 .Net Framework 4.0 4.5 4.7.2才能正常，但是我這邊的案例裝了也無效。

最後翻到了這篇：<br>
https://teratail.com/questions/171011

使用者了路徑如果包含非英文字元就會有問題，不過由於事公司配的電腦也不太可能特地開個別的使用者，最後選擇了比較曲折的辦法。

把VSCode C# Extension 幫我們自己抓的omnisharp資料夾，複製到純英文的路徑。<br>

下面是原始路徑，各版本可能會有所差異

``` Powershell
C:\Users\使用者\.vscode\extensions\ms-dotnettools.csharp-1.23.7\.omnisharp\1.37.4
```

我個人是移到c:的最底層，接著，去VSCode的 setting.json 覆蓋預設的設定
``` Json

{
    "omnisharp.path": "C:\\omnisharp\\OmniSharp.exe",
}
``` 
這樣就可以正常運作了

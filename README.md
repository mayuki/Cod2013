HTMLアプリをWindowsストアプリに。 - Community Open Day 2013
==========================================================

1. Visual StudioでWindows Store AppsのJavaScriptアプリケーション (Blank App) を作成する
2. App.htmlとcss,jsフォルダをプロジェクトに放り込む
3. Start pageをdefault.htmlからApp.htmlにする
4. window.navigator.msSaveBlob がストアアプリにはないのでmsSaveBlobをWinRTで書きかえる
5. 保存できるようになる

もっとWindowsストアアプリっぽくする
--------------------------------
- default.htmlにApp.htmlの内容をコピーする
- Start pageをdefault.htmlにもどす
- とりあえず実行してみる
- Blendで開く
- AppBarを追加する
- Saveボタン (savelocal)
- Importボタン (add)
- SaveボタンのonclickオプションにsaveToLocalを指定
- saveToLocal関数にWinJS.Utilities.markSupportedForProcessing(saveToLocal)を適用
- ファイル選択ダイアログを表示する関数を追加する

```JavaScript
function openFilePicker() {
    var inputE = document.querySelector('#filePicker');
    inputE.click();
}
WinJS.Utilities.markSupportedForProcessing(openFilePicker);
```

- ImportボタンのonclickオプションにopenFilePickerを指定
- Sepiaボタン(pictures)を追加

```JavaScript```
WinJS.Utilities.markSupportedForProcessing(processImages);
```

- SepiaボタンのonclickオプションにprocessImagesを指定
- Penボタン(highlight)を追加
- ペンモードを切り替える関数を追加

```JavaScript
function switchPenMode(command) {
    command.target.winControl.selected = !command.target.winControl.selected;
    document.querySelector('#paint-canvas').classList.toggle('enabled');
}
WinJS.Utilities.markSupportedForProcessing(switchPenMode);
```

- PenボタンのonclickオプションにswitchPenModeを指定
- css/app.css からGrid Layout/その他見た目のスタイルを削除
- #controlsをdisplay:noneに

カメラを使う機能を付ける
----------------------
- AppBarにCameraボタン(camera)を追加する
- CapabilitiesのWebcamにチェックを入れる
- コードを追加

```JavaScript
/* =====================================================================
 * カメラで撮影する
 * ===================================================================== */
function captureImageFromCamera() {
    var cameraCaptureUi = new Windows.Media.Capture.CameraCaptureUI();
    cameraCaptureUi.captureFileAsync(Windows.Media.Capture.CameraCaptureUIMode.photo).done(function(file) {
        if (!file) return;
        file.openReadAsync().done(function(stream) {
            var blob = MSApp.createBlobFromRandomAccessStream(file.contentType, stream);
            loadImagesFromFiles([blob], 1366 / 2, 768 / 2);
        });
    });
}
WinJS.Utilities.markSupportedForProcessing(captureImageFromCamera);
```

- CameraボタンのonclickにcaptureImageFromCameraを指定


1. Visual StudioでWindows Store AppsのJavaScriptアプリケーション (Blank App) を作成する
2. App.htmlとcss,jsフォルダをプロジェクトに放り込む
3. Start pageをdefault.htmlからApp.htmlにする
4. window.navigator.msSaveBlob がストアアプリにはないのでmsSaveBlobをWinRTで書きかえる
5. 保存できるようになる

もっとWindowsストアアプリっぽくする
--------------------------------
1. default.htmlにApp.htmlの内容をコピーする
2. Start pageをdefault.htmlにもどす
3. とりあえず実行してみる
4. Blendで開く
5. AppBarを追加する
	1. Saveボタン (savelocal)
	2. Importボタン (add)
	3. SaveボタンのonclickオプションにsaveToLocalを指定
	4. saveToLocal関数にWinJS.Utilities.markSupportedForProcessing(saveToLocal)を適用
	5. ファイル選択ダイアログを表示する関数を追加する
```JavaScript
function openFilePicker() {
    var inputE = document.querySelector('#filePicker');
    inputE.click();
}
WinJS.Utilities.markSupportedForProcessing(openFilePicker);
```
	6. ImportボタンのonclickオプションにopenFilePickerを指定
	7. Sepiaボタン(pictures)を追加
	WinJS.Utilities.markSupportedForProcessing(processImages);
	8. SepiaボタンのonclickオプションにprocessImagesを指定
	9. Penボタン(highlight)を追加
	10. ペンモードを切り替える関数を追加
```JavaScript
function switchPenMode(command) {
    command.target.winControl.selected = !command.target.winControl.selected;
    document.querySelector('#paint-canvas').classList.toggle('enabled');
}
WinJS.Utilities.markSupportedForProcessing(switchPenMode);
```

	11. PenボタンのonclickオプションにswitchPenModeを指定
6. css/app.css からGrid Layout/その他見た目のスタイルを削除
6. #controlsをdisplay:noneに

カメラを使う機能を付ける
----------------------
1. AppBarにCameraボタン(camera)を追加する
2. CapabilitiesのWebcamにチェックを入れる
コードを追加

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
3. CameraボタンのonclickにcaptureImageFromCameraを指定


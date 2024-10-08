## Auto Replacement Chrome icns
<img width="100%" src="https://github.com/user-attachments/assets/538686be-3331-4e28-bad8-322675404fed">

### Why
Googleが提供するChromeのアイコンがダサいので、
Chromiumのアイコンに置換えたい。

### What
#### Overview
- 📁
    - ℹ️ readme.md
    - 📄 replace_chrome_icns.sh* Chromeアプリアイコン置換えシェル
    - 📄 com.user.replace_chrome_icns.plist →`~/Library/LaunchAgents/com.user.replace_chrome_icons.plist`
    - 🖼️ app.icns*
    - 🖼️ adocument.icns*
    - 📄 .chrome_last_update （自動生成）

#### Explain
1. Chromeの更新の検知
2. アイコンの置き換え
    - ▼以下で権限を与えて使用
    - `$ chmod +x replace_icns.sh`
3. 自動実行の仕組み(Launchdを使用)
    - 所定パス先へファイル格納
    - `$ cp com.user.replace_chrome_icns.plist $HOME/Library/LaunchAgents/`
    - ▼launchctlでロード: この設定ファイルをロードして有効化
    - `$ chmod 644 ~/Library/LaunchAgents/com.user.replace_chrome_icns.plist`
    - `$ sudo launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.user.replace_chrome_icns.plist`
        - (old) `$ launchctl bootstrap ~/Library/LaunchAgents/com.user.replace_chrome_icns.plist`

- <details>
    <summary> Debug </summary>
    
        様々な確認手法
    
    - ロードされたジョブの確認
        - `$ launchctl list | grep com.user.replace_chrome_icns`
        - launchctl listコマンドを使用して、現在ロードされているジョブの一覧確認
    - ログの確認
        - `$ log show --predicate 'eventMessage contains "com.user.replace_chrome_icns"' --info --last 1h`
        - ※ Console.appやlogコマンドを使用してログを確認
    - ロード状態の詳細確認
        - `$ launchctl print gui/$(id -u)/com.user.replace_chrome_icns`
        - ジョブの詳細な状態情報が表示されジョブが正しくロードされていれば、その内容が表示
    - スクリプトの実行確認
        - `$ echo "Script executed at $(date)" >> ~/replace_chrome_icns.log`
        -  replace_chrome_icons.logファイルに実行の履歴がTOPに残る
    - plistファイルのフォーマットが正しいかどうかを確認（構文や不正な文字列がないか確認できる）
        - `$ plutil ~/Library/LaunchAgents/com.user.replace_chrome_icns.plist`
    - Plistファイルのパスと権限の確認
        - `$ ls -l ~/Library/LaunchAgents/com.user.replace_chrome_icns.plist`


  </details>

### Ref.
- Download
    - [Google Chrome](https://www.google.com/chrome/?brand=YTUH&ds_kid=43700049288280949&gad_source=1&gclid=CjwKCAjwxNW2BhAkEiwA24Cm9F5biVBlGBgzpQ_teRtC8Tm3Dg9FPOuf2JswFyXcC24d6Ogwtb0CvhoC9moQAvD_BwE&gclsrc=aw.ds)
    - [Chromium](https://www.chromium.org/getting-involved/download-chromium/)
- [サービス管理したい（launchctl）](https://kumaroot.readthedocs.io/ja/latest/command/command-launchctl.html#launchctl)
- [launchctl](https://ss64.com/mac/launchctl.html)



# ユーザーが利用する流れ
1. オフセットを実行したい線を選択する
1. 本数(整数)とオフセット幅(少数)を入力する画面が表示される
1. 上記を入力する
1. OKボタンを押す
1. 選択した線のオフセットが、指定した幅で本数分実行される

# オフセット部分の流れ
1. 元の線分データを作成し、線分の起点と終点と傾きを保持しておく
1. 元の線分の点ごとに、以下を実行
    - 傾きとオフセット幅に従ってpointsをオフセットする
    - 4点を正しく結んで2つの線分を作成する
    - 作成した2つの線分で、元の線分の進行方向に従い右か左か振り分ける
1. ベースの線分データとオフセットした2つの線分を格納する
1. オフセットした2つの線分データで線分間の交点を算出し格納していく
1. 交点の集まりをpathPointsとして線を実際に描画する

# npm-scriptについて
- windows向けですので、各自で適宜変更してください
    - 想定しているフォルダ構成は別記の通りです
- 今回使用していませんが、zxpのビルドにはシェルスクリプトも用意しました
- powershellが環境変数にセットされている前提です
- sampleを元に、powershellのデータを各自で用意いただく形になります

## npm-scriptsで想定している物

### clean
- io.kotodu.iloをアンインストールする
    - 結果のステータスが-406なら、そもそも拡張機能が存在しない

`npm run clean`

### deploy
- cleanを除く一連の動作を全て実行する
    - build, deploy, check
`npm run deploy`

### build:tsc
- TypeScriptで記述したソースをビルドする
`npm run build:zxp`

### build:zxp
- 拡張機能のzxpデータをビルドする
`npm run build:zxp`

### deploy:zxp
- ビルドデータをillustratorにインストールする
`npm run deploy:zxp`

### check
- インストールできたか確認する
`npm run check`

## 想定しているフォルダ構成
- パスを適宜変える必要があります

```
C:/Users/YourName/Documents
├ Github
│ └ illustrator-Line-Offset
│ 　 ├ dist(ビルドしたものを出力するディレクトリ)
│ 　 │ └ ilo.zxp(出力拡張機能)
│ 　 ├ src
│ 　 │ └ ソースコード群
│ 　 ├ package.json
│ 　 ├ build.ps1(拡張機能ビルド用powershell)
│ 　 ├ clean.ps1(拡張機能アンインストール用powershell)
│ 　 ├ install.ps1(拡張機能インストール用powershell)
│ 　 └ zxpbuild.sh.sample(今回未使用のビルド用シェルスクリプト)
├ ExManCmd_win
│ └ ExManCmd.exe(インストール用CUI)
├ ZXPSignCmd.exe(ZXP用の証明書発行exe)
└ sign.p12(ZXPSignCmd.exeから出力された証明書)
```
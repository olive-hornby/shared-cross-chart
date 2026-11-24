# 共有十字チャート（無料公開用スターター）

## できること
- 共通ルームID＋パスワードで盤面を共有
- Firebase Realtime Database によるリアルタイム同期・保存
- 十字チャートの追加・削除、軸ラベル変更
- 複数チャート表示／1チャート表示
- プリセットアイコン／画像アップロード
- アイコンの配置・ドラッグ移動
- アイコン名・コメント
- PNG書き出し、JSONバックアップ／読み込み

## 重要：パスワード方式について
このスターターは `SHA-256(ルームID:パスワード)` をデータベース上のルームキーとして使います。パスワード自体は保存しません。これは小規模な身内利用向けの簡易アクセス制御です。強固な認証や機密情報の保護には使わないでください。長く推測されにくいパスワードを使い、公開情報や個人情報を置かないでください。

また、初期版の同期は盤面全体を保存する方式です。同時に同じデータを編集した場合、タイミングによって後から保存した側の変更が優先されることがあります。大人数・高頻度編集向けではありません。

## 1. Firebaseを作る
1. https://console.firebase.google.com/ でプロジェクトを作成
2. Authentication > Sign-in method で **匿名（Anonymous）** を有効化
3. Realtime Database を作成
4. Database > Rules に `database.rules.json` の内容を貼り付けて公開
5. Project settings > Your apps からWebアプリを追加し、Firebase設定値を取得

## 2. HTMLにFirebase設定を入れる
`index.html` 内の `firebaseConfig` を、Firebaseコンソールに表示された値に置き換えます。
- `apiKey`
- `authDomain`
- `databaseURL`
- `projectId`
- `appId`

`databaseURL` はRealtime Databaseの画面に表示されるURLを正確にコピーしてください。

## 3. GitHub Pagesで公開
1. GitHubで新しいPublicリポジトリを作る
2. `index.html` と `database.rules.json` をアップロード
3. Settings > Pages > Deploy from a branch > `main` / `root` を選択
4. 表示された `https://ユーザー名.github.io/リポジトリ名/` が公開URL

GitHub Pagesは静的サイトの公開先です。Firebaseの設定値はブラウザに配信されるため、Database Rulesを必ず設定してください。Firebase Web API key自体は秘密情報ではありません。

## 4. 使い方
1. 公開URLを開く
2. ルームIDと共通パスワードを入力
3. 同じID・パスワードを参加者に共有
4. アイコン一覧からアイコンを選び、盤面をクリックして配置
5. アイコンをドラッグして位置を調整
6. PNGまたはJSONで書き出し可能

同じルームIDでもパスワードが違えば別の盤面になります。URLにはルームIDが付与されますが、パスワードはURLに含まれません。

## 無料枠・注意
Firebase Realtime Databaseには無料枠・通信量・保存量の制限があります。画像は450KB以下に制限していますが、多数の画像を登録するとデータベース容量・通信量を消費します。大きな画像を大量に登録しないでください。GitHub Pages/Firebaseの料金・無料枠は変更される場合があるため、公開前に各公式ページで最新条件を確認してください。

## 現在のプロトタイプの制約
- 盤面全体を同期するため、同時編集が重なると更新が競合する場合があります
- 参加者ごとの権限（閲覧のみ／編集可）やメンバー管理は未実装
- アイコン画像はRealtime Databaseに保存する簡易方式です。将来的にはFirebase Storage等への移行を推奨
- ルームの一覧・削除・管理画面は未実装
- Firebaseを設定しない場合はデモモードで画面操作のみ試せます（同期・保存なし）

## 公式ドキュメント
- Firebase: https://firebase.google.com/docs/database
- Firebase匿名認証: https://firebase.google.com/docs/auth/web/anonymous-auth
- GitHub Pages: https://docs.github.com/pages

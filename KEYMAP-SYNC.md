# キーマップ同期（同期先）

`config/mona2.keymap` は **自動生成ファイル**。直接編集しないこと。導入日 2026-10-28。

マスターは **[dgarai/zmk-config-moNa2-v2](https://github.com/dgarai/zmk-config-moNa2-v2)** の
`config/mona2.keymap`。あちらが更新されると、マスター側の
`.github/workflows/sync-keymap.yml` がこのリポジトリへ push し、その push で
このリポジトリの `build.yml`（ファームウェア）と draw ワークフロー（キーマップ図）が走る。

## キーマップを変えたいとき

**マスターを編集する。** このリポジトリを keymap-editor で開いて SAVE しても、
次の同期で上書きされて消える。keymap-editor にはマスターだけを登録しておくこと。

## 変換の内容

このキーボードはマスターと同じ moNa2 シールド（42キー）なので、キーマップは
マスターの丸ごとコピー。センサーの違い（PAW3222 / PMW3610）は `boards/` と
`config/*.conf`、`config/west.yml` 側にあり、これらは同期対象外なので触られない。

## 元に戻す

### 一時的に同期を止める

このリポジトリ側では止められない。マスターの Actions タブ →
"Sync keymap to other boards" → `…` → Disable workflow。

このリポジトリ側だけで断ち切るなら、Settings → Deploy keys から
`keymap-sync paw3222` のデプロイキーを削除する。以降マスターからの push は
権限エラーで失敗する（マスター側の同期ジョブが赤くなるので気付ける）。

### 同期導入前のキーマップに戻す

導入直前のコミットは `ae996d2b1faa9f86773b3a69049c6c67aa6acdc2`。

```sh
git fetch origin
git checkout main
git checkout ae996d2b1faa9f86773b3a69049c6c67aa6acdc2 -- config/mona2.keymap
git rm KEYMAP-SYNC.md
git commit -m "Revert keymap sync" && git push
```

マスター側の同期を止めずにこれをやると、次の同期で再び上書きされる。
先に上の「一時的に同期を止める」を行うこと。

### 完全に撤去する

上記に加えて、マスター側の `KEYMAP-SYNC.md` の「同期の仕組みを完全に撤去する」を実施する。

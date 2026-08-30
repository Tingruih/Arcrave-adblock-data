# arcrave-adblock-data

Arcrave（一個個人研究用的 Brave fork）所使用的廣告封鎖規則資料。

## 這是什麼

Brave 官方透過 Chromium 的 component updater 分發過濾清單，該通道需要
`BRAVE_SERVICES_KEY`，這把金鑰由 Brave 內部發放，個人 fork 無法取得
（實測未帶金鑰時 `go-updater.brave.com` 直接回 403）。

本 repo 的 workflow 每小時檢查一次上游，若有變動就用 **Brave 官方自己的打包腳本**
重新產生同一份資料，發佈到 GitHub Release 供 Arcrave 以一般 HTTPS 取得。
腳本一行未改，因此合併邏輯、清單順序、相容性過濾與 Brave 專屬 scriptlet 的
安全過濾都與官方一致。

## 資料來源

| 來源 | 用途 |
|---|---|
| [brave/adblock-resources](https://github.com/brave/adblock-resources) | 清單目錄 `list_catalog.json` 與資源庫 `resources.json` |
| [brave/adblock-lists-mirror](https://github.com/brave/adblock-lists-mirror) | Brave 對各第三方清單的公開快照 |
| [brave/brave-core-crx-packager](https://github.com/brave/brave-core-crx-packager) | 合併與打包腳本（`generateAdBlockRustDataFiles.js`） |

## 產物

Release tag `data`，每次覆蓋上傳。asset 名稱就是 Brave 的 component id：

```
https://github.com/Tingruih/arcrave-adblock-data/releases/latest/download/<component_id>
```

| asset | 內容 |
|---|---|
| `gkboaolpopklhgplhaaiboijnklogmbc` | `list_catalog.json` —— 清單目錄 |
| `mfddibmblmbccpadfndgakiopmmhebop` | `resources.json` —— scriptlet 資源庫 |
| 其餘 id | 該清單合併後的 `list.txt` |
| `manifest.json` | 本次建置所用的上游 commit SHA 與時間，供重現用 |

要重現任何一次建置：用 `manifest.json` 裡的 `mirror_sha` 跑
`npm run data-files-ad-block-rust -- --commit-hash <mirror_sha>`。

## 授權與歸屬

本 repo **不擁有**這些規則，也未修改規則內容，僅為個人研究 fork 重新打包發佈。

各過濾清單的著作權與授權條款歸原作者所有 —— EasyList、EasyPrivacy、
uBlock Origin filters、AdGuard 各語系清單等，分別採用 CC BY-SA 3.0、GPLv3
等授權。完整來源清單見
[`list_catalog.json`](https://github.com/brave/adblock-resources/blob/master/filter_lists/list_catalog.json)
的 `sources[].url` 欄位。

`resources.json` 衍生自 [uBlock Origin](https://github.com/gorhill/uBlock)
的 scriptlet（GPLv3）。

打包腳本來自 brave-core-crx-packager（MPL 2.0）。

本 repo 與 Brave Software 無關聯，非官方發佈。

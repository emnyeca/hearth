# Operations Manual（Rev0）

目的：Hearthリポジトリを迷わず運用し、製造データまで一貫して管理する。

## 1. 推奨ブランチ運用
- `main`：常に製造可能（少なくともRev0として破綻しない）
- 作業は短命ブランチ：例 `feat/psu-core` `fix/usb-c-cc` など

## 2. Revの付け方
- docs/design.md のタイトル行で `Rev0/RevA…` を管理
- 変更点は docs に追記（「なぜそうしたか」が残る形）

## 3. KiCadファイル運用
- KiCad 9.x 前提
- `kicad/` 直下にプロジェクト一式
- `kicad/symbols_footprints/` はプロジェクト専用のシンボル/フットプリント置き場（必要な場合）

## 4. 製造データ（fab/）
- 出力先：
  - `fab/gerber/`
  - `fab/bom/`

推奨：
- Gerber/Drillは zip にまとめて保存
- BOM/CPL（Pick&Place）も同じRevに紐づける

## 5. 出荷前セルフチェック（DMMだけで可能な範囲）
- VBUS/BAT/SYS/+3V3/+5V のテストパッドがシルクで判別できる
- LEDが仕様通りの条件で点灯/消灯する
- Boost Enable/Disable が確実に効く
- ダミー負荷が段階的に入れられる

## 6. リリース（製造版）の作り方
- `main` を更新
- タグ付け：例 `hearth-rev0.1`（※gitタグはユーザーが手動で）
- タグに紐づく `fab/` をそのまま製造に渡せる状態にする

## 7. JLCPCB向けメモ
- 実装部品の在庫有無で設計が揺れるため、
  - 主要IC（充電/パワパス/ブースト/LDO）は早めに在庫確認
  - 代替候補は docs に記録

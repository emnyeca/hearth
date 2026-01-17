# EUB-PSU “Hearth”

EUBシリーズ向けの **電源コア（Power Core）**。

- 家庭内使用前提：剛性より「かわいさ・安心感」
- **MCUなし / FWなし（Rev0）**
- **DMM（テスター）だけで切り分け可能**を最重要要件にする

## 目的
- Emiuet / ガムシンセ / コードメーカー 等へ電源を供給
- 電源単体で動作確認・切り分けが可能

## 仕様（Rev0 要約）
- 入力：USB-C（5V sink固定）/ LiPo 1セル（JST-PH 2pin）
- PowerPath：USB or バッテリー自動切替、USB接続時は給電+充電
- 出力：SYS / +3V3（LDO）/ +5V（昇圧）
- 外部出力：EUB-BUS OUT（JST XH 2.54mm 6pin）

## ドキュメント
- 設計仕様（判断軸）: docs/design.md
- 電源メモ: docs/power_notes.md
- 機構メモ: docs/mech_notes.md
- 運用手順: docs/operations.md

## KiCad
- KiCad 9.x 前提
- KiCadデータは kicad/ に配置

## リポジトリ運用（概要）
- Revは docs/design.md の冒頭で管理
- 製造データ（Gerber/BOM等）は fab/ に出力（手順は docs/operations.md）

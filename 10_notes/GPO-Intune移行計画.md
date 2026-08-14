---
tags:
  - ADリプレース
  - GPO
  - Intune
  - 移行計画
project: ADリプレース
created: 2026-08-13
updated: 2026-08-13
status: v17 (2026-08-03 更新・r3版が正)
source: claude/GPO_Intune移行計画_進行メモ.md (参照日 2026-08-13)
---

# GPO→Intune 移行計画 進行メモ

> [!info] 位置づけ
> 進行状況の正本メモ (v17・2026-08-03 更新)。納品物は r3 版が正。
> 索引: [[ADリプレース 索引]]

## 1. 正となる納品物

- パッケージ: GPO_Intune_Migration_20260729_r3.zip (全35ファイル)
- r2 (文字コード対応完了版) に対し、思考法・フレームワーク観点の再レビュー (2026-07-30) の結果を反映
	- 計画書 v0.8 → v0.9: 冒頭にエグゼクティブサマリー(結論ファースト)を1枚追加 (全29枚)。参照日 2026-07-30
	- スライド番号: 体制表=26、S-01〜13=27、AS/OP=28、リスク=25、全体フロー=13、実行計画=14〜18 (README_FIRST 更新済み)
	- 文字コード: ZIP内ファイル名は ASCII、テキストは BOM付き UTF-8 + CRLF
	- 今後 PS1 生成は必ず BOM付き UTF-8 + CRLF とする

## 2. 別冊: 設定移管 詳細計画書 (ユーザー作成・GPO→Intune フォーカス)

### レビュー経緯

- v1.0 (全56p・2026-08-03) をシニアコンサル観点で批判的レビュー。指摘20件 (高5 / 中10 / 低5) ＋ 5問チェック回答を提示
- 高5件の内訳:
	1. 出口 B/D 定義が既存計画書と非互換
	2. GPP = GPA 対象外という誤記
	3. ユーザースコープの競合制御限界の欠落
	4. 移行中の参加状態(ハイブリッド)未定義
	5. Go 基準「エラー0件」の形骸化
- v1.1 (全58p) で17件反映を確認
	- A〜E 定義・PRF / SG-INT 命名・リング構成・Phase 対応が既存計画書 v0.9 と整合化
	- 追加: 参加パターンページ、波の組成(領域×リング)ページ、R8〜R10、AS-01b / AS-03b、C-05 (GPA断念)
	- 新規記述「Windows 10 1803 で MDMWinsOverGP を 1→0 に戻せない」は CSP ドキュメントで裏取り済み (正確)

### 残指摘 (3件＋微修正2件)

- F-10: 出典 URL パスの実在確認 (中)
- F-18: 検証手順の画面パス・同期間隔が「確認中」のまま (低)
- F-19: 適用マトリクスの例示注記 (低)
- 微修正: エグゼクティブサマリーの競合制御記述にユーザースコープ未追記 / SG-INT-X 系の命名スロット不整合

## 3. プロジェクト要約 (引き継ぎ用)

- 出口5分類・2段階判定: A/D = GPO 単位、B/C/E = 設定単位
- 決定事項: 11件
- 環境分離: AD 端末はエクスポート＋承認済み書込のみ
- 未決: OP-01〜07 (OP-08 = AD 廃止目標日は提案中)。仮置き: AS-01〜05
- 社内 AI 活用は文書系タスク限定
- 検証: 読取・生成系＋作業PCフローは実測済み。書込系は -WhatIf のみ。分類精度は実データ未検証
- 思考法19種＋フレームワーク36種を学習済み → [[レビュー思考法フレームワーク]]

## 4. 次のアクション

1. r3 で受け入れテスト → 検証環境で少数 GPO 実測 (詳細計画書 v1.1 の Wave 0 に相当)
2. 詳細計画書の残指摘3件の反映 (ユーザー側)
3. 判断待ち: OP-05〜08、利用者影響・コミュニケーション計画、体制実名化、E-1 業務側再確認、ドライブマップ実装方式
4. 作成候補: 調査依頼票、B チェックリスト、3台帳・プロファイル設計書雛形
5. 実測反映で両計画書の v1.x → 確定版化

## 5. 主要出典 (参照日 2026-08-03)

- [Policy CSP - ControlPolicyConflict](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-controlpolicyconflict) — スコープ Device・Policy CSP 限定・1803 の「戻せない」挙動
- [Troubleshoot update rings](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-protection/troubleshoot-update-rings) — Update Policy CSP には不適用
- [Group Policy analytics のインポート](https://learn.microsoft.com/en-us/intune/device-configuration/import-group-policy-analytics) — 解析対象に Group Policy Preferences 明記
- [Administrative Templates (Windows)](https://learn.microsoft.com/en-us/intune/intune-service/configuration/administrative-templates-windows) — 2412 で非推奨・読み取り専用
- [Device profile troubleshoot](https://learn.microsoft.com/en-us/intune/intune-service/configuration/device-profile-troubleshoot) — 競合・コンプライアンス優先
- [Settings catalog printer provisioning](https://learn.microsoft.com/en-us/intune/intune-service/configuration/settings-catalog-printer-provisioning) — printers.csv 非推奨

## 6. 作業メモ (セッション内パス・参考)

- /home/claude/work/adreplace/ 配下: ascii_pkg/ (r3 パッケージ元)、gen_deck.js (v0.9)、gen_xlsx.py (v6)、tools/
- pwsh = /opt/pwsh
- fw_zukai.pptx = フレームワーク図解集
- user_plan_v1.md / user_plan_v1_1.md = 詳細計画書の抽出テキスト

> [!warning] 注意
> 上記パスは Claude セッション内の一時作業領域のパス。セッション終了後は失われるため、成果物は納品 ZIP を正とする。

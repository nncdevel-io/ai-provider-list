# ai-provider-list

ログ内の AI サービス通信を識別するための、
ドメイン許可・除外リストです。

## 概要

このリポジトリでは、AI プロバイダー向けドメインリストを
次の 2 形式で提供します。

- マッチング規則とメタデータを含む YAML
- フィルターや SIEM パイプラインに取り込みやすい CSV

## 収録ファイル

- `ai_domain_filter_list.yml`
  - `allow` / `exclude` を持つ正本データ
  - マッチング方針と用途情報を含む
- `ai_domains_ifilter.csv`
  - `type`（`ALLOW` / `EXCLUDE`）、
    `domain`、`generated_at` のフラット一覧
- `LICENSE`
  - MIT ライセンス

## マッチング規則

`ai_domain_filter_list.yml` の規則:

- URL からホスト名を抽出し、小文字へ正規化
- 次のいずれかで一致:
  - `host == domain`, or
  - `host` ends with `.` + `domain`
- `ALLOW` より先に `EXCLUDE` を適用

この優先順により、広いベースドメインを抑制しつつ、
必要な AI エンドポイントを許可できます。

## 判定例

URL ホストに対する判定フロー:

1. `exclude` のいずれかに一致するか確認
2. 一致したら `EXCLUDE`
3. 一致しなければ `allow` を確認
4. 一致したら `ALLOW`
5. どちらにも一致しなければ未分類として扱う

## ライセンス

MIT。詳細は [LICENSE](./LICENSE) を参照してください。

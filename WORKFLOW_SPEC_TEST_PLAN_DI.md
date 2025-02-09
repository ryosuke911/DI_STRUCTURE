# 仕様化テスト計画ワークフロー

## 1. 入力データ形式

分析フェーズからの入力を受け取り、現状の動作を記録するための計画立案の基礎とします。

### 1.1 分析結果の受け取り
```yaml
analysis_result:
  component_analysis:
    app/Http/Controllers/TodoController.php:
      metrics:
        coupling:
          direct_dependencies: 4
          indirect_dependencies: 7
          circular_dependencies: true
      
      characteristics:
        global_state: false
        singleton_usage: false
        static_methods: true
        external_services: true
      
      behavior_points:
        - "外部APIからのデータ取得"
        - "データベースへの保存"
        - "認証状態の確認"
```

## 2. 出力データ形式

### 2.1 仕様化テスト計画書
```yaml
spec_test_plan:
  target_components:
    - id: string                    # 対象コンポーネントの識別子
      observation_points:           # 動作を記録すべき箇所
        - point: string            # 観察ポイント（メソッドや処理フロー）
          priority: high|medium    # 優先度
          reason: string          # 優先度の理由
      
      recording_strategy:          # 動作記録の方法
        method: string            # 記録方式
        tools: string[]          # 使用するツール
        setup_requirements: string[] # 環境セットアップ要件
      
      expected_records:           # 記録すべき情報
        - type: "input"|"output"|"external_call"
          timing: "before"|"after"
          data_points: string[]   # 記録すべきデータ項目
```

### 2.2 実装フェーズへの入力
```yaml
implementation_input:
  components:
    - id: string
      test_scenarios:              # 記録すべき動作シナリオ
        - scenario_id: string
          description: string
          observation_points: string[]
          recording_points: string[]
      
      implementation_guide:        # 実装時の注意点
        setup_requirements: string[]
        recording_approach: string
        verification_points: string[]
```

## 3. 動作観察ポイントの特定

### 3.1 観察優先度の評価
```yaml
observation_points:
  high_priority:
    conditions:
      - "外部システムとの連携部分"
      - "データベース操作を含む処理"
      - "複数の依存を持つ処理フロー"
    reason: "依存性注入による影響が大きい箇所"
  
  medium_priority:
    conditions:
      - "静的メソッドを使用する処理"
      - "シングルトンを使用する処理"
    reason: "代替実装が必要になる可能性が高い箇所"
```

### 3.2 観察スコープの定義
```yaml
observation_scope:
  target:
    - "メソッドの入力に対する出力の対応関係"
    - "外部システムとの通信内容"
    - "データベースへの操作内容"
    - "例外発生時の振る舞い"
  
  not_target:
    - "内部実装の詳細"
    - "処理の正当性判断"
    - "パフォーマンス特性"
```

## 4. 動作記録戦略

### 4.1 記録方法の選定
```yaml
recording_strategy:
  external_api:
    method: "実際のリクエスト/レスポンスの記録"
    tool: "HTTPクライアントのログ機能"
    
  database:
    method: "SQL文とその結果の記録"
    tool: "クエリログ"
    
  static_methods:
    method: "入出力値の記録"
    tool: "ラッパー関数での記録"
```

### 4.2 記録環境の定義
```yaml
recording_environment:
  requirements:
    - "本番相当のデータセット"
    - "外部システムの検証環境"
    - "ログ収集の仕組み"
  
  constraints:
    - "本番環境に影響を与えない"
    - "センシティブデータの扱いに注意"
```

## 5. テスト実装計画

### 5.1 実装アプローチ
```yaml
implementation_approach:
  principle: "現状の動作をそのまま検証可能な形で記録"
  
  patterns:
    - pattern: "HTTPリクエスト/レスポンスの記録と再生"
      use_case: "外部API呼び出し"
      
    - pattern: "データベース操作のスナップショット"
      use_case: "永続化処理"
      
    - pattern: "静的メソッド呼び出しの記録"
      use_case: "ユーティリティクラス利用"
```

### 5.2 テスト構造
```yaml
test_structure:
  format:
    setup:
      - "記録した入力状態の再現"
    execution:
      - "対象メソッドの実行"
    verification:
      - "記録した出力との完全一致確認"
```

## 6. リスクと対策

### 6.1 技術的リスク
```yaml
technical_risks:
  - risk: "外部システムの応答が記録時と異なる"
    mitigation: "記録したレスポンスの再生機能の実装"
    
  - risk: "データベース状態の再現が困難"
    mitigation: "操作前後のスナップショット比較"
    
  - risk: "静的メソッドの記録が困難"
    mitigation: "一時的なラッパー導入による記録"
``` 
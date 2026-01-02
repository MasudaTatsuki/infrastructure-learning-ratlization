# TLS Protocol Structure

TLSの階層構造とハンドシェイクの全体像。AWS（ALB/CloudFront）やACMの挙動を理解するための基礎知識。

```
TCP/IP モデル
└ Transport Layer (TCP)
　└ SSL/TLS (暗号化プロトコル全体)
　 　└ Record Layer (共通の土台：パケットの細切れ・暗号化を担当)
　 　 　└ Sub-Protocols (用途別の4つの規約)
　 　 　 　├── Handshake Protocol (接続前の交渉)
　 　 　 　│　 ├── Client Hello (挨拶＆SNIの提示) ★
　 　 　 　│　 ├── Server Hello (返事＆暗号スイートの決定)
　 　 　 　│　 ├── Certificate (証明書送付：ACMなどで管理)
　 　 　 　│　 ├── Key Exchange (鍵交換：ECDHEなど)
　 　 　 　│　 └── Extensions (拡張フィールド)
　 　 　 　│　 　 ├── **SNI (Server Name Indication)**
　 　 　 　│　 　 ├── ALPN (HTTP/2などの利用確認)
　 　 　 　│　 　 └── Supported Groups (暗号強度の指定)
　 　 　 　│
　 　 　 　├── Alert Protocol (異常検知)
　 　 　 　│　 └── Record Layerを通じてエラーを通知
　 　 　 　│
　 　 　 　├── Change Cipher Spec Protocol (暗号化開始の合図)
　 　 　 　│　 └── 「次から暗号化するよ」という一歩手前のサイン
　 　 　 　│
　 　 　 　└── Application Data Protocol (実際のデータ)
　 　 　 　 　 └── HTTPS (HTTP on TLS) として流れる中身
```
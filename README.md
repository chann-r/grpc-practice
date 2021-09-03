# grpc-practice

### RFC（Remote Procedure Call）とは
- 外部のプログラムが提供するProcedureを呼び出す仕組み
- 異なる言語間でRFCとして呼び出す関数を定義するために、特定の言語に依らない定義方法（IDL：Interface Description Language）が必要になる
- IDLにはコンパイラが付属しており、サポートしている言語用に関数を呼び出すためのコードを自動生成できる

### gRPCについて

#### 概要
- 異なる言語で書かれたアプリケーション同士がgRPCにより自動生成されたインターフェースを通じて通信することが可能になる
- フロントは別マシーンのメソッドを簡単に呼び出せるようになるのでマイクロサービスを簡単に構築できる
- サーバー側はインターフェースを実装し、フロント側はサーバーと同じメソッドを提供するスタブを持つ

![grpc](https://user-images.githubusercontent.com/53222150/131676182-fab89e6d-6a45-4fb2-a7d6-ff14efb1321f.png)

#### 特徴
- RFCの実装の1つで、IDLにProtocol Buffersを採用
  - バイナリ形式のシリアライズフォーマットなので、JSONなどのようなテキスト形式より処理が軽い
- 通信プロトコルとしてHTTP2を採用
  - ストリーム処理を提供
    - クライアント・サーバー間のリクエストとレスポンスが1:1だけでなくN:Nも可能になる
    - 詳しい内容についてはこちらを参照
  　  https://qiita.com/tomo0/items/310d8ffe82749719e029j
- リクエストチェーン全体に渡るタイムアウトやキャンセル処理をプロトコルレベルでサポート
  - 複数のマイクロサービスを跨ぐリクエストを制御するのに長けている

#### 実装
- protoファイルにデータの構造やサービスを定義
- protoc（コンパイラ）で、シリアライズなどを含むデータアクセスするためのコードを好きな言語で自動生成
  - Protocol Buffersのメッセージやシリアライズに protocolbuffers/protobuf-go のprotoc-gen-go
  - gPRCのサーバ/クライアントに grpc/grpc-goのprotoc-gen-go-grpc

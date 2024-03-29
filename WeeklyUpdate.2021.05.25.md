**自由なカメラボイス、HP Omniceptのアイトラッキング、UIXとサムネイルの最適化**
皆さん、こんにちは！毎週恒例のアップデートをお届けします。

デスクトップの第一段階が完了したため、私たちは小さな追加、調整、バグ修正を行うことに集中しました。デスクトップの自由形式カメラでは、音声が自動的に出力されるようになり、ユーザーの視点やカメラの状態にアクセスするためのメモが追加されました。

HP Reverb G2 Omnicept Edition アイトラッキングのサポートを追加しました。このヘッドセットをお持ちの場合、視線、目の開き具合、瞳孔がアバターにマッピングされるようになりました。特に、音声や音楽の圧縮品質を向上させたOpusや、バグを修正してWOFFフォントフォーマットのサポートを追加したFreetypeなど、多くのネイティブ依存パッケージがAzure Pipelinesに移動し、最新バージョンに更新されました。

生活の質を向上させるために、LogiX自体に新しいノードが追加されました。特に、ベクターで動作するビットワイズマスクや、マテリアルのツールチップでマテリアルを別のタイプに一括変換できるようになりました。また、多数のバグやクラッシュを修正し、セキュリティを向上させました。詳細は以下の記事をご覧ください。

最後になりますが、セッションサムネイルシステムの大幅な最適化を行い、CPU、メモリ、ネットワーク帯域幅の使用量を削減しました。また、UIXパネルに関連する最適化も行い、全体的に操作性が向上し、多くのマイクロスタッターが減少しました。



**ネオスのゲームライブストリーム**
前回の定期的な金曜日のライブストリームでは、コミュニティが作った楽しい世界に戻って、たくさんのゲームをプレイしました。レーストラックを走り回ったり、MurderXで犯人を突き止めたり、幽霊に話しかけてミームピクチャーを解明したりと、盛りだくさんの内容になっていますので、ぜひご覧ください。



**HP Omnicept アイトラッキングサポート**
HP Reverb G2 Omnicept Editionヘッドセットのリリースに伴い、SDKを統合し、このヘッドセットをお持ちの方に完全なアイトラッキングサポートを提供しています。これには、目の向き、目の開き具合（まぶたを閉じる）、瞳孔の大きさなどが含まれます。

私たちの一般的な入力システムのおかげで、あなたの側で何か特別なことをする必要はありません。ヘッドセットを持っていて、Omniceptランタイムがインストールされていれば、Vive Pro Eyeヘッドセットと同じように、Neosを起動して、すでに目が設定されているアバターを使うだけで、すぐに動作します。

現時点では、リップトラッキング機能のAPIは提供されていないようですが、開発者向けに提供されるようになれば、喜んで統合したいと考えています。

**自由形式カメラの音声出力とデスクトップの改良**
また、新しいデスクトップモードにいくつかの追加と改良を加えました。特に、「フリーフォームカメラ」では、他のユーザーが実際のアバターから十分に離れていて、カメラが近づくと、自動的にあなたの声が出力されるようになり、より簡単にコミュニケーションが取れるようになりました。

また、デスクトップの情報にアクセスするための新しいLogiXノードが追加され、実際のユーザーの視点がどこにあるのか、フリーカムがアクティブかどうか、ボイスが現在アクティブかどうかを知ることができます。

ビュービジュアルをカスタマイズしている場合は、他のアバターと同じように`AudioOutput`と`AvatarAudioOutputManager`を設定する必要がありますが、後者のコンポーネントの `IsViewVoice` フィールドをチェックすると、適切に起動されます。

また、ウィスパー・モードはフリーカム・モードでは動作しないことに注意してください。

**Opus（オーディオ・エンコーディング）とFreetype（WOFFフォントのサポート）の更新**
Azure Pipelinesへの移行を進めているおかげで、より多くのネイティブな依存関係を簡単に最新バージョンにアップグレードすることができました。移行したライブラリの中には、OpusとFreetypeがあり、それぞれオーディオエンコーディングとフォントデコーディングを担当しています。

Opusライブラリは、1.1.3から最新の1.3.1にアップグレードしました。このアップグレードには、数年分の改良とバグフィックスが含まれており、特に低ビットレートでのオーディオ品質の向上に役立ちます。このライブラリは、ネットワーク経由の音声や、オーディオ・ストリーミング機能を使ったデスクトップ・オーディオのエンコードに使用されます。

Freetypeライブラリは、テキスト・レンダリング・システムのフォント・ファイルをデコードするために内部的に使用されており、2.10.0から2.10.4にアップデートされました。これは主に、重要なセキュリティ・パッチを含むバグ修正を含んでいます。また、TTFやOTFと並んで、WOFFフォントフォーマットのサポートも追加されました。

現在、WOFF2は、Freetypeライブラリがサポートしているにもかかわらず、追加の依存関係を必要とするため、まだサポートされていませんが、最終的にはこれもサポートしたいと考えています。

**新しいLogiX演算子ノード**
ご要望にお応えして，LogiX ノードに新しい演算子やオーバーロードを追加し，特にベクターやビットマスクの扱いを容易にしました。ブールベクター（bool2、bool3、bool4）は、ビットシフトノードやビットローテーションノードと同様に、ビット単位の論理演算子で動作するようになりました。

int2，int3，int4，long3，long4などの整数ベクトルも，同様にビット演算をサポートするようになりました。任意のベクターを比較演算子で使用して、ブーリアンベクターを生成することもできます。例えば、2つの float3 値を比較すると bool3 となり、ベクトルの各要素が個別に比較されます。

**素材(マテリアル)の一括変換**
生活の質の向上のために、階層内のマテリアルを一括して別のタイプに変換する機能が追加されました。これまでは、マテリアルツールチップを使って1つのマテリアルを変換することができましたが、ユーザーはシーンやオブジェクト/アバター内の複数のマテリアルを変換する必要がある場合がよくありました。

Neosの最新バージョンでは、インスペクタ内のスロット参照をマテリアルツールチップでつかむだけで、コンテキストメニューに「すべてを変換...」という新しいオプションが表示されるようになりました。これを実行すると、その階層内のすべてのマテリアルが特定のタイプに変換されます。

また、クオリティ・オブ・ライフとして、階層内のすべてのマテリアルの抽出処理を元に戻すことができるようにしました。

**ミップマップ生成の制御**
2Dテクスチャのミップマップ生成をコントロールできるようになったことも、クオリティ・オブ・ライフの向上につながります。スカイボックスなど、ミップマップを必要としないとわかっているテクスチャについては、ミップマップを完全にオフにすることができ、VRAMの使用量を節約できます。

ミップマップが必要なテクスチャに対しては、どのリスケールアルゴリズムを使ってミップマップを生成するかを選択できるようになりました。現在、Bilinear、Box（デフォルト）、Lanczos3の中から選択できます。特に最後のフィルタは、特定のテクスチャや画像に有効で、遠くからでも、特にコントラストの強いディテールでよりシャープなビジュアルを得ることができます。



**アカウントとOAuth 2.0サイトの更新**
進行中のアカウントとOAuth 2.0のウェブサイトは、新しいチームメンバーであるProbablePrimeの好意により、少しだけペイントされ、より公式な外観を与えるために、Karelによってより良いドメインhttps://auth.neos.com に移動されました。

これに伴い、メインのAPIエンドポイントもapi.neos.comに移動し、より統一されたものになりました。サードパーティのアプリケーションをお持ちの方は、必ずアップデートしてください。古いエンドポイントはしばらくの間は機能しますが、将来的には廃止される予定です。

**UIXとサムネイルシステムの最適化**
近日公開予定のMTC Streamer roomのランダムなヒッチやスタッターを調査していたところ、セッションサムネイルシステムとUIXおよび関連するサブシステムにいくつかの根本的な問題があることがわかりました。

最新のビルドでは、いくつかの最適化を行いました。これにより、CPU、メモリ、ネットワークの使用量が削減され、よりスムーズに使用できるようになりました。サムネイルシステムの一部が削除され、特にセッションを開始したばかりのときに、システムリソースに過剰な負荷がかからないようになっています。

また、ヘッドレスサーバは、他のユーザやデフォルトのワールドサムネイルから取得したサムネイルの圧縮や再アップロードを行わないようになり、サムネイルはすべてローカルキャッシュに登録されます。

無効にしたUIXキャンバスでもバックグラウンドで計算していたため、不要なCPUやメモリを使用していたことがわかりました。最新のビルドでは、UIXキャンバスが再びアクティブになるまで、計算を延期します。これにより、非アクティブなダッシュ画面、閉じたダッシュ、コンテキストメニュー、ローカルに隠された/カリングされたインターフェイスがあっても、パフォーマンスへの影響は最小限に抑えられます。

その他の最適化としては、直接読み込まれたテクスチャ（サムネイルがその代表例）は、メタデータの計算が不要なためスキップされ、UIのサムネイルの更新も起動するまで延期されるなど、細かい最適化が行われています。

最新のビルドでは、マイクロスタッターが減少し、フレームレートが向上したという報告をすでに受けています。比較的小さな変更にもかかわらず、これらの変更が感じられていることを嬉しく思います

**テキストレイアウトの不具合とちらつきの修正**
多くの小さなバグフィックスの中で、（絞り込むのに数時間かかったとしても）注目すべきは、テキストレンダリングシステムのオートサイズレイアウトです。これは、2つの大きな問題が原因で発生したもので、どちらも修正されています。

この2つの問題が重なることで、自動サイズのテキストが変更される際にランダムにスパズアウトしたり、グリッチが発生したり、一部の単語が強制的に複数行に折り返されたりしていました。今回のバグフィックスにより、テキストレンダリングシステムはより強固になり、より信頼性の高い結果が得られるようになりました。

**お気に入りアバターのセキュリティ改善**
インベントリやお気に入りのアバターのセキュリティを向上させるために、ワールドでお気に入りのアバターをロードするメカニズムを再構築しました。レコードが公開され、正確なIDを知っている人なら誰でもアクセスできる可能性がある代わりに、一時的なワンタイムキーが生成されます。

これは、リッピングの可能性を完全になくすものではありませんが（残念ながら技術的に完全に防ぐことはできません）、ある種の攻撃の可能性を大幅に軽減し、アバターの記録を公開しないことで、より良いセキュリティ衛生を提供することができます。

新しいシステムが普及すると同時に、アバターが公開フォルダに存在しない限り、それらのアバターの公開フラグを自動的に解除する予定です。

**様々なクラッシュのバグフィックスとユニコードの扱いの改善**
先週行われたその他の注目すべきバグフィックスには、多数のセキュリティ改善と、クラッシュやフリーズの原因となるものが含まれます。あまり一般的ではないいくつかのタイプのネットワークエンコーディングのリグレッションにより使用時にワールドがクラッシュするようになっていた問題が修正されました。

もう一つの注目すべき修正は、クリップボードからの無効なUnicode文字列の貼り付けに関するもので、例えば 🔆 のような特定の記号のためのUnicode文字サロゲートの半分を削除する場合などです。これまではこの操作を行うとNeosが再起動するまでフリーズしていましたが、これはこの操作を行ったユーザーにのみ影響し、攻撃には使用できません。

これと並行して、Neosのテキスト編集におけるUnicode文字列の扱いを改善し、サロゲートペアを認識して2つの独立したシンボルではなく、1つのシンボルとして扱うようになりました。

bobool3ol

No two bobool3ol's are the same. They're always 7.


**今後の予定**
小規模な作業、改善、バグ修正が一通り終わり、デスクトップサポートの第一段階がほぼ完了したので、次の大きな開発作業に取り組む前にリセットしてリフレッシュするために、前回のウィークリーアップデートで述べたように、これから約一週間の休暇を取ることにしました。

このため、残念ながら次の週はNeosの更新、質問や問題への回答（いずれにしても私からの）、そして来週の週刊誌の更新は、緊急の場合を除いて行われません。困ったことがあれば、他のチームメンバーに連絡することはできます。

皆さまにおかれましては、良い一週間をお過ごしください。そしていつも通り、皆さんのサポートに感謝しています。皆様のご協力がなければ、私たちは日々Neosを改善し続けることができません。またすぐにお会いしましょう、もっと大きな仕事や機能に取り組むために!

予定されていることはGitHubのロードマップで確認できますし、最新の開発状況は公式のDiscordサーバーで見ることができます。

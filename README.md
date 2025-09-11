# 熊剣迷路問題 revisited

<!-- Time-stamp: "2025-09-11T09:06:06Z" -->

まず Gist で公開したものですが、AI が読みやすいよう HTML 化するため、このレポジトリを作りました。自動 HTML 化のワークフローは Gemini 2.5 Flash さん製です。

レポジトリは↓  
https://github.com/JRF-2018/langchain_maze

HTML化したものは↓  
https://jrf-2018.github.io/langchain_maze/


## 説明

それぞれの IPYNB に説明がありますが、基本的には↓をお読みください。

《熊剣迷路問題 revisited。2023年6月から7月に行った LLMにごく簡単な迷路ゲームを解かせる実験。2025年の今、進化したツール類を使って再挑戦。  - JRF のひとこと》  
http://jrf.cocolog-nifty.com/statuses/2025/07/post-36be53.html

《LLM のメモリ機能の試験実装を行った。かなり本格的な実装(のモックアップ)になるようこころがけた。実装できたことはできたのだが、いまいち Gemini さんは積極的に使ってくれなかった。そこはアテが外れた。 - JRF のひとこと》  
http://jrf.cocolog-nifty.com/statuses/2025/08/post-881b46.html

《「LLM のメモリ機能を強制的に使うバンディットマシンの試験実装」と「LLM のメモリ機能とバンディット機能の試験実装」を行った。後者がメインの成果物で、メモリ機能の使用増加をどう強制するかから拡張したフレームワーク。 - JRF のひとこと》  
http://jrf.cocolog-nifty.com/statuses/2025/09/post-8225e2.html


## ファイル集

### 熊剣迷路問題 revisited

  * [《Gemini で簡単な迷路ゲームを教師付きで無理やりクリアさせてみた》](gemini_maze_0_0_1.ipynb)。前の BardAPI を使ったものを単に Gemini API を使わせてやらせてみた。簡単にゴールできた。これは[前回](http://jrf.cocolog-nifty.com/statuses/2023/07/post-619804.html)までの成果で、今回本来は公開する予定はなかったので、整理されてないのはご容赦ください。

  * [《LangChain で熊剣迷路問題 Version 0.0.1 (vertexai)》](langchain_maze_vertexai_0_0_1.ipynb)。とにかく create_tool_calling_agent を使って動くところまでやってみた。ゴールはできなかった。

  * [《LangChain で熊剣迷路問題 Version 0.0.1》](langchain_maze_0_0_1.ipynb)。とにかく create_tool_calling_agent を使って、LangChain で迷路が「偶然」ゴールできるところまでやってみた。あまりうまくいかないのは memory にツールコールの情報がないからか。

  * 《LangChain で熊剣迷路問題 Version 0.0.2》。まったくうまくいかない試みを繰り返していたので欠番。以降、0.0.3 からはきちんとゴールしてから公開している。

  * [《LangChain で熊剣迷路問題 Version 0.0.3》](langchain_maze_0_0_3.ipynb)。1 invoke につき 1 command を実現。command については、tool として提供するのではなく invoke の Final Answer として受け取るように変えてた。このようにすれば、invoke のときの行動データが自然に memory にたまるようになるはず。

  * [《LangChain で熊剣迷路問題 Version 0.0.4》](langchain_maze_0_0_4.ipynb)。1 invoke につき 1 command を実現するとしても、やはり、tool は tool call で呼んで欲しい。そこで、行動はすぐに行動ではなく、「行動予約」として登録し、invoke のあとそれを実行するようにした。

  * [《LangChain で熊剣迷路問題 Version 0.0.5》](langchain_maze_0_0_5.ipynb)。LangGraph 系の create_react_agent を使って実装。メモリにツールコールが残るため 1 invoke につき多 command でよいとした。これが現在の「決定版」。

  * [《LangChain で熊剣迷路問題 Version 0.0.6》](langchain_maze_0_0_6.ipynb)。1 invoke につき 1 command というのを stream という機能を使って実装してみた。それをする意味はあまりないが LangGraph 系ではそれが簡単にできることだけ示した。


0.0.3 から 0.0.6 までは、どれも「間違った方法」ではないというのが私の認識。それぞれに良さがあり、バージョンナンバーが上だからといって、発展形ではない。

一つだけ選べと言われたら 0.0.5 が今なら素直であると思う。しかし、0.0.3 や 0.0.4 や 0.0.6 のような方法で実装するほうがメモリの扱いなどにおいて適切な場合はあると思われる。

あと、追加の実験として↓を行った。

### メモリ機能の試験実装

  * [《LLM のメモリの試験実装 Version 0.0.1》](experimental_memory_0_0_1.ipynb)。かなり本格的なメモリ機能を試験実装してみた。とりあえずの試験実装にどういう例がいいか悩んだのだが、熊剣迷路問題を結局は使うことにしたので、このページに並べておくことにする。LangChain で熊剣迷路問題 version 0.0.7 相当という位置付け。

  * [《LLM のメモリの試験実装 Version 0.0.2》](experimental_memory_0_0_2.ipynb)。`update_plan` や `get_full_map` というメモリ機能で実現できるはずのツールを削り、メモリ機能を代わりに使うよう促してみた。が、うまくいかなかった。LangChain で熊剣迷路問題 version 0.0.8 相当という位置付け。

### バンディット機能の試験実装

  * [《LLM のメモリ機能を強制的に使うバンディットマシンの試験実装 Version 0.0.1》](experimental_bandit_0_0_1.ipynb)。メモリ機能をなかなか使ってくれないので悩んでいたところ、ChatGPT さんが記憶操作のバンディットマシンを作るのを提案してくれた。それを実装してみたもの。LangChain で熊剣迷路問題 version 0.0.9 相当という位置付け。

  * [《LLM のメモリ機能とバンディット機能の試験実装 Version 0.0.1》](experimental_bandit_tool_0_0_1.ipynb)。バンディットに特定のツールを登録するツールみたいなのを用意して、どういうツールを強制して数を増やして欲しいかを AI さん自身が決められると良いのかも…というアイデアを実装したもの。LangChain で熊剣迷路問題 version 0.0.10 相当という位置付け。

  * 《LLM のメモリ機能とバンディット機能とワークフロー機能の試験実装 Version 0.0.1》。MemoryBanditWorkflow を分ける前の仮実装のため欠番。

  * [《LLM のメモリ機能とバンディット機能とワークフロー機能の試験実装 Version 0.0.2》](experimental_bandit_workflow_0_0_2.ipynb)。ワークフローも定義できるようにしてみた。これまでの `bandit_list` を `workflow:main` のようにする実装で自然な拡張になっていると思う。LangChain で熊剣迷路問題 version 0.0.12 相当という位置付け。

## Author

JRF ( http://jrf.cocolog-nifty.com/statuses , Twitter (X): @jion_rockford )


## License

基本短いコードなので(私が作った部分は)パブリックドメインのつもりです。気になる方は MIT License として扱ってください。

かなり AI さん達(Gemini さんや Claude さんや ChatGPT さんや Grok さん)に教わって作っています。

----
(This document is mainly written in Japanese/UTF8.)

# Bear-Sword Maze Problem Revisited (with MemoryBanditWorkflow)
# 熊剣迷路問題 revisited

<!-- Time-stamp: "2026-02-12T18:55:35Z" -->

**Note to English speakers: This repository now includes English documentation and notebook versions. Please see the English Section at the bottom of this page for details.**

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

《MemoryBanditWorkflow (参: \[cocolog:95619779\](2025年9月)) を Claude Code などの SKILL.md などを利用して実現するにはどうすればいいか。試みに Claude さん自身に聞いてみた。 - JRF のひとこと》  
http://jrf.cocolog-nifty.com/statuses/2026/01/post-b86e58.html


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

  * [《LLM のメモリ機能とバンディット機能とワークフロー機能の試験実装 Version 0.0.2》](experimental_bandit_workflow_0_0_2.ipynb)。ワークフローも定義できるようにしてみた。これまでの `bandit_list` を `workflow:main` のようにする実装で自然な拡張になっていると思う。MemoryBanditWorkflow 登場。LangChain で熊剣迷路問題 version 0.0.12 相当という位置付け。


### MemoryBanditWorkflow を使った RAG エージェントの試験実装

  * [《MemoryBanditWorkflow を使った RAG の試験実装 Version 0.0.1》](experimental_rag_0_0_1.ipynb)。MemoryBanditWorkflow は汎用に作ったつもりなので、それを他に利用できることを実証するため、簡単な RAG エージェントを作ってみた。熊剣迷路問題からは離れるが…。LangChain で熊剣迷路問題 version 0.0.13 相当という位置付け。

  * [《MemoryBanditWorkflow を使った RAG の試験実装 Version 0.0.2》](experimental_rag_0_0_2.ipynb)。0.0.1 からは RAG のループを少し変えて、一章ごと取材・執筆をするように指示を出し、バンディットが呼ばれやすくならないか試してみた。MemoryBanditWorkflow は変えてない。LangChain で熊剣迷路問題 version 0.0.14 相当という位置付け。


### LangChain 1.x 系への対応とサブツールの導入

langchain_maze と experimental_rag を LangChain 1.x 系に対応させ、さらにはやりの skills やツールボックスなどの導入に相当する subtool_do の導入も行った。

  * [《LangChain 1.x 系で熊剣迷路問題》](langchain_maze_0_0_15.ipynb)。LangChain 1.x 系に対応し、subtool_do と subtool_show を導入した。LangChain で熊剣迷路問題 version 0.0.15 相当という位置付け。

  * [《MemoryBanditWorkflow を使った RAG の試験実装 (LangChain 1.x系)》](experimental_rag_0_0_16.ipynb)。上で変更された MemoryBanditWorkflow でも RAG させてみた。LangChain で熊剣迷路問題 version 0.0.16 相当という位置付け。


### 英語への翻訳

英語版もいちおう作ってみた。

  * [《Bear-Sword Maze Problem with LangChain 1.x》](langchain_maze_en_0_0_15.ipynb)。英語実装。熊剣迷路問題 version 0.0.15 相当という位置付けのまま。

  * [《Experimental RAG Implementation using MemoryBanditWorkflow (LangChain 1.x)》](experimental_rag_en_0_0_16.ipynb)。英語実装。LangChain で熊剣迷路問題 version 0.0.16 相当という位置付けのまま。


## English Version Overview

This section provides an overview of the English-language assets added in the latest versions (v0.0.15.x - v0.0.16.x).

### Core Concept: MemoryBanditWorkflow

MemoryBanditWorkflow is an agent framework designed to tackle the limitations of "stateless" LLM APIs. It incorporates:

  * **Forced Memory Utilization**: Using a Bandit approach to encourage the agent to use memory search tools.

  * **State-Machine Driven Workflows**: Allowing agents to follow structured phases (e.g., Planning -> Research -> Writing -> Verification).

  * **Sub-tools (On-demand Skills)**: A unique architecture where only a `subtool_do command` is in the initial context, and the agent "discovers" detailed skill documentation via `subtool_show` (similar to reading a `SKILL.md`).

### RagAgent: Autonomous Thesis Writing

In the latest update, we demonstrate the versatility of the framework by implementing `RagAgent`. This agent performs autonomous RAG (Retrieval-Augmented Generation) to:

  1. Research a complex topic (e.g., "Japan's Lost Decades") via web search.

  2. Store findings in a simulated database (spoofed by the LLM).

  3. Write a structured, multi-chapter thesis in Markdown.

English Files

  * [《Bear-Sword Maze Problem with LangChain 1.x》](langchain_maze_en_0_0_15.ipynb): The foundational maze-solving agent with English prompts and interface.

  *  [《Experimental RAG Implementation using MemoryBanditWorkflow (LangChain 1.x)》](experimental_rag_en_0_0_16.ipynb): The complete English implementation of the RAG thesis-writing agent.


## Author

JRF ( http://jrf.cocolog-nifty.com/statuses , Twitter (X): @jion_rockford )

## License (in Japanese)

基本短いコードなので(私が作った部分は)パブリックドメインのつもりです。気になる方は MIT License として扱ってください。

かなり AI さん達(Gemini さんや Claude さんや ChatGPT さんや Grok さん)に教わって作っています。

## License (in English)

Since the code is relatively short, I intended for my parts to be in the Public Domain. If you have concerns, please treat it under the MIT License.

This was developed with significant guidance from various AIs (Gemini, ChatGPT, Claude, and Grok).

----
(This document is mainly written in Japanese/UTF8.)

{ "category": "React",  "order": 0, "date": "2024-03-09 15:30" }
---
# Reactでrefオブジェクトのcurrentがnullの時確認すること

refオブジェクトを複数のコンポーネントを経由している場合、途中で`props.ref`にセットしていないか確認する。
その場合、途中では`props.ref`にセットしないようにする。
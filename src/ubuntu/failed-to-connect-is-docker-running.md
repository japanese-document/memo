{ "category": "Ubuntu",  "order": 1, "date": "2024-04-01 19:10" }
---
# VSCodeでDockerに接続することができない

VSCodeにDocker extensionをインストールすると以下のメッセージが表示されました。

```
Failed to connect. Is Docker running?
```

[この手順](https://docs.docker.com/engine/install/linux-postinstall/)を実行してrootでなくてもDockerを実行できるようにして解決しました。
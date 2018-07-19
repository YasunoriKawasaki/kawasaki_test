# Filestoreインスタンスの作り方
`$ gcloud beta filestore instances create nfs-server --location=us-central1-a --file-share=capacity=1TB,name=wordpress --tier=STANDARD --network=name="default"`

現在、東京リージョンでは作成できない。
# アーキテクチャ
図を書くのが辛かったので一旦文章で。
各PodにはwordpressのコンテナとCloudSQL proxyのためのコンテナの、1Pod:2Container構成である。

wordpressコンテナはMySQLへ接続する際には同Pod内のSQL proxyコンテナへ接続しにいく。

KubernetesからCloudSQLへ接続する詳細なドキュメントは[こちら](https://cloud.google.com/sql/docs/container-engine-connect?hl=ja)
# Known Issue
nfs-server内のディレクトリアクセス権限を777とかにしないと、wordpressのPodがコンテンツをnfsに保存しにいくことができない。
一旦適当なGCEにマウントして、`chmod -R 777 wp-content/` とかしたけど、絶対こんなことしなくても良さそうなので、なんとかしてほしい。
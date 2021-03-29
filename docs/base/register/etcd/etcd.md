## etcd


获取所有/keys开头的key有哪些
./etcdctl --endpoints=ip:2379  get '/keys' --prefix   --keys-only

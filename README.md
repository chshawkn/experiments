# experiments
Repository for experimental code

```bash
cd workspace

# build v2ray
mkdir -p v2ray-experiments/git/src/v2ray.com
cd v2ray-experiments/git/src/v2ray.com
# in v2ray-experiments/git/src/v2ray.com
git clone https://github.com/v2ray/v2ray-core.git
mv v2ray-core core
go install v2ray.com/core/tools/build
go get github.com/golang/protobuf/proto
go get github.com/gorilla/websocket
go get github.com/miekg/dns
brew install privoxy
echo -e "
    forward-socks5 / 127.0.0.1:7071 .
    listen-address 127.0.0.1:8118
    # local network do not use proxy
    forward 192.168.*.*/ .
    forward 10.*.*.*/ .
    forward 127.*.*.*/ .
    " > privoxy.conf
privoxy privoxy.conf
http_proxy="http://127.0.0.1:8118" go get golang.org/x/crypto/chacha20poly1305
../../bin/build
../../bin/v2ray-custom-macos/v2ray -config=$TEST_DIR/v2ray_doko_vmess.json

# build benchmark
mkdir -p v2ray-experiments/git/src/github.com/v2ray
cd v2ray-experiments/git/src/github.com/v2ray
# in v2ray-experiments/git/src/github.com/v2ray
git clone git@github.com:chshawkn/experiments.git

cd experiments/benchmark/testcases
# in v2ray-experiments/git/src/github.com/v2ray/experiments/benchmark/testcases
# curl -O https://install.direct/go.sh
source env.sh
./build.sh
../../../../../../bin/receiver -port=10001
../../../../../../bin/loadgen -remoteport=10001 -port=1081 -amount=1 -concurrency=1 -type=socks
```

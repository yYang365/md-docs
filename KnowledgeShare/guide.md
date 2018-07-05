yingu-blockchain  
yingu private block chain (ygpbc)
# 谷链项目

## 创建私链

### 创建自定义生成块JSON文件并为区块链数据创建目录后，在可以访问geth的控制台中键入以下命令：

> From <http://ethdocs.org/en/latest/network/test-networks.html#setting-up-a-local-private-testnet> 

```
    geth --identity "YinGuPBC" --rpc --rpcport "8102" --rpccorsdomain "*" 
    --datadir "E:\Users\YinGu\BlockChain\.ethereum" --port "20183" --nodiscover --rpcapi "db,eth,net,web3"
    --networkid 1803 init "E:\Users\YinGu\BlockChain\ygpbc-genesis.json"
```

> From <http://www.ethdocs.org/en/latest/network/test-networks.html#setting-up-a-local-private-testnet> 

* --identity "YinGuPBC"  
    为节点设置身份，以便在对等列表中更轻松地识别它。

* --rpc  
    在节点上启用RPC接口。通常在Geth中默认启用。

* --rpcport "8102"  
    将8000更改为网络上打开的任何端口。geth的默认值是8080。

* --rpccorsdomain "http://chriseth.github.io/browser-solidity/"  
    这决定了哪些URL可以连接到节点以执行RPC客户端任务。要非常小心，输入一个特定的URL而不是通配符（*），这将允许任何URL连接到RP​​C实例。

* --datadir "E:\Users\YinGu\BlockChain\.ethereum"  
    这是私人连锁数据将被存储在的数据目录（在nubits。选择与公共以太坊链文件夹不同的位置。

* --port "20183"  
    这是“网络侦听端口”，将使用它来手动与其他对等端进行连接。

* --nodiscover  
    使用它可以确保节点不会被没有手动添加的人发现。否则，如果节点具有相同的生成文件和网络标识符，则可能无意中将其添加到陌生人的区块链中。

* --rpcapi "db,eth,net,web3"  
    这决定了允许通过RPC访问哪些API。默认情况下，Geth通过RPC启用web3界面。
    重要提示：请注意，通过RPC / IPC接口提供API将使每个人都可以访问可以访问此接口的API（例如dapp's）。小心启用了哪些API。默认情况下，geth通过IPC接口启用所有API，并通过RPC接口启用db，eth，net和web3 API。

* --maxpeers 0  
    如果您不希望其他人连接到测试链，请使用maxpeers 0。或者，如果确切知道要连接到节点的对等点的数量，则可以调整此数字。

> From <http://ethdocs.org/en/latest/network/test-networks.html#setting-up-a-local-private-testnet> 

```
E:\Users\YinGu>geth --identity "YinGuPBC" --rpc --rpcport "8102" --rpccorsdomain "*" --datadir "E:\Users\YinGu\BlockChain\.ethereum" --port "20183" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1803 init "E:\Users\YinGu\BlockChain\ygpbc-genesis.json"
INFO [03-08|18:23:36] Maximum peer count                       ETH=25 LES=0 total=25
INFO [03-08|18:23:36] Allocated cache and file handles         database=E:\\Users\\YinGu\\BlockChain\\.ethereum\\geth\\chaindata cache=16 handles=16
INFO [03-08|18:23:36] Writing custom genesis block
INFO [03-08|18:23:36] Persisted trie from memory database      nodes=3 size=505.00B time=0s gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [03-08|18:23:36] Successfully wrote genesis state         database=chaindata                                                hash=a3c5c1…d6926b
INFO [03-08|18:23:36] Allocated cache and file handles         database=E:\\Users\\YinGu\\BlockChain\\.ethereum\\geth\\lightchaindata cache=16 handles=16
INFO [03-08|18:23:36] Writing custom genesis block
INFO [03-08|18:23:36] Persisted trie from memory database      nodes=3 size=505.00B time=499.8µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [03-08|18:23:36] Successfully wrote genesis state         database=lightchaindata
```

### 通过控制台与geth进行交互，请输入：

```
    geth --identity "YinGuPBC" --rpc --rpcport "8102" --rpccorsdomain "*" --datadir "E:\Users\YinGu\BlockChain\.ethereum" 
    --port "20183"  --nodiscover --rpcapi "db,eth,net,web3" --networkid 1803 console
```

> From <http://www.ethdocs.org/en/latest/network/test-networks.html#setting-up-a-local-private-testnet> 

```
E:\Users\YinGu>geth --identity "YinGuPBC" --rpc --rpcport "8102" --rpccorsdomain "*" --datadir "E:\Users\YinGu\BlockChain\.ethereum" --port "20183" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1803 console
INFO [03-08|18:35:37] Maximum peer count                       ETH=25 LES=0 total=25
INFO [03-08|18:35:37] Starting peer-to-peer node               instance=Geth/YinGuPBC/v1.8.1-stable-1e67410e/windows-amd64/go1.9.2
INFO [03-08|18:35:37] Allocated cache and file handles         database=E:\\Users\\YinGu\\BlockChain\\.ethereum\\geth\\chaindata cache=768 handles=1024
WARN [03-08|18:35:37] Upgrading database to use lookup entries
INFO [03-08|18:35:37] Database deduplication successful        deduped=0
INFO [03-08|18:35:37] Initialised chain configuration          config="{ChainID: 0 Homestead: 0 DAO: <nil> DAOSupport: false EIP150: <nil> EIP155: 0 EIP158: 0 Byzantium: <nil> Engine: unknown}"
INFO [03-08|18:35:37] Disk storage enabled for ethash caches   dir=E:\\Users\\YinGu\\BlockChain\\.ethereum\\geth\\ethash count=3
INFO [03-08|18:35:37] Disk storage enabled for ethash DAGs     dir=C:\\Users\\yYang\\AppData\\Ethash                     count=2
INFO [03-08|18:35:37] Initialising Ethereum protocol           versions="[63 62]" network=1803
INFO [03-08|18:35:37] Loaded most recent local header          number=0 hash=a3c5c1…d6926b td=131072
INFO [03-08|18:35:37] Loaded most recent local full block      number=0 hash=a3c5c1…d6926b td=131072
INFO [03-08|18:35:37] Loaded most recent local fast block      number=0 hash=a3c5c1…d6926b td=131072
INFO [03-08|18:35:37] Regenerated local transaction journal    transactions=0 accounts=0
INFO [03-08|18:35:37] Starting P2P networking
INFO [03-08|18:35:37] RLPx listener up                         self="enode://30432034a34f8b8d1bba079daa9bdff94f79161e6cf8d80180f10dfdcee232c8d95b65722841d9892429b7b15abccc94d41a3289d6081bfa6a89dced13332717@[::]:20183?discport=0"
INFO [03-08|18:35:37] IPC endpoint opened                      url=\\\\.\\pipe\\geth.ipc
INFO [03-08|18:35:37] HTTP endpoint opened                     url=http://127.0.0.1:8102 cors=* vhosts=localhost
Welcome to the Geth JavaScript console!

instance: Geth/YinGuPBC/v1.8.1-stable-1e67410e/windows-amd64/go1.9.2
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

>
```
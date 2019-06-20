# API开发接入

## 搭建非见证人节点

Cybex非见证人节点可以在Linux系统上架设。如果您使用的是64位的Linux，您可以使用我预编译的程序启动节点。在您启动节点前，需要遵循以下步骤。此外，您也可以选择从源代码自行编译。

### 安装预编译版本

**第一步** 编译您的工作目录。

```Bash
export Cybex_ROOT_DIR=${PATH TO YOUR NEWLY BUILT DIRECTORY}
```

**第二步** 编译子目录。

```Bash
cd $Cybex_ROOT_DIR
mkdir bin
mkdir data
```

**第三步** 下载预编译好的节点、钱包文件至bin目录

```Bash
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/bin/witness_node -O bin/witness_node
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/bin/cli_wallet -O bin/cli_wallet
md5sum witness_node # will output "c5d10757a7d913eb88f162ba5318a80e  witness_node"
md5sum cli_wallet # will output "3a68465978b81690fc3c6849616cfff4  cli_wallet"
```
**第四步** 下载Download genesis.json，config.ini。

如果您连接的是测试链

```Bash
cd $Cybex_ROOT_DIR
wget https://raw.githubusercontent.com/CybexDEX/how-to-run-Cybex-node/master/testchain/genesis.json -O genesis.json
wget https://raw.githubusercontent.com/CybexDEX/how-to-run-Cybex-node/master/testchain/config.ini -O data/config.ini
```
如果您连接的生产链

```Bash
cd $Cybex_ROOT_DIR
wget https://raw.githubusercontent.com/CybexDEX/how-to-run-Cybex-node/master/mainchain/genesis.json -O genesis.json
wget https://raw.githubusercontent.com/CybexDEX/how-to-run-Cybex-node/master/mainchain/config.ini -O data/config.ini
```

您可以能会需要将bit转为二进制

```Bash
cd $Cybex_ROOT_DIR
chmod u+x bin/cli_wallet bin/witness_node
```

**第五步** 启动非见证人节点

```Bash
cd $Cybex_ROOT_DIR
./bin/witness_node -d data --genesis-json genesis.json --max-ops-per-account 500 --resync-blockchain --replay-blockchain
```

**注意事项**

如果您使用的是MAC系统，请使用以下代码

```Bash
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/mac-bin/witness_node -O bin/witness_node
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/mac-bin/cli_wallet -O bin/cli_wallet
```
校验二进制总和

```Bash
$ md5sum witness_node
c8929ef1c61c8f060785552c2b059fd4   witness_node
534faa2613003a2b842554c0aacdf692   cli_wallet
```

### 节点插件

#### account_history

提供账户历史记录相关查询

参数：  
**track-account** 
设置跟踪哪些账号，可以指定多次。例如track-account = "1.2.100"    
**partial-operations**
只有设置了track-account的账号的相关操作保存。例如partial-operations=true    
**max-ops-per-account**
为每个账号保存多少条记录。 例如max-ops-per-account=500    

#### market_history

作用：提供市场行情历史信息查询

参数：  
**bucket-size**
设置跟踪哪些k线，以秒为计时单位，可设置多个。例如bucket-size = [60,300] 表示追踪1分钟和5分钟k线    
**max-order-his-records-per-market** 
设置为每个交易对最多保存多少条成交记录， 默认1000条。    
**max-order-his-seconds-per-market**
设置为每个交易对最多保存多少秒的成交记录，默认259200秒。    

#### snapshot    

作用：提供数据库全量快照   
 
参数：  
**snapshot-at-block**
设置在第几个区块时快照    
**snapshot-at-time**
设置在哪个时刻快照    
**snapshot-to**
设置快照文件的文件名    

#### delayed_node
  
作用：提供基于不可逆区块的数据库查询功能

参数：  
**trusted-node**
设置上连节点作为区块获取节点。例如trusted-node="127.0.0.1:8090"

### 连接测试节点

Cybex的测试链，开放以下两个测试节点供开发者社区调用：

1. Hangzhou

wss://hangzhou.51nebula.com/

2. Shenzhen

wss://shenzhen.51nebula.com/

## JSON-RPC API连接

Cybex中任意节点包含两种API接⼝，所有API数据均使用JSON-RPC2.0形式。可以选择使用HTTP或Websocket⽅式进⾏访问。两种方式的params参数结构均为：

`
[API-identifier, Method-to-Call, Call-Parameters]
`

### HTTP调用

HTTP⽅式仅允许只读的查询数据库，如: 

``` 
$ curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts", [["1.2.0"]]], "id": 1}' http://127.0.0.1:8090/rpc
```

返回结果

```  
{
	"id": 1,
	"result": [{
		"id": "1.2.0",
		"annotations": [],
		"membership_expiration_date": "1969-12-31T23:59:59",
		"registrar": "1.2.0",
		"referrer": "1.2.0",
		"lifetime_referrer": "1.2.0",
		"network_fee_percentage": 2000,
		"lifetime_referrer_fee_percentage": 8000,
		"referrer_rewards_percentage": 0,
		"name": "committee-account",
		"owner": {
			"weight_threshold": 1,
			"account_autos": [],
			"key_auths": [],
			"address_auths": []
		},
		"active": {
			"weight_threshold": 6,
			"account_auths": [
				["1.2.5", 1],
				["1.2.6", 1],
				["1.2.7", 1],
				["1.2.8", 1],
				["1.2.9", 1],
				["1.2.10", 1],
				["1.2.11", 1],
				["1.2.12", 1],
				["1.2.13", 1],
				["1.2.14", 1]
			],
			"key_auths": [],
			"address_auths": []
		},
		"options": {
			"memo_key": "GPH1111111111111111111111111111111114T1Anm",
			"voting_account": "1.2.0",
			"num_witness": 0,
			"num_committee": 0,
			"votes": [],
			"extensions": []
		},
		"statistics": "2.7.0",
		"whitelisting_accounts": [],
		"blacklisting_accounts": []
	}]
}
```

### Websocket调用

Websocket⽅式需要登录（链上执行⼴播需要会话信息）并可执行查询和修改操作。wscat调用示例：

```
$ npm install -g wscat
$ wscat -c ws://127.0.0.1:8090   
> {"id":1, "method":"call", "params":[0,"get_accounts",[["1.2.0"]]]}  
```

返回结果

```  
{
	"id": 1,
	"result": [{
		"id": "1.2.0",
		"annotations": [],
		"membership_expiration_date": "1969-12-31T23:59:59",
		"registrar": "1.2.0",
		"referrer": "1.2.0",
		"lifetime_referrer": "1.2.0",
		"network_fee_percentage": 2000,
		"lifetime_referrer_fee_percentage": 8000,
		"referrer_rewards_percentage": 0,
		"name": "committee-account",
		"owner": {
			"weight_threshold": 1,
			"account_autos": [],
			"key_auths": [],
			"address_auths": []
		},
		"active": {
			"weight_threshold": 6,
			"account_auths": [
				["1.2.5", 1],
				["1.2.6", 1],
				["1.2.7", 1],
				["1.2.8", 1],
				["1.2.9", 1],
				["1.2.10", 1],
				["1.2.11", 1],
				["1.2.12", 1],
				["1.2.13", 1],
				["1.2.14", 1]
			],
			"key_auths": [],
			"address_auths": []
		},
		"options": {
			"memo_key": "GPH1111111111111111111111111111111114T1Anm",
			"voting_account": "1.2.0",
			"num_witness": 0,
			"num_committee": 0,
			"votes": [],
			"extensions": []
		},
		"statistics": "2.7.0",
		"whitelisting_accounts": [],
		"blacklisting_accounts": []
	}]
}
```  

###发起API请求

Cybex的API分为两类：

1. 查询操作，可以直接访问
2. 修改操作，需要通过广播交易broadcast_transaction方式进行，⼴播交易时，需要进行签名。

进⾏查询时，可以使⽤curl的普通HTTP形式访问节点端⼝下的/rpc路路径来执⾏，或者使用辅助库的工具类来进行，工具类使用Websocket方式连接节点。

* 发起API请求

Cybex节点提供了许多可以通过Websocket进行连接的API，包括：Database API、Account History API、Network Broadcast API、Network Nodes API。

请求API的具体步骤如下：

1. 登录全节点
2. 获得API访问权限
3. 获取API标识
4. 提供获取的标识，调用某个具体的API方法
  
1. 登录

首先，我们需要完成登录操作:

```
> {"id":2,"method":"call","params":[1,"login",["",""]]}
< {"id":2,"result":true}
```

对于需要登录后才能调用的API，调用方需要填写用户名和密码。节点在验证通过后，将会返回登录成功的信息。

2. 获得API访问权限

数据库API提供了大部分常用的查询接口，以此为例，我们可以通过以下方式注册Database API：

```
> {"id":2,"method":"call","params":[1,"database",[]]}
```

3. 获取API标识

在发起连接请求后，全节点会驳回该请求或返回一个唯一标识：

```
< {"id":2,"result":2}
```

这个返回结果代表了我们之前对Database API发出请求被允许了，这个id被称为DATABASE API ID。

4. 提供获取的标识，调用某个具体的API方法

现在，我们就可以调用Database API的某个具体方法了:

```
> {"id":1, "method":"call", "params":[DATABASE_API_ID,"get_accounts",[["1.2.0"]]]}
```

* 广播交易

Cybex原生的操作（Operations）需要通过广播的方式执行，广播操作通过`broadcast_transaction`完成。

```
void graphene::app::network_broadcast_api::broadcast_transaction(const signed_transaction &trx)
```

其中，trx参数用来包裹Operation。

⼴播并执⾏⼀个交易，⼤致的步骤是：

1）构造交易体     
2）向交易体中添加操作     
3）添加签名     
4）⼴播交易     

前两个步骤是需要手动进行的，后两个步骤可以通过辅助库的工具类协助执行。

注册用户是一个常见的业务操作，我们以这个操作和JavaScript库为例，详细讲解一下广播的交易步骤。

首先，判断这是一个数据修改（新增）操作，所以需要通过广播交易`broadcast_transaction`方式进行。创建账户使用的是`account_create_operation`，因此，我们通过以下方式实现：

```
let tr = new TransactionBuilder(); // 构造一个交易体，TransactionBuilder可从Cybexjs库中获得。
tr.add_type_operation("account_create", { // 向交易中添加所需广播的操作，操作名称为 account_create.
	fee: { 
		amount: 0,
		asset_id: "1.3.0"
	}, // 该笔交易的手续费字段。
	registrar: chain_registrar.get("id"), // 注册人ID
	referrer: chain_referrer.get("id"), // 引荐人ID
	referrer_percent: referrer_percent, // 引荐人返利额度
	name: new_account_name, // 用户名
	owner: { // 用户的Owner权限设置
		weight_threshold: 1, // Owner权限的域限
		account_auths: [], // 占有权限的用户列表
		key_auths: [[owner_pubkey, 1]], // 占有权限的公钥列表
		address_auths: [] // 占有权限的地址列表
	},
	active: { // 活跃权限配置
		weight_threshold: 1,
		account_auths: [],
		key_auths: [[active_pubkey, 1]],
		address_auths: []
	},
	options: { //其他配置
		memo_key: active_pubkey, // Memo公钥，其他人向该用户发送Memo时，将使用该公钥进行加密
		voting_account: "1.2.5", // 默认投票代理
		num_witness: 0, // 初始投票的证人数量
		num_committee: 0, // 初始投票的委员会数量
		votes: [] //初始投票的ID集合
 	}
 });
 tr.add_signer(privKey); // 向交易添加签名，传入私钥
 await tr.broadcast(); // 广播交易，异步操作
```  

Cybex原生operation的具体参数描述，请参考使用手册相关章节。

###数据库通知

在Cybex中，通过websocket连接的方式监听数据库变更或事件发生。

Cybex运行通过以下方式进行订阅：

```
set_subscribe_callback( int identifier, bool clear_filter ): ```  

简单的全局监听回调可以通过该方法实现。

每个通知经由全节点初始化的通知都会具有一个唯一的ID，用户通过identifier参数为ID赋值。 
```
set_pending_transaction_callback(int identifier):
```
 _待确认_的事务回调。

``` subscribe_to_market(int identifier, asset_id a, asset_id b)):
```
 订阅市场上关于`a:b`交易对的变更并使用`identifier`的ID进行通知。

``` get_full_accounts(array account_ids, bool subscribe): ```

根据指定`account_ids`查询该账户所有相关信息，并通过将`subscribe`设置为`True`订阅该账户的更新。

让我们通过一个全局订阅的回调将通知与通常的RPC调用做一下区分：

```
> {"id":4,"method":"call","params":[DATABASE_API_ID,"set_subscribe_callback",[SUBSCRIPTION_ID, true]]}
```

上述调用需要将SUBSCRIPTION_ID注册为ID来获取通知.

自此，你从见证人节点获取的对象都会自动订阅该对象未来的所有变更。

在调用set_subscribe_callback方法之后，见证人节点会将该对象的每一次变更都以通知的形式发送给你：

```
< {
    "method": "notice"
    "params": [
        SUBSCRIPTION_ID,
        [[
            { "id": "2.1.0", ...  },
            { "id": ...  },
            { "id": ...  },
            { "id": ...  }
        ]]
    ],
}
```

**Session示例**

以下是一个完整seesion的示例:

```
> {"method": "call", "params": [1, "login", ["", ""]], "id": 2}
< {"id":2,"result":true}
> {"method": "call", "params": [1, "database", []], "id": 3}
< {"id":3,"result":2}
> {"method": "call", "params": [1, "history", []], "id": 4}
< {"id":4,"result":3}
> {"method": "call", "params": [2, "set_subscribe_callback", [5, false]], "id": 6}
< {"id":6,"result":null}
> {"method": "call", "params": [2, "get_objects", [["2.1.0"]]], "id": 7}
(plenty of data coming in from this point on)
```

###常见问题

1. 金额和价格如何表示？

Cybex网络的所有费用，均使用`整数`计数，即`amount`类型，其价值value（或价格price）根据以下公式计算:

**value = amount / 10^precision**

其中，_precision_ 为资产类型对应的精度。

2. 如何获取手续费信息？

在Cybex网络内，所有需要广播的操作都需要缴纳手续费，手续费用`fee`字段表示。消耗的手续费会部分燃烧，燃烧的手续费进入系统预算池。`fee`包含的参数如下：

| **参数** | **类型** | **描述** |
| --- | --- | --- |
| amount | share_type | 手续费金额 |
| asset_id | asset_id_type | 用来缴纳手续费的资产ID |

在添加operation时，可以先将`amount`设为0，然后通过调用database_api中的get_required_fees接口获取该交易所需的最低手续费。

一旦手续费不足，交易广播会失败。手续费可以填写等于或高于最低手续费的任意值，多出的手续费会被系统销毁。

3. 手续费如何计算？

Cybex网络内的所有操作，都需要向网络缴付手续费。手续费默认以CYB计算，对于不持有CYB的操作发起者，可以使用其他Cybex网络内的资产支付手续费，换算公式为：

**操作手续费 = 操作手续费(以CYB计) * 用来支付的资产与CYB的汇率**

其中，后者是资产创建时，与CYB之间设置的初始汇率，该汇率可以通过get_assets接口查询。

以一般转账为例，当前transfer_operation的费率为0.01 CYB，手续费希望以BAT计算。CYB/BAT的初始汇率为0.4，则`一般转账用BAT支付的收费 = 0.01 * 0.4 = 0.004 BAT`。

4. 为什么提现手续费高于普通转账手续费？

Cybex的提现操作，是指将账户余额从Cybex网络转出至外部网络（账户）。提现操作需要通过Cybex网关，完成内盘资产到外盘的汇兑。实际上，资金经由网关出入Cybex网络，都会产生服务手续费，只是一般情况下，网关免除了充值操作（从外盘转入Cybex内盘）的手续费，仅收取提现手续费。

通过Cybex网关，将从将Cybex账户余额提现至外部账户（地址）的手续费计算公式为

**提现手续费 = 网关手续费 + 提现操作手续费**

其中，前者由提供充提服务的网关收取，一般以提现资产计费。后者与其他Cybex网络内的操作一样，由Cybex网络收取，默认使用CYB支付。如果提现账户未持有CYB，则可以选择使用提现资产进行支付。

注：提现操作手续费会因为备注内容的字数而增加，每千字节手续费为0.00555 CYB。如以提现币种计，则按以上规则换算。


##Python库

### 安装

使用pip安装，支持python3
`pip install PyCybex`


### 基本使用
```python
import cybex

NODE_URL = 'ws://127.0.0.1:8090'
ACCOUNT_NAME = 'nathan'

instance = cybex.Cybex(NODE_URL)
```

使用时需要配置连接的node的url和账户名以初始化对象。

### 获取行情信息
```python
import cybex
...
ASSET_NAME = 'JADE.ETH'

# 创建两个资产对象
base = cybex.asset.Asset('JADE.ETH', cybex_instance = instance)
quote = cybex.asset.Asset('CYB', cybex_instance = instance)

# 创建市场对象
market = cybex.market.Market(base = base, quote = quote, cybex_instance = instance)

##### 最新tick #####
# 每次调用market对象的tick函数，会去链上查询最新的tick。
t = market.tick()

##### 深度行情 #####
# 每次调用market对象的orderbook函数，会去链上查询最新的orderbook。
order_book = market.orderbook()

# 获取卖单队列
for ask_order in order_book['asks']:
    # 订单价格，为以base计价的quote的价值
    print(ask_order['price'])
    # 卖单中，出售quote资产的数量
    print(ask_order['quote'].amount)
    # 卖单中，收购base资产的数量
    print(ask_order['base'].amount)

# 获取买单队列
for bid_orders in order_book['bids']:
    # 订单价格，为以base计价的quote的价值
    print(ask_order['price'])
    # 买单中，收购quote资产的数量
    print(ask_order['quote'].amount)
    # 买单中，出售base资产的数量
    print(ask_order['base'].amount)

##### 获取K线 #####
# 赛贝是一个全球部署的去中心化交易所，系统使用UTC时间，
# 获取K线需要使用UTC时间
# 以下例子获取开始时间在UTC时间2018-08-31日，5:59:00到6:01:00之间的3600秒k线，
# 将会返回UTC时间2018-08-31 06:00:00的小时K线
from datetime import datetime
start = datetime.strptime('2018-08-31 05:59:00', '%Y-%m-%d %H:%M:%S')
end = datetime.strptime('2018-08-31 06:01:00', '%Y-%m-%d %H:%M:%S')

# 返回一个kline的序列，所有开始时间在start到end之间的K线
# 如果在某个时间段内没有成交，则对应的K线不存在，也不会返回。
kline_arr = market.get_market_history(3600, start, end)
kline = kline_arr[0]

# 开盘价格
print(float(kline['openPrice']))
# 收盘价格
print(float(kline['closePrice']))
# 最高价格
print(float(kline['highPrice']))
# 最低价格
print(float(kline['lowPrice']))

```


### 获取账户信息
```python
import cybex
...
account = cybex.account.Account(ACCOUNT_NAME, cybex_instance = instance)

# 查看账户id
print(account["id"])

# 查看账户在链上活跃权限对应的公钥
print(account['active']['key_auths'])

# 查看账户在链上账户权限对应的公钥
print(account['owner']['key_auths'])

# 查看账户余额
for bal in account.balances:
    # 资产名
    print(bal.asset.symbol)
    # 资产id
    print(bal.asset["id"])
    # 资产数量
    print(bal.amount) # 不含精度的浮点数
    
```    

### 下单
报单操作可以向赛贝去中心化交易所发送订单。在赛贝中，一个交易市场由两种资产组成，用户报单即提交订单，付出一些资产A，期望获得一些资产B的行为。 为了方便用户理解，赛贝交易所的常用客户端没有采用资产交换的方式来表示订单和价格，而是采用与传统交易所相似的价格+数量的方式，Python库也使用这种模式报单，本demo会同时在注释中说明报单的实际资产交换值。 报单操作前，需要将报单账户的私钥提前加入Python库的本地钱包中，并解锁钱包。钱包操作详见钱包操作。 赛贝交易所的资产由瑶池(JADE)托管，所有由瑶池托管的资产以JADE.作为资产前缀，比如以太坊的资产名为JADE.ETH，此外还有JADE.EOS, JADE.BTC, JADE.USDT等。赛贝的网页前端为了照顾用户使用习惯，在显示时去除了JADE前缀，在使用Python库时，资产名需要加JADE.前缀。

```python
import cybex
from cybex import Market, Asset, Account

NODE_URL = 'ws://127.0.0.1:8090'
WALLET_PWD = '123456'

instance = cybex.Cybex(NODE_URL)
instance.wallet.unlock(WALLET_PWD)

asset1 = Asset('CYB')
asset2 = Asset('JADE.ETH')
m = Market(base = asset1, quote = asset2,
    cybex_instance = self.instance)

# 下面的buy操作表示:
# 以1000的单价购买1个asset2，交易超时时间为3600秒，超时自动撤销
# 希望付出1000个CYB，希望至少获得1个JADE.ETH
# 用户下单时，账户中会立刻扣除1000个CYB，如果账户中CYB不足，会造成下单失败
m.buy(1000, 1, 3600, killfill = False, account = 'seller-account')

# 下面的sell操作表示:
m.sell(1200, 1, 3600, killfill = False, account = 'seller-account')

# 下面的cancel操作表示:
# 通过account.openorders接口获取订单id
instance.cancel(order_id, account = 'seller-account')
```

### 转账

转账操作可用于将资产从一个账户转移到另一个账户或地址。转账分为普通转账和锁定期转账。锁定期资产与接收方的公钥关联，只有该公钥对应的私钥持有者，才能申领该锁定期资产。锁定期资产的申领操作详见[资产操作](https://github.com/NebulaCybexDEX/cybex-node-doc/blob/master/transaction/python/balance.md)

```python
import cybex

NODE_URL = 'ws://127.0.0.1:8090'
WALLET_PWD = '123456'

instance = cybex.Cybex(NODE_URL)
instance.wallet.unlock(WALLET_PWD)

# 从from-account转10CYB到to-account，并带有附言memo string，
# 如果不需要填写附言，传入''作为附言字符串即可
instance.transfer('to-account', 10, 'CYB', 'memo string', 'from-account')

# 获取收款账号的公钥
# 这里使用收款账号活跃权限的第一把公钥，使用者也可视情况选择其他公钥
account = cybex.account.Account('to-account', cybex_instance = instance)
to-account-pubkey = account['active']['key_auths'][0][0]

# 从from-account转10CYB到to-account，并锁定600秒
instance.transfer('to-account', 10, 'CYB', '', 'from-account', extensions = [[1, {
    'vesting_period': 600,
    'public_key': to-account-pubkey
}]])

```


### 提现
提现操作可用于将资产从赛贝交易所提现到外部链。

* 瑶池（Jadepool）作为Cybex推荐的网关将为您提供这一服务 
* 网关服务需要一定的手续费支持运行，其中需要执行一次Cybex内盘转账，该部分手续费您可选择使用CYB支付或ETH支付 
* 网关执行手续费将以您希望取出的目标资产支付，并从您提取金额中扣除 
* 请务必确认您的提币地址正确，一旦填写错误，您的提币将丢失 
* 所有出入金到账需要一定时限，请耐心等待 
* 提币操作请使用您的个人钱包地址, 提币到合约地址、交易所地址、ICO项目地址可能会发生合约执行失败，将导致转账失败，资产将退回到您的Cybex账户，处理时间较长，请您谅解。
* 目前支持提现的币种有
JADE.ETH  JADE.BTC  JADE.LTC  JADE.EOS  JADE.USDT
JADE.MT   JADE.SNT  JADE.PAY  JADE.GET  JADE.MVP
JADE.DPY  JADE.MVP  JADE.LHT  JADE.INK  JADE.BAT
JADE.OMG  JADE.NAS  JADE.KNC  JADE.MAD  JADE.TCT
JADE.GNT  JADE.GNX
* 提现EOS时，如果需要在链上转账中填入附言，请使用to-account[memo]作为目标地址。如不需要填入附言，请使用to-account[]作为目标地址

```python
import cybex

NODE_URL = 'ws://127.0.0.1:8090'
WALLET_PWD = '123456'

instance = cybex.Cybex(NODE_URL)
instance.wallet.unlock(WALLET_PWD)

# 不同资产使用不同的目标地址作为提现地址
# asset-name为您希望提现的资产名字，请使用JADE.作为资产前缀，如JADE.ETH
# withdraw-account-name为您在赛贝交易所的账号名
instance.withdraw(to_addr, 10, 'asset-name', 'withdraw-account-name')
```

### 其他操作
[申领资产](https://github.com/NebulaCybexDEX/Cybex-node-doc/blob/master/transaction/python/balance.md)  
[高级操作](https://github.com/NebulaCybexDEX/Cybex-node-doc/blob/master/transaction/python/advanced)


##JavaScript库

CybexJS库用于包装直接Cybex链进行交互接口与逻辑。其中包含两个独立的NPM包——**Cybexjs**与**Cybexjs-ws**。其中Cybexjs-ws包用以管理客户端与Cybex节点之间的Websocket链接，以提供底层的**API查询**/**消息订阅**与**Transaction广播上链**的实现；而Cybexjs库则提供了与ECC/Serialize有关的方法，并提供了相关的辅助工具，用以完成**公私钥生成管理**/**Transaction构造**/**签名验签**等基础功能。此外还提供了工具类**ChainStore**，用以提供链上信息的快捷查询和本地化缓存。

### 安装

通过npm或yarn安装 
`
yarn add Cybexjs
`


### 引入

可以通过CommonJS或ES import方式获取所需的工具类，如

```javascript
import { Apis } from "Cybexjs-ws";	
import { ChainStore } from "Cybexjs";

// or
const { CybexStore } = require("Cybexjs");
const { Apis } = require("Cybexjs-ws");
``` 

### 初始化

Cybexjs需要使用Cybexjs-ws库中的Apis工具类进行与Cybex节点的通讯，并进行Login操作：

```
let instanceRes = await Apis.instance("ws://127.0.0.1:8090", true).init_promise;	
```

初次使用Apis.instance()方法并传入所需连接的节点地址，Cybexjs将会尝试链接节点并进行访问各类型API所需的准备工作。需要直接进行节点的访问查询时，再次调用Apis.instance()获取一个全局实例，使用该实例的方法db\_api()/history\_api()/network\_api()/network\_api()获得访问不同类型api的途径，并添加参数进行查询或广播，如：
	
```
let globalDynamicObject = 
	await this.Apis.instance()
      	.db_api()
      	.exec("get_dynamic_global_properties", []);
```

大多数情况下，对于链上数据的获取可以同过Cybexjs库中封装好的方法进行。在使用之前，需进行ChainStore的初始化工作：

```
ChainStore.init().then(() => {
   ChainStore.subscribe(updateState);
});
				
let dynamicGlobal = null;
function updateState(object) {
    dynamicGlobal = ChainStore.getObject("2.1.0");
    console.log("ChainStore object update\n", dynamicGlobal ? dynamicGlobal.toJS() : dynamicGlobal);
		}
```

初始化过程中，会自动调用`set_subscribe_callback`方法订阅链上notice通知，并通过`subscribe`方法传入的更新回调同步链上状态的更新。

### 查询接口

Cybexjs库中的 **ChainStore** 类提供了若干用以获取常用数据的辅助方法，可直接按需加以调用。并提供了工具函数 **FetchChain** 用来执行更加一般的数据获取。

```	javascript
// example
let name = "harley";
FetchChain(
	"getAccount", 
	name, 
	undefined, 
	{ [name]: true }
).then(([harley]) => {
   console.log(harley);
});
```

方法参数：

| 参数 | 类型 | 说明 |
| :---: | :---: | --- |
| **methodName** | string | 用以传入希望调用的接口的方法名 |
| **objectIds** | string[] | 希望获取的Cybex数据标识，通常是某一记录的id。但对于用户或资产这类语义话数据，也可能是用户名或资产全称。|
| **timeout** | number(可选) | 希望的超时时间。可选参数，默认为1900毫秒 |
| **subMap** | Object(可选) | 用以管理查询更新订阅的对象，默认为空对象 |

**methodName** 支持的部分方法名

| 接口方法名 | 接口参数 |
| --- | --- |
| getObject | Cybex object ID |
| getAsset | ID or symbol |
| getAccountRefsOfKey | key |
| getAccountRefsOfAccount | account ID |
| getBalanceObjects | address |
| getAccount | account ID |

更多接口可查看ChainStore实现。


### 管理密钥

Cybexjs库提供了有关公私钥及签名管理的操作。通过**PrivateKey**/**PublicKey**/**Signature**等工具类实现。并提供了**Login**类来简化一般的私钥管理签名操作。

Login类使用如下格式生成私钥种子

`
	keySeed = accountName + role + password
`

通过`generateKeys(account, password, [roles])`方法将生成对应不同roles的私钥。在需要使用时，需要提供私钥，并调用`checkKeys(account, password, auths)`方法验证其有效性，一旦验证成功，即可使用`signTransaction(tr)`方法对Transaction进行签名。

更加一般的方法私钥管理和签名方法，可以使用**Transaction**与**Privatekey**配合实现。

### 实现交易

* 关于广播并执行一个交易，大致的步骤是 **构造交易体 - 向交易体中添加操作 - 添加签名 - 广播交易**，其中需要手动进行的主要是**构造和添加交易中的操作**，其他主要可以由**TransacitonBuilder**类协助执行。
* Cybex中的所有标准数据结构都有唯一ID进行标识。CybexID采用三段式的形式标记（如x.x.xxxx的形式）。	
	* 其中第一段为数据大类，目前Cybex中有0/1/2三类ID：
		* **0** 目前仅用来表示撮合成交
		* **1** 表示链上协议中定义的数据结构，用来进行交易和操作的验证，钱包与数据库都需遵循该结构；
		* **2** 表示系统当前运行中所采用的动态数据，不需要在客户端和节点之间进行通讯，仅存在于链上，用于执行业务逻辑。如某一资产现有的资金池状况/智能资产的抵押状况/当前系统的区块数据等。
	* ID第二段表示具体的数据类型，第三段表示在该类型中的序号。

* 可以添加的操作列表，以及操作结构的数据说明可以在 [https://github.com/CybexDex/Cybexjs/blob/master/serializer/src/operations.js](https://github.com/CybexDex/Cybexjs/blob/master/serializer/src/operations.js) 和 [https://github.com/CybexDex/Cybexjs/blob/master/serializer/src/types.js](https://github.com/CybexDex/Cybexjs/blob/master/serializer/src/types.js) 查询。

注册用户是一个典型的需要广播来修改链上信息的交易，用以创建链上新的用户。其他需要广播的交易步骤也与此类似。

```	javascript
        let tr = new TransactionBuilder(); // 构造一个交易体，TransactionBuilder可从Cybexjs库中获得。
        tr.add_type_operation("account_create", { // 向交易中添加所需广播的操作，操作名称为 account_create.
        fee: { 
			  amount: 0,
            asset_id: "1.3.0"
          }, // 该笔交易的手续费字段。
          registrar: chain_registrar.get("id"), // 注册人ID
          referrer: chain_referrer.get("id"), // 引荐人ID
          referrer_percent: referrer_percent, // 引荐人返利额度
          name: new_account_name, // 用户名
          owner: { // 用户的Owner权限设置
            weight_threshold: 1, // Owner权限的域限
            account_auths: [], // 占有权限的用户列表
            key_auths: [[owner_pubkey, 1]], // 占有权限的公钥列表
            address_auths: [] // 占有权限的地址列表
          },
          active: { // 活跃权限配置
            weight_threshold: 1,
            account_auths: [],
            key_auths: [[active_pubkey, 1]],
            address_auths: []
          },
          options: { //其他配置
            memo_key: active_pubkey, // Memo公钥，其他人向该用户发送Memo时，将使用该公钥进行加密
            voting_account: "1.2.5", // 默认投票代理
            num_witness: 0, // 初始投票的证人数量
            num_committee: 0, // 初始投票的委员会数量
            votes: [] //初始投票的ID集合
          }
        });
        tr.add_signer(privKey); // 向交易添加签名，传入私钥
        await tr.broadcast(); // 广播交易，异步操作
```

* 本接口数据结构可查询 [https://github.com/NebulaCybexDEX/Cybex-core/blob/master/libraries/chain/include/graphene/chain/protocol/account.hpp](https://github.com/NebulaCybexDEX/Cybex-core/blob/master/libraries/chain/include/graphene/chain/protocol/account.hpp) 获得基本说明。

	
## 区块链对象及标识

我们将简单介绍一下Cybex对象的有关概念，以便各个客户端在调用之前有一个大致的了解。

石墨烯技术对协议和实现空间的对象进行了区分。在协议层面，有诸如账户、资产、委员会成员、订单、余额等原始对象。而实现层面的对象，则是为了访问更高的抽象层，比如获取数据库当前状态（包括当前区块链全局属性、动态资产数据、事务历史记录、账户统计和预算记录等等）。

### 标准数据结构与三段式ID

Cybex中的所有标准数据结构都由唯一ID进行标识，ID采用三段式的格式，即`xx.xx.xx`：

1. 第一段为数据大类，目前仅1/2/3三类ID，其中：  
    0 ⽬目前仅⽤用来表示撮合成交；  
    1 表示链上协议中定义的数据结构，⽤用来进⾏交易和操作的验证，钱包与数据库都需遵循该结构;  
    2 表示系统当前运⾏中所采用的动态数据，不需要在客户端和节点之间进行通讯，仅存在于链上，⽤于执⾏业务 逻辑。如某⼀资产现有的资⾦池状况/智能资产的抵押状况/当前系统的区块数据等。  
2. 第二段为数据类型；
3. 第三段为数据在该类型中的序号。

### 常用对象列表

| ID | 对象类型 | 区块链定义的基础数据ID
| --- | --- | ---
| 1.1.x | base object | 
| 1.2.x | account object | _1.2.0（理事会成员账户详细信息）_  
| 1.3.x | asset object |  _1.3.0（核心资产，即CYB）_  
| 1.4.x | force settlement object |
| 1.5.x | committee member object |
| 1.6.x | witness object |
| 1.7.x | limit order object |
| 1.8.x | call order object |
| 1.9.x | custom object |
| 1.10.x | proposal object |
| 1.11.x | operation history object |
| 1.12.x | withdraw permission object |
| 1.13.x | vesting balance object |
| 1.14.x | worker object |
| 1.15.x | balance object |
| 2.0.x | global\_property\_object | _2.0.0（区块链全局参数）_  
| 2.1.x | dynamic\_global\_property\_object | _2.1.0（当前区块链数据）_  
| 2.3.x | asset\_dynamic\_data |
| 2.4.x | asset\_bitasset\_data |
| 2.5.x | account\_balance\_object |
| 2.6.x | account\_statistics\_object |
| 2.7.x | transaction\_object |
| 2.8.x | block\_summary\_object |
| 2.9.x | account\_transaction\_history\_object |
| 2.10.x | blinded\_balance\_object |
| 2.11.x | chain\_property\_object |
| 2.12.x | witness\_schedule\_object |
| 2.13.x | budget\_record\_object |
| 2.14.x | special\_authority\_object |

# API文档

## 数据库API

### 1. 对象

#### get_objects


`Return the details of objects retrieved, in the order they are mentioned in ids`

* Requesst:

		curl --data '{"jsonrpc":"2.0","method":"get_objects","params":[["1.2.7", "1.3.0"]],"id":1}' https://apihk.cybex.io
* Parameters:

		ids: IDs of the objects to retrieve

* Response:
		
		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.2.7",                      // id of account
				"membership_expiration_date": "1969-12-31T23:59:59",
				"registrar": "1.2.7",
				"referrer": "1.2.7",
				"lifetime_referrer": "1.2.7",
				"network_fee_percentage": 2000,
				"lifetime_referrer_fee_percentage": 8000,
				"referrer_rewards_percentage": 0,
				"name": "harley",
				"owner": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB8jM4e1z1xS63xxVhuperU1VvusQz8Ye8CrnTBigFUaErpigpPL", 1]
					],
					"address_auths": []
				},
				"active": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR", 1]
					],
					"address_auths": []
				},
				"options": {
					"memo_key": "CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR",
					"voting_account": "1.2.5",
					"num_witness": 0,
					"num_committee": 0,
					"votes": [],
					"extensions": []
				},
				"statistics": "2.6.7",
				"whitelisting_accounts": [],
				"blacklisting_accounts": [],
				"whitelisted_accounts": [],
				"blacklisted_accounts": [],
				"cashback_vb": "1.13.0",
				"owner_special_authority": [0, {}],
				"active_special_authority": [0, {}],
				"top_n_control_flags": 0
			}, {
				"id": "1.3.0",                   // id of asset
				"symbol": "CYB",
				"precision": 5,
				"issuer": "1.2.3",
				"options": {
					"max_supply": "100000000000000",
					"market_fee_percent": 0,
					"max_market_fee": "1000000000000000",
					"issuer_permissions": 0,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 1,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1,
							"asset_id": "1.3.0"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.0"
			}]
		}
		
### 2. 区块和事务

#### get_block_header

`Return the header of referenced block, or null if no matching block was found`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_block_header","params":["4427000"],"id":1}' https://apihk.cybex.io

* Parameters:

		block_num: height of the block to be returned

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"previous": "00438cf7e5e52236bf61d15740449cfee84c257b",
				"timestamp": "2018-07-30T09:49:03",
				"witness": "1.6.8",
				"transaction_merkle_root": "7a9f852c737b442ce42d3f239f723994aad82fa7",
				"extensions": []
			}
		}

#### get_block

`Return the referenced block, or null if no matching block was found`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_block","params":["4427000"],"id":1}' https://apihk.cybex.io

* Parameters:

		block_num: height of the block to be returned

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"previous": "00438cf7e5e52236bf61d15740449cfee84c257b",
				"timestamp": "2018-07-30T09:49:03",
				"witness": "1.6.8",
				"transaction_merkle_root": "7a9f852c737b442ce42d3f239f723994aad82fa7",
				"extensions": [],
				"witness_signature": "2037dac761871e52abf2e58669e23ad2e07732f1a808cae794e5503f11a004f29d3d8689eb922fdce556034cc3cd6b6785da3109f5ec6aa0ae21368a84e54db296",
				"transactions": [{
					"ref_block_num": 36086,
					"ref_block_prefix": 1500000766,
					"expiration": "2018-07-30T09:49:29",
					"operations": [
						[1, {
							"fee": {
								"amount": 55,
								"asset_id": "1.3.0"
							},
							"seller": "1.2.31042",
							"amount_to_sell": {
								"amount": 17977820,
								"asset_id": "1.3.27"
							},
							"min_to_receive": {
								"amount": 7340834,
								"asset_id": "1.3.0"
							},
							"expiration": "2018-07-30T09:54:59",
							"fill_or_kill": false,
							"extensions": []
						}]
					],
					"extensions": [],
					"signatures": ["1f696c3e532e163b4860a39da54f89c81c37f850a012b46767919ff6436e008fd939d992df4442894205a1582452dfd23aae8c2446d4ea0ef9e031fc0d036cfe80"],
					"operation_results": [
						[1, "1.7.61886713"]
					]
				}, {
					"ref_block_num": 36086,
					"ref_block_prefix": 1500000766,
					"expiration": "2018-07-30T09:49:29",
					"operations": [
						[1, {
							"fee": {
								"amount": 55,
								"asset_id": "1.3.0"
							},
							"seller": "1.2.31085",
							"amount_to_sell": {
								"amount": 10474665,
								"asset_id": "1.3.27"
							},
							"min_to_receive": {
								"amount": 1259669,
								"asset_id": "1.3.26"
							},
							"expiration": "2018-07-30T09:53:59",
							"fill_or_kill": false,
							"extensions": []
						}]
					],
					"extensions": [],
					"signatures": ["1f2b5b86599c7dd7e22525f18bed562cc82e21b03e2fdf26dbf4a40b37239c403a014d3086cbee64f3f114372ed36dce75b3a45e2d2b48a1b5dfcbfe825a7d9cc5"],
					"operation_results": [
						[1, "1.7.61886714"]
					]
				}, {
					"ref_block_num": 36086,
					"ref_block_prefix": 1500000766,
					"expiration": "2018-07-30T09:49:29",
					"operations": [
						[1, {
							"fee": {
								"amount": 55,
								"asset_id": "1.3.0"
							},
							"seller": "1.2.31102",
							"amount_to_sell": {
								"amount": 123926,
								"asset_id": "1.3.3"
							},
							"min_to_receive": {
								"amount": 1212338,
								"asset_id": "1.3.26"
							},
							"expiration": "2018-07-30T09:53:59",
							"fill_or_kill": false,
							"extensions": []
						}]
					],
					"extensions": [],
					"signatures": ["1f107e8f2f499566972a6d8a7d504ac30f2fe6d71d9b7ff3a4374dec6ff1907bfb4b8ef2ab06695a7c15baa386a24c4400fcd577a3b0fc54172e038755bd9d674b"],
					"operation_results": [
						[1, "1.7.61886715"]
					]
				}, {
					"ref_block_num": 36086,
					"ref_block_prefix": 1500000766,
					"expiration": "2018-07-30T09:49:29",
					"operations": [
						[1, {
							"fee": {
								"amount": 55,
								"asset_id": "1.3.0"
							},
							"seller": "1.2.10641",
							"amount_to_sell": {
								"amount": 2660695,
								"asset_id": "1.3.0"
							},
							"min_to_receive": {
								"amount": 14157,
								"asset_id": "1.3.2"
							},
							"expiration": "2018-07-30T09:53:59",
							"fill_or_kill": false,
							"extensions": []
						}]
					],
					"extensions": [],
					"signatures": ["1f041a5aa35f798776cf668c2b4bbaeefe0fb3762607bb454fe73aa8dd3cfb4e9b3a386cc2e2a2e47e575ab9f173d8931acbedf65547a244c9ee65b2f48dc7cf8b"],
					"operation_results": [
						[1, "1.7.61886716"]
					]
				},
				.....
				]
			}
		}

#### get_transaction

`Used to fetch an individual transaction`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_transaction","params":["4427000","2"],"id":1}' https://apihk.cybex.io

* Parameters:

		block_num: block number

		trx_in_block: subscript number of transaction in block, starting from zero

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"ref_block_num": 36086,
				"ref_block_prefix": 1500000766,
				"expiration": "2018-07-30T09:49:29",
				"operations": [
					[1, {
						"fee": {
							"amount": 55,
							"asset_id": "1.3.0"
						},
						"seller": "1.2.31102",
						"amount_to_sell": {
							"amount": 123926,
							"asset_id": "1.3.3"
						},
						"min_to_receive": {
							"amount": 1212338,
							"asset_id": "1.3.26"
						},
						"expiration": "2018-07-30T09:53:59",
						"fill_or_kill": false,
						"extensions": []
					}]
				],
				"extensions": [],
				"signatures": ["1f107e8f2f499566972a6d8a7d504ac30f2fe6d71d9b7ff3a4374dec6ff1907bfb4b8ef2ab06695a7c15baa386a24c4400fcd577a3b0fc54172e038755bd9d674b"],
				"operation_results": [
					[1, "1.7.61886715"]
				]
			}
		}

### 3. 区块链全局

#### get\_chain\_properties()

`Retrieve the chain property object associated with the chain`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_chain_properties","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"id": "2.11.0",
				"chain_id": "90be01e82b981c8f201c9a78a3d31f655743b29ff3274727b1439b093d04aa23",
				"immutable_parameters": {
					"min_committee_member_count": 21,
					"min_witness_count": 11,
					"num_special_accounts": 0,
					"num_special_assets": 0
				}
			}
		}

**get\_global\_properties()**

`Retrieve the current global property object`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_global_properties","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"id": "2.0.0",
				"parameters": {
					"current_fees": {
						"parameters": [
							[0, {
								"fee": 1000,
								"price_per_kbyte": 555
							}],
							[1, {
								"fee": 55
							}],
							[2, {
								"fee": 5
							}],
							[3, {
								"fee": 55
							}],
							[4, {}],
							[5, {
								"basic_fee": 1000000,
								"premium_fee": "7500000000",
								"price_per_kbyte": 5000
							}],
							[6, {
								"fee": 55,
								"price_per_kbyte": 389
							}],
							[7, {
								"fee": 5555
							}],
							[8, {
								"membership_annual_fee": "60168000000000",
								"membership_lifetime_fee": 100000000
							}],
							[9, {
								"fee": 277784
							}],
							[10, {
								"symbol3": "100000000000",
								"symbol4": "100000000000",
								"long_symbol": 50000000,
								"price_per_kbyte": 100000
							}],
							[11, {
								"fee": 111114,
								"price_per_kbyte": 389
							}],
							[12, {
								"fee": 277784
							}],
							[13, {
								"fee": 277784
							}],
							[14, {
								"fee": 1000,
								"price_per_kbyte": 555
							}],
							[15, {
								"fee": 55
							}],
							[16, {
								"fee": 27778
							}],
							[17, {
								"fee": 2777
							}],
							[18, {
								"fee": 277784
							}],
							[19, {
								"fee": 5
							}],
							[20, {
								"fee": 2777844
							}],
							[21, {
								"fee": 555
							}],
							[22, {
								"fee": 8333,
								"price_per_kbyte": 2777
							}],
							[23, {
								"fee": 277,
								"price_per_kbyte": 389
							}],
							[24, {
								"fee": 0
							}],
							[25, {
								"fee": 8333
							}],
							[26, {
								"fee": 555
							}],
							[27, {
								"fee": 800,
								"price_per_kbyte": 389
							}],
							[28, {
								"fee": 0
							}],
							[29, {
								"fee": 277784
							}],
							[30, {
								"fee": 555569
							}],
							[31, {
								"fee": 0
							}],
							[32, {
								"fee": 55557
							}],
							[33, {
								"fee": 111114
							}],
							[34, {
								"fee": 2777844
							}],
							[35, {
								"fee": 555,
								"price_per_kbyte": 2777
							}],
							[36, {
								"fee": 27778
							}],
							[37, {}],
							[38, {
								"fee": 55557,
								"price_per_kbyte": 389
							}],
							[39, {
								"fee": 11667,
								"price_per_output": 3889
							}],
							[40, {
								"fee": 11667,
								"price_per_output": 3889
							}],
							[41, {
								"fee": 11667
							}],
							[42, {}],
							[43, {
								"fee": 55557
							}],
							[44, {}],
							[45, {
								"fee": 10000,
								"price_per_kbyte": 10
							}],
							[46, {
								"fee": 10000,
								"price_per_kbyte": 10
							}],
							[47, {
								"fee": 10000,
								"price_per_kbyte": 10
							}]
						],
						"scale": 10000
					},
					"block_interval": 3,
					"maintenance_interval": 86400,
					"maintenance_skip_slots": 3,
					"committee_proposal_review_period": 0,
					"maximum_transaction_size": 2048,
					"maximum_block_size": 2048000000,
					"maximum_time_until_expiration": 86400,
					"maximum_proposal_lifetime": 62208000,
					"maximum_asset_whitelist_authorities": 10,
					"maximum_asset_feed_publishers": 10,
					"maximum_witness_count": 1001,
					"maximum_committee_count": 1001,
					"maximum_authority_membership": 10,
					"reserve_percent_of_fee": 2000,
					"network_percent_of_fee": 2000,
					"lifetime_referrer_percent_of_fee": 3000,
					"cashback_vesting_period_seconds": 31536000,
					"cashback_vesting_threshold": 10000000,
					"count_non_member_votes": true,
					"allow_non_member_whitelists": false,
					"witness_pay_per_block": 10000,
					"worker_budget_per_day": "50000000000",
					"max_predicate_opcode": 1,
					"fee_liquidation_threshold": 10000000,
					"accounts_per_fee_scale": 1000,
					"account_fee_scale_bitshifts": 4,
					"max_authority_depth": 2,
					"extensions": []
				},
				"next_available_vote_id": 37,
				"active_committee_members": ["1.5.11", "1.5.3", "1.5.12", "1.5.13", "1.5.6", "1.5.1", "1.5.2", "1.5.4", "1.5.5", "1.5.7", "1.5.8", "1.5.9", "1.5.10", "1.5.14", "1.5.15", "1.5.16", "1.5.17", "1.5.18", "1.5.19", "1.5.20", "1.5.21"],
				"active_witnesses": ["1.6.1", "1.6.2", "1.6.3", "1.6.4", "1.6.5", "1.6.6", "1.6.7", "1.6.8", "1.6.9", "1.6.10", "1.6.11"]
			}
		}

#### get\_config

`Retrieve the current global property object`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_config","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"GRAPHENE_SYMBOL": "CYB",
				"GRAPHENE_ADDRESS_PREFIX": "CYB",
				"GRAPHENE_MIN_ACCOUNT_NAME_LENGTH": 1,
				"GRAPHENE_MAX_ACCOUNT_NAME_LENGTH": 63,
				"GRAPHENE_MIN_ASSET_SYMBOL_LENGTH": 3,
				"GRAPHENE_MAX_ASSET_SYMBOL_LENGTH": 16,
				"GRAPHENE_MAX_SHARE_SUPPLY": "1000000000000000",
				"GRAPHENE_MAX_PAY_RATE": 10000,
				"GRAPHENE_MAX_SIG_CHECK_DEPTH": 2,
				"GRAPHENE_MIN_TRANSACTION_SIZE_LIMIT": 1024,
				"GRAPHENE_MIN_BLOCK_INTERVAL": 1,
				"GRAPHENE_MAX_BLOCK_INTERVAL": 30,
				"GRAPHENE_DEFAULT_BLOCK_INTERVAL": 5,
				"GRAPHENE_DEFAULT_MAX_TRANSACTION_SIZE": 2048,
				"GRAPHENE_DEFAULT_MAX_BLOCK_SIZE": 2000000,
				"GRAPHENE_DEFAULT_MAX_TIME_UNTIL_EXPIRATION": 86400,
				"GRAPHENE_DEFAULT_MAINTENANCE_INTERVAL": 86400,
				"GRAPHENE_DEFAULT_MAINTENANCE_SKIP_SLOTS": 3,
				"GRAPHENE_MIN_UNDO_HISTORY": 10,
				"GRAPHENE_MAX_UNDO_HISTORY": 10000,
				"GRAPHENE_MIN_BLOCK_SIZE_LIMIT": 5120,
				"GRAPHENE_MIN_TRANSACTION_EXPIRATION_LIMIT": 150,
				"GRAPHENE_BLOCKCHAIN_PRECISION": 100000,
				"GRAPHENE_BLOCKCHAIN_PRECISION_DIGITS": 5,
				"GRAPHENE_DEFAULT_TRANSFER_FEE": 100000,
				"GRAPHENE_MAX_INSTANCE_ID": "281474976710655",
				"GRAPHENE_100_PERCENT": 10000,
				"GRAPHENE_1_PERCENT": 100,
				"GRAPHENE_MAX_MARKET_FEE_PERCENT": 10000,
				"GRAPHENE_DEFAULT_FORCE_SETTLEMENT_DELAY": 86400,
				"GRAPHENE_DEFAULT_FORCE_SETTLEMENT_OFFSET": 0,
				"GRAPHENE_DEFAULT_FORCE_SETTLEMENT_MAX_VOLUME": 2000,
				"GRAPHENE_DEFAULT_PRICE_FEED_LIFETIME": 86400,
				"GRAPHENE_MAX_FEED_PRODUCERS": 200,
				"GRAPHENE_DEFAULT_MAX_AUTHORITY_MEMBERSHIP": 10,
				"GRAPHENE_DEFAULT_MAX_ASSET_WHITELIST_AUTHORITIES": 10,
				"GRAPHENE_DEFAULT_MAX_ASSET_FEED_PUBLISHERS": 10,
				"GRAPHENE_COLLATERAL_RATIO_DENOM": 1000,
				"GRAPHENE_MIN_COLLATERAL_RATIO": 1001,
				"GRAPHENE_MAX_COLLATERAL_RATIO": 32000,
				"GRAPHENE_DEFAULT_MAINTENANCE_COLLATERAL_RATIO": 1750,
				"GRAPHENE_DEFAULT_MAX_SHORT_SQUEEZE_RATIO": 1500,
				"GRAPHENE_DEFAULT_MARGIN_PERIOD_SEC": 2592000,
				"GRAPHENE_DEFAULT_MAX_WITNESSES": 1001,
				"GRAPHENE_DEFAULT_MAX_COMMITTEE": 1001,
				"GRAPHENE_DEFAULT_MAX_PROPOSAL_LIFETIME_SEC": 2419200,
				"GRAPHENE_DEFAULT_COMMITTEE_PROPOSAL_REVIEW_PERIOD_SEC": 1209600,
				"GRAPHENE_DEFAULT_NETWORK_PERCENT_OF_FEE": 2000,
				"GRAPHENE_DEFAULT_LIFETIME_REFERRER_PERCENT_OF_FEE": 3000,
				"GRAPHENE_DEFAULT_MAX_BULK_DISCOUNT_PERCENT": 5000,
				"GRAPHENE_DEFAULT_BULK_DISCOUNT_THRESHOLD_MIN": 100000000,
				"GRAPHENE_DEFAULT_BULK_DISCOUNT_THRESHOLD_MAX": "10000000000",
				"GRAPHENE_DEFAULT_CASHBACK_VESTING_PERIOD_SEC": 31536000,
				"GRAPHENE_DEFAULT_CASHBACK_VESTING_THRESHOLD": 10000000,
				"GRAPHENE_DEFAULT_BURN_PERCENT_OF_FEE": 2000,
				"GRAPHENE_WITNESS_PAY_PERCENT_PRECISION": 1000000000,
				"GRAPHENE_DEFAULT_MAX_ASSERT_OPCODE": 1,
				"GRAPHENE_DEFAULT_FEE_LIQUIDATION_THRESHOLD": 10000000,
				"GRAPHENE_DEFAULT_ACCOUNTS_PER_FEE_SCALE": 1000,
				"GRAPHENE_DEFAULT_ACCOUNT_FEE_SCALE_BITSHIFTS": 4,
				"GRAPHENE_MAX_WORKER_NAME_LENGTH": 63,
				"GRAPHENE_MAX_URL_LENGTH": 127,
				"GRAPHENE_NEAR_SCHEDULE_CTR_IV": "7640891576956012808",
				"GRAPHENE_FAR_SCHEDULE_CTR_IV": "13503953896175478587",
				"GRAPHENE_CORE_ASSET_CYCLE_RATE": 17,
				"GRAPHENE_CORE_ASSET_CYCLE_RATE_BITS": 32,
				"GRAPHENE_DEFAULT_WITNESS_PAY_PER_BLOCK": 1000000,
				"GRAPHENE_DEFAULT_WITNESS_PAY_VESTING_SECONDS": 86400,
				"GRAPHENE_DEFAULT_WORKER_BUDGET_PER_DAY": "50000000000",
				"GRAPHENE_MAX_INTEREST_APR": 10000,
				"GRAPHENE_COMMITTEE_ACCOUNT": "1.2.0",
				"GRAPHENE_WITNESS_ACCOUNT": "1.2.1",
				"GRAPHENE_RELAXED_COMMITTEE_ACCOUNT": "1.2.2",
				"GRAPHENE_NULL_ACCOUNT": "1.2.3",
				"GRAPHENE_TEMP_ACCOUNT": "1.2.4"
			}
		}

#### get\_chain\_id

`Get the chain ID`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_chain_id","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": "90be01e82b981c8f201c9a78a3d31f655743b29ff3274727b1439b093d04aa23"
		}

#### get\_dynamic\_global\_properties

`Retrieve the current dynamic global property object`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_dynamic_global_properties","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"id": "2.1.0",
				"head_block_number": 4514733,
				"head_block_id": "0044e3ad4f9edb64dfad7dfd42a000edac77ce9e",
				"time": "2018-08-02T10:56:33",
				"current_witness": "1.6.6",
				"next_maintenance_time": "2018-08-02T19:00:00",
				"last_budget_time": "2018-08-01T19:00:03",
				"witness_budget": 96720000,
				"accounts_registered_this_interval": 70,
				"recently_missed_count": 0,
				"current_aslot": 10499027,
				"recent_slots_filled": "340282366920938463463374607431768211455",
				"dynamic_flags": 0,
				"last_irreversible_block_num": 4514724
			}
		}

### 4. 账户

#### get_accounts

`Get a list of accounts by ID`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_accounts","params":[["1.2.7"]],"id":1}' https://apihk.cybex.io

* Parameters:

 		account_ids: IDs of the accounts to retrieve

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.2.7",
				"membership_expiration_date": "1969-12-31T23:59:59",
				"registrar": "1.2.7",
				"referrer": "1.2.7",
				"lifetime_referrer": "1.2.7",
				"network_fee_percentage": 2000,
				"lifetime_referrer_fee_percentage": 8000,
				"referrer_rewards_percentage": 0,
				"name": "harley",
				"owner": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB8jM4e1z1xS63xxVhuperU1VvusQz8Ye8CrnTBigFUaErpigpPL", 1]
					],
					"address_auths": []
				},
				"active": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR", 1]
					],
					"address_auths": []
				},
				"options": {
					"memo_key": "CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR",
					"voting_account": "1.2.5",
					"num_witness": 0,
					"num_committee": 0,
					"votes": [],
					"extensions": []
				},
				"statistics": "2.6.7",
				"whitelisting_accounts": [],
				"blacklisting_accounts": [],
				"whitelisted_accounts": [],
				"blacklisted_accounts": [],
				"cashback_vb": "1.13.0",
				"owner_special_authority": [0, {}],
				"active_special_authority": [0, {}],
				"top_n_control_flags": 0
			}]
		}

#### get\_full\_accounts

`Map of string from names_or_ids to the corresponding account`

* Request:

			curl --data '{"jsonrpc":"2.0","method":"get_full_accounts","params":[["1.2.7"], false],"id":1}' https://apihk.cybex.io

* Parameters:

		names_or_ids: Each item must be the name or ID of an account to retrieve
		callback: Bool, function to call with updates

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [
				["1.2.7", {
					"account": {
						"id": "1.2.7",
						"membership_expiration_date": "1969-12-31T23:59:59",
						"registrar": "1.2.7",
						"referrer": "1.2.7",
						"lifetime_referrer": "1.2.7",
						"network_fee_percentage": 2000,
						"lifetime_referrer_fee_percentage": 8000,
						"referrer_rewards_percentage": 0,
						"name": "harley",
						"owner": {
							"weight_threshold": 1,
							"account_auths": [],
							"key_auths": [
								["CYB8jM4e1z1xS63xxVhuperU1VvusQz8Ye8CrnTBigFUaErpigpPL", 1]
							],
							"address_auths": []
						},
						"active": {
							"weight_threshold": 1,
							"account_auths": [],
							"key_auths": [
								["CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR", 1]
							],
							"address_auths": []
						},
						"options": {
							"memo_key": "CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR",
							"voting_account": "1.2.5",
							"num_witness": 0,
							"num_committee": 0,
							"votes": [],
							"extensions": []
						},
						"statistics": "2.6.7",
						"whitelisting_accounts": [],
						"blacklisting_accounts": [],
						"whitelisted_accounts": [],
						"blacklisted_accounts": [],
						"cashback_vb": "1.13.0",
						"owner_special_authority": [0, {}],
						"active_special_authority": [0, {}],
						"top_n_control_flags": 0
					},
					"statistics": {
						"id": "2.6.7",
						"owner": "1.2.7",
						"most_recent_op": "2.9.176217607",
						"total_ops": 36289,
						"removed_ops": 35789,
						"total_core_in_orders": 0,
						"lifetime_fees_paid": "73433177259",
						"pending_fees": 0,
						"pending_vested_fees": 4002933
					},
					"registrar_name": "harley",
					"referrer_name": "harley",
					"lifetime_referrer_name": "harley",
					"votes": [],
					"cashback_balance": {
						"id": "1.13.0",
						"owner": "1.2.7",
						"balance": {
							"amount": "112298566510",
							"asset_id": "1.3.0"
						},
						"policy": [1, {
							"vesting_seconds": 31536000,
							"start_claim": "1970-01-01T00:00:00",
							"coin_seconds_earned": "1045287256041805458",
							"coin_seconds_earned_last_update": "2018-08-02T19:00:00"
						}]
					},
					"balances": [{
						"id": "2.5.11",
						"owner": "1.2.7",
						"asset_type": "1.3.0",
						"balance": 1226976574
					}],
					"vesting_balances": [{
						"id": "1.13.0",
						"owner": "1.2.7",
						"balance": {
							"amount": "112298566510",
							"asset_id": "1.3.0"
						},
						"policy": [1, {
							"vesting_seconds": 31536000,
							"start_claim": "1970-01-01T00:00:00",
							"coin_seconds_earned": "1045287256041805458",
							"coin_seconds_earned_last_update": "2018-08-02T19:00:00"
						}]
					}, {
						"id": "1.13.10",
						"owner": "1.2.7",
						"balance": {
							"amount": "2622780000",
							"asset_id": "1.3.0"
						},
						"policy": [1, {
							"vesting_seconds": 86400,
							"start_claim": "1970-01-01T00:00:00",
							"coin_seconds_earned": "226607328000000",
							"coin_seconds_earned_last_update": "2018-08-03T03:13:54"
						}]
					}],
					"limit_orders": [],
					"call_orders": [],
					"settle_orders": [],
					"proposals": [],
					"assets": [],
					"withdraws": []
				}]
			]
		}

#### get\_account\_by\_name

`Return the detail of account by name`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_account_by_name","params":["nathan"],"id":1}' https://apihk.cybex.io

* Parameters:

		name: account name

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"id": "1.2.25",
				"membership_expiration_date": "1969-12-31T23:59:59",
				"registrar": "1.2.25",
				"referrer": "1.2.25",
				"lifetime_referrer": "1.2.25",
				"network_fee_percentage": 2000,
				"lifetime_referrer_fee_percentage": 8000,
				"referrer_rewards_percentage": 0,
				"name": "nathan",
				"owner": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB6iPuTiBpghi4Dh8phPeAUTofNa3DZK4HjugTpduA48kLXxfKZd", 1]
					],
					"address_auths": []
				},
				"active": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB7pb9B2J3D6NXRmsaXNnBZr5AVnH5nLNtrti3ADZfgYpRn7ihQQ", 1]
					],
					"address_auths": []
				},
				"options": {
					"memo_key": "CYB7pb9B2J3D6NXRmsaXNnBZr5AVnH5nLNtrti3ADZfgYpRn7ihQQ",
					"voting_account": "1.2.5",
					"num_witness": 0,
					"num_committee": 0,
					"votes": [],
					"extensions": []
				},
				"statistics": "2.6.25",
				"whitelisting_accounts": [],
				"blacklisting_accounts": [],
				"whitelisted_accounts": [],
				"blacklisted_accounts": [],
				"cashback_vb": "1.13.1",
				"owner_special_authority": [0, {}],
				"active_special_authority": [0, {}],
				"top_n_control_flags": 0
			}
		}

#### get\_account\_references

`all accounts that referr to the key or account id in their owner or active authorities`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_account_references","params":["1.2.7"],"id":1}' https://apihk.cybex.io

* Parameters:

		account_id : account id

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": ["1.2.0", "1.2.1", "1.2.2"]
		}

#### lookup\_account\_names

`The accounts holding the provided names`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"lookup_account_names","params":[["nathan", "harley"]],"id":1}' https://apihk.cybex.io

* Parameters:

		account_names: Names of the accounts to retrieve

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.2.25",
				"membership_expiration_date": "1969-12-31T23:59:59",
				"registrar": "1.2.25",
				"referrer": "1.2.25",
				"lifetime_referrer": "1.2.25",
				"network_fee_percentage": 2000,
				"lifetime_referrer_fee_percentage": 8000,
				"referrer_rewards_percentage": 0,
				"name": "nathan",
				"owner": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB6iPuTiBpghi4Dh8phPeAUTofNa3DZK4HjugTpduA48kLXxfKZd", 1]
					],
					"address_auths": []
				},
				"active": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB7pb9B2J3D6NXRmsaXNnBZr5AVnH5nLNtrti3ADZfgYpRn7ihQQ", 1]
					],
					"address_auths": []
				},
				"options": {
					"memo_key": "CYB7pb9B2J3D6NXRmsaXNnBZr5AVnH5nLNtrti3ADZfgYpRn7ihQQ",
					"voting_account": "1.2.5",
					"num_witness": 0,
					"num_committee": 0,
					"votes": [],
					"extensions": []
				},
				"statistics": "2.6.25",
				"whitelisting_accounts": [],
				"blacklisting_accounts": [],
				"whitelisted_accounts": [],
				"blacklisted_accounts": [],
				"cashback_vb": "1.13.1",
				"owner_special_authority": [0, {}],
				"active_special_authority": [0, {}],
				"top_n_control_flags": 0
			}, {
				"id": "1.2.7",
				"membership_expiration_date": "1969-12-31T23:59:59",
				"registrar": "1.2.7",
				"referrer": "1.2.7",
				"lifetime_referrer": "1.2.7",
				"network_fee_percentage": 2000,
				"lifetime_referrer_fee_percentage": 8000,
				"referrer_rewards_percentage": 0,
				"name": "harley",
				"owner": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB8jM4e1z1xS63xxVhuperU1VvusQz8Ye8CrnTBigFUaErpigpPL", 1]
					],
					"address_auths": []
				},
				"active": {
					"weight_threshold": 1,
					"account_auths": [],
					"key_auths": [
						["CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR", 1]
					],
					"address_auths": []
				},
				"options": {
					"memo_key": "CYB7UdEEgrFYAR4DfSi8svfUUzE2E2twSFqAx66LqmgxXJjk5h7LR",
					"voting_account": "1.2.5",
					"num_witness": 0,
					"num_committee": 0,
					"votes": [],
					"extensions": []
				},
				"statistics": "2.6.7",
				"whitelisting_accounts": [],
				"blacklisting_accounts": [],
				"whitelisted_accounts": [],
				"blacklisted_accounts": [],
				"cashback_vb": "1.13.0",
				"owner_special_authority": [0, {}],
				"active_special_authority": [0, {}],
				"top_n_control_flags": 0
			}]
		}

#### lookup\_accounts

`Map of account names to corresponding IDs`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"lookup_accounts","params":["nathan", 7],"id":1}' https://apihk.cybex.io

* Parameters:

		lower_bound_name: Lower bound of the first name to return
		limit: Maximum number of results to return must not exceed 1000
		
* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [
				["nathan", "1.2.25"],
				["natu61", "1.2.10857"],
				["nau529", "1.2.13494"],
				["nauhc6", "1.2.36394"],
				["nawuh925", "1.2.27623"],
				["naya1008", "1.2.29372"],
				["nayayan12", "1.2.16352"]
			]
		}

#### get\_account\_count

`Get the total number of accounts registered with the blockchain.`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_account_count","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": 36884
		}

### 5. 余额

#### get\_account\_balances**

`Get an account’s balances in various assets.`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_account_balances","params":["1.2.7", []],"id":1}' https://apihk.cybex.io

* Parameters:

		id: ID of the account to get balances for
		assets: IDs of the assets to get balances of; if empty, get all assets account has a balance in

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"amount": 1225975847,
				"asset_id": "1.3.0"
			}]
		}

#### get\_named\_account\_balances

`Semantically equivalent to get_account_balances, but takes a name instead of an ID, return balances of the account`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_named_account_balances","params":["harley", ["1.3.0"]],"id":1}' https://apihk.cybex.io

* Parameters:

		name: account name
		asset_ids: asset ids

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"amount": 1224975115,
				"asset_id": "1.3.0"
			}]
		}

#### get\_vesting\_balances

`Return vesting balances`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_vesting_balances","params":["1.2.7"],"id":1}' https://apihk.cybex.io

* Parameters:

		account_id: account id

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.13.0",
				"owner": "1.2.7",
				"balance": {
					"amount": "112298566510",
					"asset_id": "1.3.0"
				},
				"policy": [1, {
					"vesting_seconds": 31536000,
					"start_claim": "1970-01-01T00:00:00",
					"coin_seconds_earned": "1045287256041805458",
					"coin_seconds_earned_last_update": "2018-08-02T19:00:00"
				}]
			}, {
				"id": "1.13.10",
				"owner": "1.2.7",
				"balance": {
					"amount": "2623960000",
					"asset_id": "1.3.0"
				},
				"policy": [1, {
					"vesting_seconds": 86400,
					"start_claim": "1970-01-01T00:00:00",
					"coin_seconds_earned": "226709280000000",
					"coin_seconds_earned_last_update": "2018-08-03T04:18:30"
				}]
			}]
		}

### 6. 资产

#### get\_assets

`Return the detail of assets corresponding to the provided IDs`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_assets","params":[["1.3.0"]],"id":1}' https://apihk.cybex.io

* Parameters:

		asset_ids: IDs of the assets to retrieve

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.3.0",
				"symbol": "CYB",
				"precision": 5,
				"issuer": "1.2.3",
				"options": {
					"max_supply": "100000000000000",
					"market_fee_percent": 0,
					"max_market_fee": "1000000000000000",
					"issuer_permissions": 0,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 1,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1,
							"asset_id": "1.3.0"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.0"
			}]
		}

#### list\_assets

`Return the assets found`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"list_assets","params":["CYB", 3],"id":1}' https://apihk.cybex.io

* Parameters:

		lower_bound_symbol: Lower bound of symbol names to retrieve
		limit: Maximum number of assets to fetch (must not exceed 100)

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.3.0",             
				"symbol": "CYB",           
				"precision": 5,
				"issuer": "1.2.3",
				"options": {
					"max_supply": "100000000000000",
					"market_fee_percent": 0,
					"max_market_fee": "1000000000000000",
					"issuer_permissions": 0,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 1,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1,
							"asset_id": "1.3.0"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.0"
			}, {
				"id": "1.3.503",
				"symbol": "Cybex",
				"precision": 6,
				"issuer": "1.2.29",
				"options": {
					"max_supply": "100000000000",
					"market_fee_percent": 0,
					"max_market_fee": 0,
					"issuer_permissions": 79,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 100000,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1000000,
							"asset_id": "1.3.503"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "{\"main\":\"It's Cybex!\",\"market\":\"\"}",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.503"
			}, {
				"id": "1.3.1",
				"symbol": "JADE",
				"precision": 6,
				"issuer": "1.2.29",
				"options": {
					"max_supply": "1000000000000000",
					"market_fee_percent": 0,
					"max_market_fee": 0,
					"issuer_permissions": 79,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 200000000,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1000000,
							"asset_id": "1.3.1"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "{\"main\":\"It's gateway name.\",\"market\":\"\"}",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.1"
			}]
		}

#### lookup\_asset\_symbols

`The assets corresponding to the provided symbols or IDs`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"lookup_asset_symbols","params":[["CYB", "JADE.ETH"]],"id":1}' https://apihk.cybex.io

* Parameters:

		asset_symbols: Symbols or stringified IDs of the assets to retrieve

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.3.0",
				"symbol": "CYB",
				"precision": 5,
				"issuer": "1.2.3",
				"options": {
					"max_supply": "100000000000000",
					"market_fee_percent": 0,
					"max_market_fee": "1000000000000000",
					"issuer_permissions": 0,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 1,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1,
							"asset_id": "1.3.0"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.0"
			}, {
				"id": "1.3.2",
				"symbol": "JADE.ETH",
				"precision": 6,
				"issuer": "1.2.29",
				"options": {
					"max_supply": "1000000000000000",
					"market_fee_percent": 0,
					"max_market_fee": 0,
					"issuer_permissions": 79,
					"flags": 0,
					"core_exchange_rate": {
						"base": {
							"amount": 100000000,
							"asset_id": "1.3.0"
						},
						"quote": {
							"amount": 1000000,
							"asset_id": "1.3.2"
						}
					},
					"whitelist_authorities": [],
					"blacklist_authorities": [],
					"whitelist_markets": [],
					"blacklist_markets": [],
					"description": "{\"main\":\"It's an ETH IOU of Jade gateway.\",\"market\":\"\"}",
					"extensions": []
				},
				"dynamic_asset_data_id": "2.3.2"
			}]
		}

### 7. 市场/喂价

#### get\_order\_book

`Returns the order book for the market base:quote`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_order_book","params":["CYB", "JADE.ETH", 3],"id":1}' https://apihk.cybex.io

* Parameters:

		base: String name of the first asset
		quote: String name of the second asset
		depth: of the order book. Up to depth of each asks and bids, capped at 50. Prioritizes most moderate of each

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"base": "CYB",
				"quote": "JADE.ETH",
				"bids": [{
					"price": "2003.106324701195219123",
					"quote": "0.016065",
					"base": "32.17991"
				}, {
					"price": "2000",
					"quote": "0.973636",
					"base": "1947.27200"
				}, {
					"price": "1983.700198370019837001",
					"quote": "0.083682",
					"base": "166"
				}],
				"asks": [{
					"price": "2008.716065865598575878",
					"quote": "0.000001",
					"base": "0.00200"
				}, {
					"price": "2173.865785526401599965",
					"quote": "0.460010",
					"base": "1000"
				}, {
					"price": "2173.913043478260869565",
					"quote": "0.920000",
					"base": "2000"
				}]
			}
		}

#### get\_limit\_orders

`Get limit orders in a given market`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_limit_orders","params":["1.3.0", "1.3.2", 1],"id":1}' https://apihk.cybex.io

* Parameters:

		a: ID of asset being sold
		b: ID of asset being purchased
		limit: Maximum number of orders to retrieve

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.7.66979388",
				"expiration": "2023-08-03T10:38:43",
				"seller": "1.2.5132",
				"for_sale": 493347144,
				"sell_price": {
					"base": {
						"amount": 500000000,
						"asset_id": "1.3.0"
					},
					"quote": {
						"amount": 2450000,
						"asset_id": "1.3.2"
					}
				},
				"deferred_fee": 0
			}, {
				"id": "1.7.66983772",
				"expiration": "2023-08-03T10:48:20",
				"seller": "1.2.19408",
				"for_sale": 1912008,
				"sell_price": {
					"base": {
						"amount": 1913687,
						"asset_id": "1.3.2"
					},
					"quote": {
						"amount": 414200000,
						"asset_id": "1.3.0"
					}
				},
				"deferred_fee": 0
			}]
		}	

#### get\_ticker

`Return the market ticker for the past 24 hours`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_ticker","params":["JADE.ETH", "CYB"],"id":1}' https://apihk.cybex.io

* Parameters:

		a: String name of the first asset
		b: String name of the second asset

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"time": "2018-08-06T03:08:16",
				"base": "JADE.ETH",
				"quote": "CYB",
				"latest": "0.000487188654353562",
				"lowest_ask": "0.000487188654353562",
				"highest_bid": "0.00048",
				"percent_change": "1.49",
				"base_volume": "225.62666",
				"quote_volume": "460960.32089"
			}
		}

#### get\_24\_volume

`The market volume over the past 24 hours`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_24_volume","params":["CYB", "JADE.ETH"],"id":1}' https://apihk.cybex.io

* Parameters:

		a: String name of the first asset
		b: String name of the second asset
* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"time": "2018-08-06T02:56:44",
				"base": "CYB",
				"quote": "JADE.ETH",
				"base_volume": "462814.14025",
				"quote_volume": "226.514174"
			}
		}

#### get\_trade\_history

`Return recent transactions in the market`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_trade_history","params":["CYB", "JADE.ETH", "2018-08-05T00:00:00", "2018-08-04T00:00:00", 3],"id":1}' https://apihk.cybex.io

* Parameters:

		a: String name of the first asset
		b: String name of the second asset
		stop: Stop time as a UNIX timestamp
		limit: Number of trasactions to retrieve, capped at 100
		start: Start time as a UNIX timestamp

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"sequence": 7757291,
				"date": "2018-08-04T23:47:42",
				"price": "2012.072434607645875251",
				"amount": "0.019422",
				"value": "39.07847",
				"side1_account_id": "1.2.34536",
				"side2_account_id": "1.2.34016"
			}, {
				"sequence": 7757289,
				"date": "2018-08-04T23:46:48",
				"price": "2012.072434607645875251",
				"amount": "0.009148",
				"value": "18.40643",
				"side1_account_id": "1.2.34536",
				"side2_account_id": "1.2.34007"
			}, {
				"sequence": 7757287,
				"date": "2018-08-04T23:46:36",
				"price": "2012.072434607645875251",
				"amount": "0.004692",
				"value": "9.44064",
				"side1_account_id": "1.2.34536",
				"side2_account_id": "1.2.8307"
			}]
		}

### 8. 见证人节点

#### get\_witnesses

`Get a list of witnesses by ID`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_witnesses","params":[["1.6.1"]],"id":1}' https://apihk.cybex.io

* Parameters:

		witness_ids: IDs of the witnesses to retrieve
		
* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.6.1",
				"witness_account": "1.2.6",
				"last_aslot": 10605553,
				"signing_key": "CYB8iWrVHcBaq74mqA6Dd58yeN7pEgmvBLrhYB1HSQRj8UBrHec5u",
				"pay_vb": "1.13.2",
				"vote_id": "1:0",
				"total_votes": "280493540206",
				"url": "",
				"total_missed": 3096,
				"last_confirmed_block_num": 4621255
			}]
		}
		
#### get\_witness\_by\_account

`Return the witness object, or null if the account does not have a witness`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_witness_by_account","params":["1.2.7"],"id":1}' https://apihk.cybex.io

* Parameters:

		account: The ID of the account whose witness should be retrieved

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"id": "1.6.2",
				"witness_account": "1.2.7",
				"last_aslot": 10605630,
				"signing_key": "CYB8Vfkpqn6B35xTMCwhqUuGDHuTfdmo368zDWT4xz2MZmy368Gvh",
				"pay_vb": "1.13.10",
				"vote_id": "1:1",
				"total_votes": "280484687773",
				"url": "",
				"total_missed": 542713,
				"last_confirmed_block_num": 4621332
			}
		}

#### lookup\_witness\_accounts

`Get names and IDs for registered witnesses`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"lookup_witness_accounts","params":["harley", 15],"id":1}' https://apihk.cybex.io

* Parameters:

		lower_bound_name: Lower bound of the first name to return
		limit: Maximum number of results to return must not exceed 1000

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [
				["harley", "1.6.2"],
				["jupiter", "1.6.3"],
				["mars", "1.6.4"],
				["mercury", "1.6.5"],
				["moon", "1.6.6"],
				["neptune", "1.6.7"],
				["saturn", "1.6.8"],
				["sun", "1.6.9"],
				["uranus", "1.6.10"],
				["venus", "1.6.11"]
			]
		}

#### get\_witness\_count

`Get the total number of witnesses registered with the blockchain`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_witness_count","params":[],"id":1}' https://apihk.cybex.io

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": 11
		}

### 9. 理事会成员

#### get\_committee\_members

`Get a list of committee_members by ID`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_committee_members","params":[["1.5.1"]],"id":1}' https://apihk.cybex.io

* Parameters:

		committee_member_ids: IDs of the committee_members to retrieve

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [{
				"id": "1.5.1",
				"committee_member_account": "1.2.17",
				"vote_id": "0:12",
				"total_votes": "241189234794",
				"url": ""
			}]
		}

#### get\_committee\_member\_by\_account

`Get the committee_member owned by a given account`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"get_committee_member_by_account","params":["1.2.7"],"id":1}' https://apihk.cybex.io

* Parameters:

		account: The ID of the account whose committee_member should be retrieved

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": {
				"id": "1.5.12",
				"committee_member_account": "1.2.7",
				"vote_id": "0:23",
				"total_votes": "280482493905",
				"url": ""
			}
		}

#### lookup\_committee\_member\_accounts

`Get names and IDs for registered committee_members`

* Request:

		curl --data '{"jsonrpc":"2.0","method":"lookup_committee_member_accounts","params":["1.5.1", 22],"id":1}' https://apihk.cybex.io

* Parameters:
		
		lower_bound_name: Lower bound of the first name to return
		limit: Maximum number of results to return must not exceed 1000

* Response:

		{
			"id": 1,
			"jsonrpc": "2.0",
			"result": [
				["april", "1.5.1"],
				["avir", "1.5.2"],
				["bigerjoe", "1.5.3"],
				["bigjoe", "1.5.4"],
				["celtics-tatum", "1.5.5"],
				["earth", "1.5.11"],
				["fairystar", "1.5.6"],
				["harley", "1.5.12"],
				["jupiter", "1.5.13"],
				["kvir", "1.5.7"],
				["lucifer", "1.5.8"],
				["mars", "1.5.14"],
				["mercury", "1.5.15"],
				["moon", "1.5.16"],
				["nathan", "1.5.0"],
				["neptune", "1.5.17"],
				["processon", "1.5.9"],
				["saturn", "1.5.18"],
				["sun", "1.5.19"],
				["tina", "1.5.10"],
				["uranus", "1.5.20"],
				["venus", "1.5.21"]
			]
		}

### 10. 授权和验证

#### get\_required\_fees

For each operation calculate the required fee in the specified asset type.

```
vector<fc::variant> graphene::app::database_api::get_required_fees(const vector<operation> &ops, asset_id_type id) const
```

* Parameters:

```
ops: operation whose fee should be get
id: asset id which the fee should be calculated in
```

The following sample is to get the fee of `limit_order_create` operation.

* Request:

```
{
    "jsonrpc": "2.0",
    "method": "get_required_fees",
    "params": [
        [
            [
                1,
                {
                    "fee": {
                        "amount": 0,
                        "asset_id": "1.3.0"
                    },
                    "seller": "1.2.37471",
                    "amount_to_sell": {
                        "amount": 100,
                        "asset_id": "1.3.0"
                    },
                    "min_to_receive": {
                        "amount": 1,
                        "asset_id": "1.3.2"
                    },
                    "expiration": "2018-10-25T08:58:48",
                    "fill_or_kill": false,
                    "extensions": []
                }
            ]
        ],
        "1.3.0"
    ],
    "id": 1
}
```

* Response:

```
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": [{
        "amount": 55,
        "asset_id": "1.3.0"
    }]
}
```

## 其他API

### 账户历史API

**Account History API**

http://docs.bitshares.org/api/history.html#id1

### 网络广播API

**NETWORK BROADCAST API**

http://docs.bitshares.org/api/network_broadcast.html

### 网络节点API

**NETWORK NODES API**

http://docs.bitshares.org/api/network_node.html


#应用场景

##Cybex网关

**网关是一个能够加快价值进出Cybex网络的可信任交易方。为实现这个目的，网关发行可赎回的借据，作为Cybex区块链上的可替代可分割资产。如果发行人可靠，这些借据将会以面值交易，从而在交易中产生作用。就像银行发行汇票那样， 第三方虽然不会了解彼此，但可以接受支付而无需担忧个人支票透支。由于可靠的协议能够加快第三方交易速度，使这项服务很有成效。和银行发行汇票的不同之处是，数字借据由网关发行，更加灵活好用。**

Cybex网络允许任何人发行数字资产，即UIA。这些资产能够用于记录公司股份，存款，及其他用途。发行的资产可以跟Cybex网络上包括bitUSD等完全抵押市场锚定资产在内的其他类型资产自由交易。

本文将概述运作Cybex网关的商业机会。网关起到引入外部资产（如法币、黄金、白银，比特币等）到Cybex网络的桥梁作用。绝大部分价值必须通过网关或交易所进出Cybex网络，因而网关成为了Cybex能否成功的关键因素。

* 网关VS交易所

大多数在加密货币圈中浸淫过一阵儿的人，对交易所都不会陌生。交易所允许两种类型资产的储蓄，维护订单本，使用存入的资金加快资产间交易。一单在交易所的存款，就是一份通往由交易所掌管的专用数据库的借据。

网关只需行使交易所三项职能中的其中一项：接受存款并发行借据。网关不需要维护用来记录账户余额的内部数据库，也不用维护订单。网关的工作极度简化，从而最大限度地减少了有关各方的风险。

时至今日，当你想用比特币兑换美元，你必须同时将这二者存到可靠的第三方手上。一旦第三方像Mt.Gox那样不靠谱，你就能明白自己账上的比特币会遭受些什么了：被偷，遗失，被盗用亦或被没收。如果Mt.Gox只是一个简单的美元网关，那么仅仅美元的存款会处于危险之中，因为所有的订单和Cybex都会在用户密钥的控制下保存在区块链上。

正如你们从Mt.Gox上看到的，网关能够将交易所具有的弊端减少50%，同时用户能够从一份不限制Cybex取款的全球订单上获益。

* 成为网关，创造收入

通过为Cybex社区提供有价值的服务以赢得收益，这是成为网关的主要理由。网关有很多盈利方式，最简单的一种是每次使用服务时收取交易手续费。你也可以在用户每次转移你的资产到Cybex网络时收取费用，以及收取网络上面的市场费。除去各类交易费，网关还有机会代表他们的客户通过浮动收益创收。

* 它是如何工作的

每个单独的比特币交易所已经行使了类似网关的作用。用户传输资金到交易所，交易所记录其欠了用户多少，当用户要求提现的时候交易所就将资金返还。用户可以快速而安全的和自己同一间交易所的用户交易。

Cybex网关的工作流程与之相同，唯一区别就是跟踪用户储蓄信息的数据库是Cybex区块链。当用户通过网关交易法币或比特币，网关会回应一个由自身发行的借据资产给用户。当用户返回借据给网关时，网关就会将法币或比特币还给用户。

借据资产对Cybex生态圈内的用户而言非常有用，因为它能作为一个非常灵活的多方代管资产。用户可以通过你的网关使用美元借据来交易bitUSD或来自其他网关的借据。通常，用户会用几乎没有合约风险的bitUSD保值，但是当需要将bitUSD返还为法币的时候仍要通过网关来实现。

* 合规性

在绝大多数国家，任何运营存款和汇款业务的公司都会被严格管理。这些规章包括必要的许可证、契约、保险、KYC（了解你的客户），以及AML（反洗钱）合规等等。Cybex区块链提供了网关，包含遵守这些规定的全部工具。

如果你已经开始了这庄生意，随即你需要维护一个追踪用户存款的数据库，它可能记录每个独立的交易和余额变化。你的数据库在任何时间点都能准确的知道谁欠了什么。在执法需求下，你也拥有扣押或冻结用户余额的能力。Cybex还提供包括以下特性：

1）具有可以控制你资产余额的白名单公钥的能力。
2）具有冻结所有资金和停止所有你资产交易的能力。
3）具有从任意用户转移任何数量你资产余额到任意用户的能力。

换句话说，如果你正在运营一间合法的加密货币交易所，你可以很容易扩展事业成为Cybex网关。

* 安全性

作为网关你应该很关心代表客户持有的资金的安全性。幸运的是从一个定制的内部系统转移你的数据库到Cybex网络会给你的生意带来很大的安全性提升，因为你所有的利润都来自区块链。每次转账都会记录在一个公共账簿上，每次资产转让被分布在全世界的签署和核对。开源代码在很多不同的网关中得到使用，可以得到整个行业的评审。你大可放心，没有资金会在未经所有人私钥的正确签署下就被转移。

作为用户资产的创建者，你拥有完全的自由去兑现现金，或为社会工程及泄露用户密钥的偷窃进行反向交易。无人能够在没有交易历史记录的前提下取出真金白银。Cybex代码内的任何bug不会使你的网关丢失金钱，因为您保留着如何兑现您资产的权力。

跟交易所比较，成为网关在安全性方面最大的进步或许是你只需要处理单个资产。不像在交易所要同时拥有用户的法币和加密资产，你可以仅允许法币存款并发行法币的借据。Cybex区块链能够让用户直接和其他用户交易，而无须给你转账资产并由此创造了额外的负担给你的交易所。

作为网关你所面对的最大安全责任是严防控制用户发行资产的私钥访问。基于这个原因，所有的用户发行资产被同时拥有冷密钥和热密钥的N/M多重签名系统控制。在网关里，你能在大量不同的个体中分开访问控制以保证没有个人或一个被感染的机器能够入侵控制你的用户发行资产。这是媲美交易所保护代表客户的加密货币私钥的安全防范机制。仔细守护这些密钥并将其保存在冷存储中是至关重要的。

访问用户发行资产，是和保证绝对控制、需要冷存储的主人密钥，及保存在安全热系统上的管理密钥分开的。每个管理密钥的动作都能被主人密钥撤销。这样的安排提供了控制的多级组织和灵活性的最大化。

* 无限商机

Cybex生态系统很年轻，在快速成长。能够建立起信任和提供可靠服务的早期网关，将有可能成为默认的人们常去的网关，拥有一个最大限度提高交易量的机会，由此收取手续费。在前期就参与进来的也会在我们的用户群中增加对网关公司的认识。可能开始作为一个网关，你也能发展其他的相关服务给Cybex生态系统，如托管钱包，或者甚至成为代理人。

### 搭建网关

**网关是一个能够加快价值进出Cybex网络的可信任交易方。为实现这个目的，网关发行可赎回的借据（以下简称IOU），作为Cybex区块链上的可替代可分割资产。**

由网关发行的IOU可以在Cybex网络内的交易中产生作用，并有网关负责承兑至持有人的外部网络账户。网关通过将外部资产（如比特币或在其他主链发行的通证）引入Cybex网络，起到了进出Cybex网络的桥梁作用。

因此，网关的工作主要包含以下几个方面：

1）`创建资产`，即Cybex内盘IOU；    
2）`发行资产`，将IOU发放至网关Cybex账户；    
3）接收用户外部资产，将对应外部资产的内盘IOU`转账`至用户Cybex账户；    
4）回收用户Cybex账户内盘IOU，`转账`相应的外部资产至用户外部账户。    

其中，3、4）相当于用户发起的充值、提现操作。

注意：Cybex网关是一个中心化的服务，上述与Cybex主链交互的操作之外，需要根据网关前置场景的业务逻辑，对内外盘资产的承兑汇率、交易黑白名单等进行设置。同时，为了确保内外盘资产的对应关系，建议网关设计中包含内外部审计的相关逻辑，以避免超发带来的承兑风险。

### 创建资产

创建Cybex内盘IOU需要通过[asset_create](https://bitshares.org/doxygen/structgraphene_1_1chain_1_1asset__create__operation.html)操作实现，操作说明如下：

| **参数** | **类型** | **必填** | **描述** | **示例** |
| --- | --- | --- | --- | --- |
| fee | asset | Y | 手续费 | 详见[如何获取手续费信息](https://github.com/CybexDex/Cybex-node-doc/wiki/常见问题) |
| symbol | string | Y | 资产符号，大于等于5的资产，手续费为500CYB | 为了区别各网关发布的资产，建议命名格式遵循“网关简称.资产符号”的格式，如JADE.ETH，表示瑶池创建并负责承兑的ETH |
| precision | unit8_t | Y | 资产精度，小数点后位数，填写小于等于12的正整数 | 详见[金额和价格如何表示](https://github.com/CybexDex/Cybex-node-doc/wiki/常见问题) |
| common_options | asset_options | Y | 资产设置 | 详见下表“common_options参数” |
| bitasset_opts | bitasset_options | N | 智能资产设置 | 网关发行的IOU一般不使用智能资产 |
| is_prediction_market | bool | Y | 是否为预测市场 | 如果该资产为[预测市场的资产](https://github.com/CybexDex/Cybex-node-doc/wiki/资产-通证)则设置为true |
| extensions | extensions_type | N | 扩展字段 |  |

**common_options参数**

| **参数** | **类型** | **必填** | **描述** | **示例** |
| --- | --- | --- | --- | --- |
| max_supply | share_type | Y | 最大发行量，所有发行操作的资产总和不能大于该值 | 由于总量无法修改，Cybex也不支持发行符号相同的资产，建议将该参数设置的越大越好，如10000000000000，以便应对外部资产持续增加带来的内盘资产需求。 |
| market_fee_percent | uint16_t | Y | 交易手续费 | 使用该资产进行交易时，除了交易操作本身的手续费外，是否需要额外收取手续费。手续费以交易额的百分比计算，如20，表示交易额的20%为手续费 |
| max_market_fee | share_type | Y | 交易手续费上限 |  |
| issuer_permissions | uint16_t | Y | 发行人在发行操作后，可以对哪些权限进行调整 | 详见下表“asset_issuer_permission_flags” |
| flags | uint16_t | Y | 当前激活的flag | 详见下表“asset_issuer_permission_flags” |
| core_exchange_rate | price | Y | 核心汇率 | 详见下表“core_exchange_rate参数” |
| whitelist_authorities | flat_set<account_id_type> | Y | 资产持有账户ID白名单 | 需要限制持有账户时使用，如1.2.4532 |
| blacklist_authorities | flat_set<account_id_type> | Y | 资产持有账户ID黑名单 | 需要排除持有账户时使用，如1.2.3442 |
| whitelist_markets | flat_set<asset_id_type> | Y | 交易对手资产ID白名单 | 需要限制交易市场时使用，如1.3.2，表示仅可以与1.3.2的资产进行交易 |
| blacklist_markets | flat_set<asset_id_type> | Y | 交易对手资产ID黑名单 | 需要排除交易市场时使用，如1.3.3，表示不能与1.3.3的资产进行交易 |
| description | string | Y | 资产描述 |  |
| extensions | extensions_type | N | 扩展字段 |  |

**asset_issuer_permission_flags**

| **参数** | **类型** | **必填** | **描述** | **示例** |
| --- | --- | --- | --- | --- |
| charge_market_fee | bool | Y | 是否设置了交易手续费 |  |
| white_list | bool | Y | 是否要求持有人加入白名单 |  |
| override_authority  | bool | Y | 是否允许资产发行人在未征得持有人同意的情况下，强制将资产从A账户转账至B账户 |  |
| transfer_restricted  | bool | Y | 是否强制资产发行人参与每笔交易，即每次转账操作中必须有一方为资产发行人 |  |
| disable_force_settle | bool | Y | 是否禁用强制清算 |  |
| global_settle | bool | Y | 是否允许全局清算 |  |
| disable_confidential | bool | Y | 是否禁用隐私交易 |  |
| witness_fed_asset | bool | Y | 是否由见证人节点喂价 |  |
| committee_fed_asset | bool | Y | 是否由委员会喂价 |  |

**core_exchange_rate参数**

| **参数** | **类型** | **必填** | **描述** | **示例** |
| --- | --- | --- | --- | --- |
| base | asset | Y | 基准资产信息 |  |
| amount | share_type | Y | 汇率计算的分母 | 10 |
| asset_id | asset_id_type | Y | 基准资产ID，通常设置为Cybex的核心资产，即CYB | 1.3.0 |
| quote | asset | Y | 新建资产信息 |  |
| amount | share_type | Y | 汇率计算的分子 | 10 |
| asset_id | asset_id_type | Y | 新建资产ID，即当前资产的ID | 1.3.0 |

**Python示例**

https://github.com/CybexDex/Cybex-node-doc/blob/master/transaction/python/advanced/create_asset.md

### 发行资产

创建Cybex内盘IOU后，需要通过[asset_issue](https://bitshares.org/doxygen/structgraphene_1_1chain_1_1asset__issue__operation.html)操作进行资产发行，操作说明如下：

| **参数** | **类型** | **必填** | **描述** | **示例** |
| --- | --- | --- | --- | --- |
| fee | asset | Y | 手续费 |  |
| issuer | account_id_type | Y | 资产发行人账户ID |  |
| asset_to_issue | asset | Y | 发行的资产（包含数量、资产ID） |  |
| issue_to_account | account_id_type  | Y | 发行资产的初始归集账户，即操作执行后，本次发行的资产会由哪个账户持有。一般来说，由于网关负责承兑资产，内盘IOU的初始归集账户为网关账户 |  |
| memo | memo_data | N | 备注 |  |
| extensions | extensions_type | N | 扩展字段 |  |

### 普通转账

资产进出Cybex网络的过程，需要通过[transfer](https://bitshares.org/doxygen/structgraphene_1_1chain_1_1transfer__operation.html)操作实现`网关账户`和`用户账户`的双向转账功能。操作参数如下：

| **参数**   | **类型**   | **必填**   | **描述**   | **示例**   | 
|:----|:----|:----|:----|:----|
| fee   | asset   | Y   | 手续费   |    | 
|     amount   | share_type   | Y   | 手续费金额   | 55   | 
|     asset_id   | asset_id_type   | Y   | 资产ID   | 1.3.0   | 
| from   | account_id_type   | Y   | 付款账户ID   | 1.2.37471   | 
| to | account_id_type | Y | 收款账户ID | 1.2.83 | 
| amount   | asset   | Y   | 转账   |    | 
|     amount   | share_type   | Y   | 转账金额   | 10000   | 
|     asset_id | asset_id_type | Y | 资产ID | 1.3.0 | 
| memo   | memo_data   | N   | 备注   |    | 
| extension   | extension_type   | N   | 扩展字段   |    | 

用户发起充值、提现操作后，网关需要根据操作上链的结果，执行后续流程。例如：用户将外部资产抵押给网关后，网关将IOU从网关Cybex账户转账至用户Cybex账户，基于到账情况，网关对该笔充值订单的状态进行更新。

**重点关注**

为完成正确的网关出入金操作，需要对网关账户的资金往来加以监控。监控网关账户交易状态有多种方法，较为经济可靠的方式之一，是在每次新区块生成后，调用 [get_account_history](https://bitshares.org/doxygen/classgraphene_1_1app_1_1history__api.html#a70b3af7edeea81534a4db9e83b2cbd88) API来获取网关账户的操作记录的最新列表，对于其中新出现的操作记录加以处理。但在加以处理之前，最好确认该笔操作记录已经完全被链上确认。

Cybex链遵循DPOS共识机制约束以产生和确认区块。任何一笔transaction交易由钱包发送至节点并加以广播后，虽会以较快的速度被打包上链，但此时刚刚上链的区块，仍属于待确认状态，该区块及其所含交易，仍有被回滚的可能。只有三分之二以上见证人节点确认此区块有效性后，才可认为该区块及其所含交易被正式记录在了Cybex链上。

因此，在取得最新的操作记录后，要将该记录所在区块高度，与当前系统最新的**不可逆区块高度**进行比对。要查询当前不可逆区块高度，可通过`get_dynamic_global_properties`接口获得当前系统最新状态对象，该对象的`last_irreversible_block_num`字段即为当前链上不可逆区块高度。仅当操作所在区块高度**低于**不可逆区块高度时，才应对该笔操作进行处理。

### 可选操作

* 注册Cybex账户

用户（包括网关账户）可以通过Cybex前端页面或者水龙头服务进行Cybex账户注册，也可以由网关获取前置应用场景的信息，代用户完成账户注册。

Cybex为所有第三方提供了一个公共的[水龙头服务模板](https://github.com/CybexDex/CybexFaucet)，第三方可以选择直接使用公共水龙头，或者参考该模板的实现方式，基于前置应用加入自定义的逻辑。

注意：使用水龙头服务中，填写的“注册人”账户需要成为[Cybex终生会员](https://github.com/CybexDex/Cybex-node-doc/wiki/会员等级)，目前Cybex终生会员的手续费为1000CYB。

##加密数字资产托管

数字货币借贷平台为比特币、以太币等主流加密数字货币的持有人，提供了数字资产借贷的综合场景。基于数字货币的抵押与借贷，在为数字货币持有者提供流动性的同时，平台通过借款利息获得收益。在触发风控预警规则时，逾期或者质押物贬值但未追加抵押的资产可以自动出售，从而避免传统互联网借贷贷后执行难的弊端，更好地保障投资人的利益。

Cybex网关及拓链的冷热钱包产品为借贷过程中涉及的平台及贷款人账户监管、抵押资产保管提供了完善的解决方案。同时，Cybex生态的交易所、RTE等产品也可以帮助平台加速抵押资产出售。

注意：Cybex的所有产品均仅提供数字货币支付通道（不支持法币充值、提现）产品方案均基于“抵押数字货币，借入数字货币”的业务场景展开。

### 中心化数字资产借贷

**借款申请**

借款人选择抵押资产币种和借入资产币种，填写借款额度、选择借款期限。平台根据币种、额度、期限和借款人资质，计算出抵押率及借款利率。

借款申请提交后，平台向借款人提供抵押币种的充值地址，用于接收借款人抵押资产的转账。同时，借款人向平台提交借入币种的打款地址。抵押转账完成后，借款人向平台发起申请确认。

**放款确认**

平台对抵押资产到账情况（币种、金额）及借款人资质（KYC、AML）进行核实后，对于审核通过的借款申请执行放款操作。

**正常还款**

平台向借款人提供借入币种的还款地址，借款人在还款日前（含当日）按照还款计划将当期应还本金及利息转入该地址。

**抵押资产赎回**

所有期数正常还款后，在借款到期日，平台将抵押资产返还抵押时的打款地址或其他借款人提交的接收地址。

**逾期/抵押物贬值**

由于数字货币抵押存在一定抵押物价值变化风险，对于触发LTV“预警线”的贷款，需要通知借款人追加抵押额或提前还款。当抵押物价值波动导致LTV到达“处置线”时，平台将采取强制平仓措施。
同样的，对于逾期的借款人，在平台追讨未果的情况下，也将通过强制平仓的方式保障平台利益。

### 去中心化数字资产借贷

基于C2C借贷平台的整体流程，Cybex可以通过以下环境对平台业务进行去中心化和资产托管：

**抵押/还款**

借款人在借贷平台进行注册时，由平台同时创建Cybex账户。瑶池针对借款人的每一笔贷款，生成一个独立的抵押资产转入地址，由网关将其与Cybex账户关联。用户通过对该地址充值，完成资产抵押操作。同时，每笔贷款也会关联至一个独立的还款地址，借款人可以将借入币种还款至该地址。
平台通过订阅该地址的余额变动，确认抵押资产或每期还款的到账情况。避免不同用户在同一币种进行充值时，使用平台方提供的同一地址，到账金额和借款人的对应关系需要平台人为介入确认。

**放款/赎回**

Cybex网关可以帮助在用户完成平台注册时，授权平台注册Cybex（区块链）账户，从而简化业务逻辑。后续的的放款和赎回操作无需用户另行提供地址，可以直接由瑶池转账至Cybex账户，通过借款人可以通过余额提现将资产转出。

**多币种支持**

平台通过私有化部署的瑶池可以一次性实现多币种的收付款功能。
由瑶池通过模块化接入各种加密数字货币，并根据各个区块链的地址规范生成、管理各个币种的地址及其私钥，节省了平台为了支持多币种借贷针对各个区块链的开发工作。

**账户管理**

对于集成网关的平台，仅需要保存平台账户与Cybex账户的关联，由网关负责建立、维护Cybex账户与各币种地址的对应关系。
瑶池基于地址维度向平台输出各币种的出入金情况，并提供可视化的后台管理系统，对各个地址和往来交易加以监控和管理。

**流动性管理**

借款人抵押的资产将会真实放在瑶池内， 瑶池种子密钥的持有人将负责整个托管账户的资金管理工作。平台可以对用户抵押资产的存放地址设置阈值，将地址内的资产转移至（瑶池）托管账户。
同时，平台可以根据流动性需求对托管账户余额设置阈值，瑶池内各币种均预留一定资产用于偿付借款人的日常赎回，其余金额均转出至冷钱包。

**资产安全**

通过在瑶池和悟空的软硬件对接，可以实现基于托管账户余额设置阈值实现冷热钱包的自动转账。在确保平台资金流动性的同时，将大额抵押资产存放在硬件冷钱包内。
此外，悟空企业版的多签功能，还能够帮助平台多部门人员对资金进行协同管理，避免大额资金由个人保管带来的管理风险。

### 资产托管方案

Cybex数字资产托管方案由三个部分组成：Cybex网关、拓链的瑶池系统及悟空硬件钱包（企业版）。

* 网关：Cybex主链与区块链应用的中间层，资金通过网关转入或转出Cybex网络，可以基于主链及业务需求进行定制化开发；
* 瑶池：热钱包，自动生成并管理充值地址，支持托管账户限额管理、限额内自动提现及超限人工审核等等；
* 悟空：冷钱包，多签名私钥由硬件生成，各签名方支持助记词备份，支持Bitcoin，Ethereum和所有ERC20规范的Token的多签名交易。
 
上述产品组合除了能够为C2C（Crypto-to-Crypto Lending）业务内大量的数字资产提供安全保障外，平台用户（含借款人和贷款人）通过授权由平台自动创建的Cybex账户，可以在Cybex交易所内利用RTE完成平仓、二次投资等资产处置环节。

**目标架构**

![目标架构](https://raw.githubusercontent.com/GabrielleZhou/CybexWiki/master/C2CLendingArchitecture.jpg)

**资金/信息流**

![资金信息流](https://raw.githubusercontent.com/GabrielleZhou/CybexWiki/master/C2CLendingInfoFlow.jpg)

* 资金流

Cybex账户相当于平台用户的多币种余额账户，由瑶池进行各币种的通道支持。余额账户内显示的金额，是网关基于用户存入的数字货币发行的存取凭证，真实的资产将抵押在瑶池内。因此，瑶池充当了平台资金池的角色。
为了对平台资产进行监管，Cybex将为每个数字资产借贷平台独立部署一套瑶池系统，该套瑶池的主账号即为平台托管账号。托管账户内的资金由托管方负责管理，平台方可以设置保留在托管账户内的资金水位。托管账户的余额在确保流动性的同时，也是平台方向托管方缴纳的备付金。

* 信息流

Cybex账户内的各币种余额，通过信息流向平台同步。平台向Cybex账户执行转账操作时，仅仅是转移了网关发放的存取凭证，托管账户内的真实资金并未发生转移。只有在用户进行提现操作时，瑶池内的数字资产才会转入用户的提币地址。
对于借款人而言，在Cybex账户余额变动时，虽然未执行提现操作，但抵押资产的赎回已经完成。因此，平台可以在后续创建的其他金融场景内引导用户使用余额。同时，Cybex社区的其他应用，如DEX交易所、RTE等，也可以为平台及平台用户提供服务。


##运营见证人节点

###环境配置

* 4核及以上
* 16GByte内存及以上
* 500G文件系统存储及以上（随时间变化可能需要增加）
* 10Mbit/s带宽及以上（随交易量增加可能需要增加）
* Linux操作系统（推荐使用Ubuntu最新发行版本）
* 系统基础库
  * libcurl
  * libssl
  * openssl
  * libcrypto
  * 其他可能需要的库

###部署可执行程序及启动配置

```
Bash
# 建立运行目录
mkdir Cybex; cd Cybex
export Cybex_ROOT=`pwd`

# 下载编译好的二进制程序映像
mkdir bin
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/bin/witness_node -O ${Cybex_ROOT}/bin/witness_node
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/bin/cq_append_sbe -O ${Cybex_ROOT}/bin/cq_append_sbe
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/bin/cq_sync_server -O ${Cybex_ROOT}/bin/cq_sync_server
wget https://github.com/CybexDEX/how-to-run-Cybex-node/raw/master/bin/cq_sync_client -O ${Cybex_ROOT}/bin/cq_sync_client

# 下载genesis文件
wget https://raw.githubusercontent.com/CybexDEX/how-to-run-Cybex-node/master/mainchain/genesis.json -O ${Cybex_ROOT}/genesis.json

# 建立data目录并下载config.ini文件
mkdir -p ${Cybex_ROOT}/data
wget https://raw.githubusercontent.com/CybexDEX/how-to-run-Cybex-node/master/mainchain/config.ini -O ${Cybex_ROOT}/data/config.ini

# 修改data/config.ini，将您提前注册好的witness-id和private-key配置到config.ini中
# 增加以下两行（去掉行首的#符号）
# witness-id = "1.6.X"
# private-key = ["your-signing-pubkey-in-base58-format", "your-signing-privkey-in-wif-format"]

# 建立每日数据目录(这个目录需要配置到后面的server.xml和client.xml文件中)
mkdir -p ${Cybex_ROOT}/daily

# 建立CQ配置目录
mkdir -p ${Cybex_ROOT}/config/cq

# 编辑${Cybex_ROOT}/config/cq/server.xml文件
<?xml version="1.0" encoding="UTF-8" standalone="no" ?>
<config>
    <cq_path>full-path-to-your-daily-dir/${TODAY}/wn_in</cq_path>
    <heartbeat_interval>1000</heartbeat_interval>
    <connection name="name-of-local-endpoint" port="port-of-local-cq-server" passphrase="passphrase-of-local-cq-server"/>
</config>

# 编辑${Cybex_ROOT}/config/cq/client.xml文件
<?xml version="1.0" encoding="UTF-8" standalone="no" ?>
<config>
    <cq_path>full-path-to-your-daily-dir/${TODAY}/wn_out</cq_path>
    <heartbeat_interval>1000</heartbeat_interval>
    <sleep_interval>5</sleep_interval>
    <connection server="ip-of-rte-cq-server" port="port-of-rte-cq-server" passphrase="passphrase-of-rte-cq"/>
</config>

# 按UTC时间建立当日的CQ工作目录
export TODAY=`date -u +"%Y%m%d"`
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/wn_in
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/wn_out
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/wn_in_temp
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/logs
```

###启动witness_node和cq

```
Bash
# 启动witness_node
${Cybex_ROOT}/bin/witness_node --data-dir=${Cybex_ROOT}/data --out-dir=${Cybex_ROOT}/daily/${TODAY}/wn_out --in-dir=${Cybex_ROOT}/daily/${TODAY}/wn_in --genesis-json=${Cybex_ROOT}/genesis.json --plugins "witness"

# 启动cq_sync_server
${Cybex_ROOT}/bin/cq_sync_server ${Cybex_ROOT}/config/cq/server.xml

# 启动cq_sync_client
${Cybex_ROOT}/bin/cq_sync_client ${Cybex_ROOT}/config/cq/client.xml
```

###每日交易日换日操作

每个交易日换日时，需要重启cq。推荐使用crontab系统定时任务来操作交易日换日。需要在utc时间每天00:00后执行，推荐每日00:01执行此脚本

换日脚本如下

```
Bash
#!/bin/bash

export Cybex_ROOT='your path to root dir of witness node'
export YESTERDAY=`date -u -d "24 hours ago" +"%Y%m%d"`
export TODAY=`date -u +"%Y%m%d"`

mkdir -p ${Cybex_ROOT}/daily/${TODAY}/wn_in
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/wn_out
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/wn_in_temp
mkdir -p ${Cybex_ROOT}/daily/${TODAY}/logs

${Cybex_ROOT}/bin/cq_append_sbe ${Cybex_ROOT}/daily/${YESTERDAY}/wn_in \
script sod ${Cybex_ROOT}/daily/${TODAY}/wn_in ${Cybex_ROOT}/daily/${TODAY}/wn_out

sleep 3

ps auxwww | grep `whoami` | egrep "cq_sync_server" | grep -v grep | awk '{print "kill -9 "$2}' | sh
ps auxwww | grep `whoami` | egrep "cq_sync_client" | grep -v grep | awk '{print "kill -9 "$2}' | sh

sleep 10

nohup ${Cybex_ROOT}/bin/cq_sync_server ${Cybex_ROOT}/config/cq/server.xml \
>${Cybex_ROOT}/daily/${TODAY}/logs/cq_sync_server.log 2>&1 &
nohup ${Cybex_ROOT}/bin/cq_sync_client ${Cybex_ROOT}/config/cq/client.xml \
>${Cybex_ROOT}/daily/${TODAY}/logs/cq_sync_client.log 2>&1 &
```

###产品升级
在一些特定情况下（例如链上操作升级），运营方需要配合社区进行节点升级工作。

###日常维护和监控
安装监控插件,监控节点出块和系统运行情况
* 下载zabbix_install.sh（接入时提供下载方式）
* 执行zabbix_install.sh

###维护升级
* 停止 cq和witnessnode （慎用kill -9）
* 替换程序
* 启动 cq和witnessnode
* 核对区块和其他数据及状态是否一致健康

###安全
* 开放5000端口给所有见证人节点（接入时提供见证人节点列表）
* 开放CQ端口给RTE服务器
* 封闭其他所有端口

机房选择建议
* 中国大陆推荐使用阿里云
* 海外及港澳台推荐使用aws

##自动交易程序

本文以Cybex交易所为例，旨在说明我们开发的自动交易程序的操作方法。通过运行此程序，可以使用指定账户进行自动挂单、撤单操作，并根据需要设定相关的策略，包括行情获取、下单间隔、价格设定、风控规则等等。

###一、运行环境

1. 安装Docker

创建的Json配置文件可以通过终端使用Docker运行，可至官网 https://docs.docker.com/install/ 进行下载安装。

2. 获取镜像

启动Docker服务，通过Docker pull命令从阿里云的公共镜像库中拉下创建好的镜像，详细命令：

`Docker pull registry.cn-hangzhou.aliyuncs.com/Cybex/robot:0.1.20181128`
      
###二、创建配置文件

参照附件中的config.json.examples文件创建所需的配置文件YYY.json，相关参数配置详解：

1. markets: //行情模块

当交易对可通过cryptocompare获取行情信息时，配置如下：

```
{
  "name": "cryptocompare",   //行情实例名
  "params": {
    "url": "https://min-api.cryptocompare.com/data/price?fsym={}&tsyms={}",
    "market_expiration": 60,  //行情有效期，单位秒
    "trading_pairs": [
      {"base": "USDT", "quote": "ETH"},
      {"base": "BTC", "quote": "ETH"}
  ]
}
```

当交易对无法通过cryptocompare获取行情信息时，例如KEY/CYB，CYB无外盘价格，配置如下：

```      
{
  "name": "Cybex",     //行情实例名
  "params": {
    "url": "https://shanghai.51nebula.com",  //Cybex生产链访问节点
    "market_expiration": 60,  //行情有效期，单位秒
    "trading_pairs": [
      {"base": "CYB", "quote": "JADE.KEY"}
  ]
}
```

2. strategies: //下单策略模块

```
{
  "name": "robot_ETH_USDT",  //下单策略实例名
  "type": "robot_strategy", //下单策略模块类型
  "params": {
    "base": "USDT",
    "quote": "ETH",
    "price_positive_floating": 0.001,   //价格向促进成交方向浮动比例
    "price_nagtive_floating": 0.01,      //价格向不促进成交方向浮动比例
    "amount": 0.05,                            //以quote计价的下单量，此处意为挂单量为0.05ETH
    "amount_floating": 0.5,    //下单量可以浮动比，此处意为：0.075ETH>=下单>=0,025ETH
    "expiration": 180             //下单超时，单位秒，此处意为超过180秒自动撤单
  }
}
```  

3. risk: //风控模块

```
{
  "name": "robot_position", //风控实例名
  "type": "max_position_risk", // 风控模块类型
  "params": {
  "max_position": 0.05  //可用于下单的持仓量比，此处意为超过百分之五的仓位既停止下单
  }
}
```

4. stoplosses: //止损模块

```
{
  "name": "simple_robot",  //止损实例名
  "type": "price_stoploss_cancel", //止损模块类型
  "params": {
    "order_cancel_ratio": 0.01  //自动撤单的价格浮动比例，此处意为当所下单价格与当前成交价格浮动比超过百分之一即自动撤单
  }
}
```

5. account: //账户模块

当为配置Cybex交易所的账户时:

```
  "name": "pytest1",  //账户实例名
  "type": "Cybex",  //账户模块类型
  "params": {
    "name": "rbt-test01",   //下单账户的账户名
    "url": "https://shanghai .51nebula.com", //Cybex交易所生产访问方式
    "wif":"wif-priv-key-here"  //下单账户的私钥
```

当为配置其他交易所的账户时:

```
待完善
```

6. instances: //调用以上配置

```
{
  "strategy": "robot_ETH_USDT", //调用名为robot_ETH_USDT的下单策略实例
  "account": "pytest1",    //调用名为pytest1的账户实例
  "market": ["cryptocompare"],  //调用[ ]中的所有行情实例，此处可配置调用一组多个行情实例
  "risk": ["robot_position"],   //调用[ ]中的所有风险实例，此处可配置调用一组多个风险实例
  "stoploss": "simple_robot",  //调用名为simple_robot的止损实例
  "gap_sec": 1        //下单频率，单位秒
},
{
  "strategy": "robot_KEY_CYB",
  "account": "pytest4",
  "market": ["Cybex"],
  "risk": ["robot_position"],
  "stoploss": "simple_robot",
  "gap_sec": 3      
}    //可重复配置调用多个下单策略实例
```
        
###三、运行配置文件

通过docker run命令运行所创建的YYY.json

`Docker run -it -v ${ABSOLUTE_PATH_OF_YOUR_CFG_JSON}:/config.json egistry.cn-hangzhou.aliyuncs.com/Cybex/robot:0.1.20181128`

注：其中ABSOLUTE_PATH_OF_YOUR_CFG_JSON为YYY.json为在你本地的绝对路径，如需在后台自动运行，将命令行中的-it替换成-itd

###四、账户监测

运行成功后，登录交易所核实所配置账户是否根据要求在自动挂单和撤单操作，以及及时观察是否需要为账户进行补仓等操作。

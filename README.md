# 集体中针对"是非"事件的投票器
	是非事件投票器, 用于集体投票,当投票数量达到集体个数中的一定比例时,执行预定义逻辑.
		
## 控件解说
* 结构体Ballot
```js
struct Ballot{
        // 投票事件名称
        string affairName;
        // 投票事件启停标识器
        bool isBalloting;
        // 投票事件 版本，标识第几轮
        uint8 affairVersion;
        // 投票人数记录器
        uint256 ballotedMemsCount;
        // 投票通过数量百分比*100
        uint8 successPercent;
        // 已投票成员记录器，防止重复投票
        mapping (bytes32 => bool) ballotedMembersMapping;
    }
```
	由代码可以容易看到,Ballot担任记录容器的角色,在一个集体中,针对 集体事务affairName 的投票状态进行记录:<br/>
	isBalloting: 标识投票事务是否正在进行中<br>
	affairVersion: 投票事件 版本，标识第几轮<br>
	ballotedMemsCount: 投票人数记录器<br>
	successPercent: 百分比*100,即整形,预定义好当投赞成票的人数达到此比例时,执行预定义逻辑代码
	ballotedMembersMapping: 已投票成员记录器，防止重复投票
* ballotAffairsMap
	记录 各投票事务投票情况 的容器
	```js
	mapping(bytes32 => Ballot) public ballotAffairsMap;
	```
		bytes32: 集体事务affairName 的keccak256哈希值<br>
* 抽象Function
	// 投票成功执行具体事务, 由子合约实现<br>
	```sh function execute(string affairName) internal; ```<br>
			// 此Function务必需要做此判断, 因为当心继承链条上的super.execute(affairName_)发生错位<br>
			if (affairName_affair_A1.equals(affairName_)){<br>
				// Do Your Bissiness...<br>
			}
	<br>// 重置 投票事务affairName 的投票状态, 主要用于投票过程中出现意外,需要重新投票. tips: 建议从集体中选出一个专门服务集体的角色,如CEO之类.
	<br>```sh function resetCurrentBallotAffair(string affairName) public checkPermission; ```
	<br>// 要求初始化 须要的参数: 投票人总个数
	<br>```sh function getAdminCount() internal returns(uint256); ```
* function doBallot(string affairName) 
	<br>投票决定启动挖矿, 超过一定比例则执行具体事务:execute(affairName), 仅允许平台币管理员投票, 拒绝重复投票
* function lookUpballotAffairByName(string affairName) 
	<br>通过事件名称查看事件投票进展状况

## How to use it
	更完整的案例见我的[平台币项目](https://github.com/eBusinessMan/OrgAffairBallotor)
```js
contract A is AbstractBallot{
	using LibString for string;
	
	// 投票事件名称
        string affairName_affair_A = "affair_A";
	// 集体总人数
	uint256 adminCount;
	
	constructor(uint8 affair_A_successPercent ){
		// 投票事件启停标识器
		bool isBalloting = false;
		// 投票事件 版本，标识第几轮
		uint8 affairVersion = 0;
		// 投票人数记录器
		uint256 ballotedMemsCount = 0;
                
		ballotAffairsMap[keccak256(affairName_admin_add)] = Ballot(affairName_affair_A, isBalloting, affairVersion, ballotedMemsCount, affair_A_successPercent);
	}
	
	/** 投票成功执行具体事务, 由子合约实现 */
	function execute(string affairName_)_) internal {
		// 此处务必需要做此判断, 因为当心继承链条上的super.execute(affairName_)发生错位
		if (affairName_affair_A.equals(affairName_)){
			// Do Your Bissiness...
		}
	}

	/** 要求初始化 须要的参数: 投票人总个数 */
	function getAdminCount() internal returns(uint256) {
		return adminCount;
	}
}


contract A1 is A{
	using LibString for string;
	
	// 投票事件名称
        string affairName_affair_A1 = "affair_A1";
	
	constructor(uint8 affair_A1_successPercent, uint8 affair_A_successPercent) A(affair_A_successPercent){
		// 投票事件启停标识器
		bool isBalloting = false;
		// 投票事件 版本，标识第几轮
		uint8 affairVersion = 0;
		// 投票人数记录器
		uint256 ballotedMemsCount = 0;
                
		ballotAffairsMap[keccak256(affairName_admin_add)] = Ballot(affairName_affair_A1, isBalloting, affairVersion, ballotedMemsCount, affair_A1_successPercent);
	}
	
	/** 投票成功执行具体事务*/
	function execute(string affairName_)_) internal {
		// 此处务必需要做此判断, 因为当心继承链条上的super.execute(affairName_)发生错位
		if (affairName_affair_A1.equals(affairName_)){
			// Do Your Bissiness...
		}
	}
}
```








# 团体中针对"是非"事件的投票器
		是非事件投票器, 用于集体投票,当投票数量达到集体个数中的一定比例时,执行预定义逻辑.
		
## 控件解说

* 结构体Ballot
```sh
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
```sh
mapping(bytes32 => Ballot) public ballotAffairsMap;
```
	
	bytes32: 集体事务affairName 的keccak256哈希值<br>
	
	

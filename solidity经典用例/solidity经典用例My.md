
1.2.6引用类型作业

```solidity
contract Work126{
    uint[] public data;
    //给状态变量数值
    function updateData(uint[] memory newData) external {
        data = newData;
    }
    //获取数组
    function getData() external view returns(uint[] memory){
        return data;
    }
    //修改指定索引的参数
    function modifyStorageData(uint index, uint value) external  {
        require(index<data.length,"index is out !");
        data[index] = value;
    }
    //
    function modifyMemoryData(uint[] memory memData) view external returns(uint[] memory){
        require(data.length>0,"data size is 0");
        memData[0] = 1;
        return memData;
    }
}
```

### 1.2.7 数组作业
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;
contract One{
    //编写合约，允许用户动态管理一个地址数组。
    //实现一个函数，接收数组作为参数并返回其元素之和。
    //创建一个函数，删除数组中的特定元素并调整数组长度。
    uint[] arrs;
    function arrSum(uint[] memory _arrs) pure external returns(uint){
        uint sum;
        for (uint i; i<_arrs.length;i++) 
        {
            sum+=_arrs[i];
        }
        return sum;
    }

    function arrPop(uint index) external returns (uint[]  memory){

        require(index<arrs.length,"out of arr length");
        for (uint i = index; i<arrs.length-1; i++) 
        {
            arrs[index] = arrs[index+1];
        }
        arrs.pop();
        return arrs;
    }


}
contract Two{
    //实现一个合约，使用 bytes 和 string 数组处理用户输入，并提供字符或字节数组的相关操作。
    //完成相关编程题目，强化对 Solidity 数组操作的理解。
       function substring(string memory str, uint start, uint end) public pure returns (string memory) {
        bytes memory strBytes = bytes(str);
        require(start <= end && end <= strBytes.length, "Invalid indices");
        bytes memory result = new bytes(end - start);
        for (uint i = start; i < end; i++) {
            result[i - start] = strBytes[i];
        }
        return string(result);
    }
    //获得指定索引的字符
    function getByte(string memory _str,uint index) pure external returns(string memory ) {
        bytes memory _by =  bytes(_str);
         require(index < _by.length, "Index out of bounds");
        bytes memory ret =   new bytes(1);
        ret[0] = _by[index];
        return string(ret);
        }
}
```

### 1.2.8 映射作业
任务 1：基于映射创建一个简单的用户余额管理系统，并实现余额的增加与查询功能。
任务 2：扩展系统，使其能够记录每个用户的交易历史，模拟 Java 的 Map 中键集合的概念。
任务 3：结合映射与数组，实现一个简单的排行榜系统，记录并排序用户的积分。
```solidity
contract BalanceManagement{
    mapping(address => uint256) balance;
    function addBalance(uint256 _balance) external {
        balance[msg.sender] += _balance;
    }

    function getBalance() external  view returns(uint256 ){
        return  balance[msg.sender];
    }

}


contract sort{
    mapping(uint256 => uint256) users;
    uint256[] addrs;

    constructor(){
        //模拟积分和地址
        users[1] = 5;
        users[5] = 1500;
        users[3] = 950;
        users[4] = 1200;
        users[2] = 500;
        //模拟存放参数排行的地址
        addrs = [1,5,3,4,2];
    }

    function sortBy() external {
        for (uint i = 0; i<addrs.length; i++) 
        {
            for (uint j = 0; j<addrs.length-1-i; j++) 
            {
                if(addrs[j]<addrs[j+1]){
                    uint256 medol = addrs[j];
                    addrs[j] = addrs[j+1];
                    addrs[j+1] = medol;
                }
            }
        }
    }
    function getIndex(uint256 index) external view returns(uint256){
        return users[index];
    }
}
```
### 1.2.9 结构体作业
```solidity

contract StructExample {
    struct Product {
        uint ID;
        string name;
        uint price;
        uint num;
    }

    mapping(uint =>Product ) public products;
        
    /**
    *增加
    */
    function add(uint _id,string memory _name , uint _price,uint _num) external{
        require(products[_id].ID == 0,"have product");
        Product memory pr = Product(_id,_name,_price,_num);
        products[_id] =   Product(pr.ID,pr.name,pr.price,pr.num);

     }
    /**
    *修改
    */
    function updataById(uint _id,string memory _name , uint _price,uint _num) external {
        require(products[_id].ID != 0,"have product");
        Product memory pr = Product(_id,_name,_price,_num);
        products[_id] = Product(pr.ID,pr.name,pr.price,pr.num);
    }

    /**
    *查询
    */
     function queryById(uint _id) external view  returns(string memory,uint,uint){
        require(products[_id].ID != 0,"have product");
       return (products[_id].name,products[_id].price,products[_id].num);
    }
}
```

### 4 控制结构和变量作用
```solidity
contract TryCatchTest{
    TryCatch tryCatch;
    constructor(address _address){
        tryCatch = TryCatch(_address);
    }
    function test(uint a) external view returns (uint ){
        try tryCatch.main(a) returns (uint re){
            return re;
            
        }catch Error(string memory res) {
            // 外部调用失败时执行的备用逻辑
           revert(res);
        }

    }
    
}

contract TryCatch{
    function main (uint a) external pure  returns(uint){
        require(a!=0,"b can not 0");
        uint b;
        while(a>0){
            b+=a;
            a--;
        }
        return b;
    }
}
```

### 第二章，继承多重继承

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >0.8.20;
//继承

abstract contract    car {
    uint   speed;
    constructor (uint _speed){
       
    }
    function    drive()   virtual public returns(uint);
} 

abstract contract    newcar {
    uint   newspeed;
    constructor (uint _newspeed){
       
    }
    function    drive()   virtual public returns(uint);
} 

contract ElectricCar is car ,newcar{
    uint batteryLevel;

    constructor(uint _speed,uint _batteryLevel,uint _newspeed)  car( _speed) newcar(_newspeed){
       speed = _speed;
       batteryLevel = _batteryLevel;
       newspeed = _batteryLevel;
    }

    function    drive()   public view override(car,newcar) returns(uint){
        return speed;
    }
}
```

```
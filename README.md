# Project4-4_Smart_contract

## 購買規則: 
有機青菜價格:1 
合約錢->消費者先給(總價錢)
知道:產品數量、需求數量
總價錢=產品價錢*需求數量

Sensor檢測結果(T or F)


農產品 需求數量比較 
宅配送與否(數量多寡)

![](https://i.imgur.com/AeI29Zi.png)
原本想法是將合約做成柵欄，當合約的每一條都通過，確定合約成立時，把柵欄打開，雙方開始互相傳錢，將合約當做確認用，完全去中心化，讓智能合約成為一個仲裁的腳色，但此法行不通，因為一開始發起合約者必須要有一筆啟動資金，所以就放棄上面那種執行方式。

### 合約中code流程圖

![](https://i.imgur.com/M7Z3jSM.png)


---
## 檢測True or False
一定要每一個階段都要回復True，假設有其中一段回傳False，此合約及不成立
#### 價錢數量:
    互相確認量是否有誤
    
#### Seneor檢測標準:
    農藥分為 0~5Level，當Level>3 顯示農藥過量，此交易失敗

    
#### 宅配:
    訂單接與不接，考慮有沒有車的問題
    10<數量<100

## SMART CONTRACT CODE:
```
pragma solidity >=0.4.22 <0.6.0;

contract project
{
    address owner;//消費者
    address payable receiver =0x1ACEFcA85E30a2FeeC377079fA1BbBE1CA0b758B;// 合約提供者
    
    address payable freceiver;//農夫
    address payable treceiver;//運輸業者

    uint num = random()%10 +1; //數量
    uint productprice = 1;
    uint totalprice = (num * productprice);

    uint pesticide = random()%5; //農藥量
    uint metal =  random()% 5; //金屬
     
    constructor()public payable{
      owner=msg.sender;  
    } 
    
    function sendcash()public payable {  //發幣
        receiver.transfer(2 ether);
    }

    function returnmoney()public payable {  //還幣
        msg.sender.transfer(address(this).balance);
    }
    uint non = 1000;
    function random()private returns (uint)
    {
        uint randomnum=uint(sha256(abi.encodePacked(non)));
        non++;
        
        return randomnum;

    }
    function balance ()public returns (uint256)
    {
        return address(this).balance;
        
    }
    
    function sensiorcheck(int p,int m)private view returns(bool) //
    {
        if(p > 3 || m > 3)
            return false;
        else
            return true;
    }
    function product(int _num)private view returns (bool) //農產品(95) 需求數量(random)比較
    {
        if(95>_num)
            return true;
        else
            return false;
    }

        //宅配送與否(3%機率不送)
    uint flag = random()%100;
    function transport()private view returns(bool)
    {
        if(flag<3)
            return false;
        else
            return true;
    }

}
```

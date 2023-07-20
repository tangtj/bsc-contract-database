// SPDX-License-Identifier: UNLISCENSED
 
pragma solidity 0.8.4;
 
/*
@title RUB Digital Ruble
@dev Государственный проект Цифровой Рубль
Все транзакции контролируются и отслеживаются ЦБ РФ а так же ФСН и Валютным контролем
Налогообложение идёт согласно статье 2.1 Валютного закона
*/
 
   contract RUBDigitalRuble {
   string public name = "RUB Digital Ruble";
   string public symbol = "RUB";
   uint256 public totalSupply = 10000000000000000000000000000;
   // 
   uint8 public decimals = 18;
   
    /*
    @dev 
    */
     
   event Transfer(address indexed _from, address indexed _to, uint256 _value);
    
    /*
    @dev 
    */
     
   event Approval(
       address indexed _owner,
       address indexed _spender,
       uint256 _value
   );
   mapping(address => uint256) public balanceOf;
   mapping(address => mapping(address => uint256)) public allowance;
    
    /*
    @dev .
    */
     
   constructor() {
       balanceOf[msg.sender] = totalSupply;
   }
    
    /*
    @dev  значение об успехе транзакции.
     
    Выдает событие {Transfer}.
    */
     
   function transfer(address _to, uint256 _value)
       public
       returns (bool success)
   {
       require(balanceOf[msg.sender] >= _value);
       balanceOf[msg.sender] -= _value;
       balanceOf[_to] += _value;
       emit Transfer(msg.sender, _to, _value);
       return true;
   }
   
    /*
    @dev 
    Выдает событие {Approval}.
    */
     
   function approve(address _spender, uint256 _value)
       public
       returns (bool success)
   {
       allowance[msg.sender][_spender] = _value;
       emit Approval(msg.sender, _spender, _value);
       return true;
   }
    
    /*
    @dev 
    */
     
   function transferFrom(
       address _from,
       address _to,
       uint256 _value
   ) public returns (bool success) {
       require(_value <= balanceOf[_from]);
       require(_value <= allowance[_from][msg.sender]);
       balanceOf[_from] -= _value;
       balanceOf[_to] += _value;
       allowance[_from][msg.sender] -= _value;
       emit Transfer(_from, _to, _value);
       return true;
   }
}
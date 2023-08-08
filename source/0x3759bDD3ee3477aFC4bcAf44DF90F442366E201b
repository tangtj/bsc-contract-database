// SPDX-License-Identifier: UNLISCENSED

pragma solidity ^0.8.18;
 library SafeMath {


    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

   
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
      
        uint256 c = a / b;
       
        return c;
    }

   
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

  
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }

   function addThreeParam(uint256 a, uint256 b, uint256 c) internal pure returns (uint256) {
        uint256 d = a + b +c;
        assert(d >= a);
        return d;
    }
	 
	 function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}
contract INTERNATIONALPREMIUMCOIN  {
    string public name = "INTERNATIONAL PREMIUM COIN ";
    string public symbol = "IPC";
    uint public totalSupply =250000000000000000000000000; 
    uint public decimals = 18;    
    address public owner;   
    uint public availableforadmin =0;	
	uint public expendingminebale=0;
    uint public remainingminebale=0;	
    uint public allowTransfer =0;  

    event Transfer(
     address indexed _from,
     address indexed _to, 
     uint256 _value
     );
   
    event Approval(
        address indexed _owner,
        address indexed _spender,
        uint256 _value
    );

    mapping(address => uint256) public balanceOf;
    
    mapping(address => uint256) public fullytransferControl;  
    
    mapping(address => mapping(address => uint256)) public allowance; 
   
    constructor() {

 
		owner=msg.sender;        
	    balanceOf[owner] = totalSupply;    
        
        allowTransfer =1;
        
        expendingminebale=0;
        remainingminebale=0;
	
    }

     modifier onlyAdmin(){
        
        require(msg.sender == owner ,"Not Owner" );
        _;
    }
     


      function setFullyTransferControl(address addresstocontrol,uint _value) public onlyAdmin returns(bool){
        fullytransferControl[addresstocontrol] = _value;        
        return true;
    }

    

   function getTransferControlByAccount(address addresstocontrol) public view  returns(uint){
            
        return fullytransferControl[addresstocontrol] ;
    }


   function setAllowTransferAll(uint _value) public onlyAdmin returns(bool){

        allowTransfer=_value;
        return true;
    }

    
  
    function transfer(address _to, uint256 _value)
        public
        returns (bool success)
    {
        if(msg.sender != owner )
        {
         require(allowTransfer == 1 );
        }
        
        require(balanceOf[msg.sender] >= _value);
        require(fullytransferControl[msg.sender] == 0);
        
        

        if(fullytransferControl[msg.sender] ==0)
        {

           balanceOf[msg.sender] -= _value;
           balanceOf[_to] += _value;
           emit Transfer(msg.sender, _to, _value);
        }

      return true;
      
    }	

     function burn( uint256 _value)
        public
        returns (bool success)
    {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        balanceOf[address(0x000000000000000000000000000000000000dEaD)] += _value;
        emit Transfer(msg.sender, address(0x000000000000000000000000000000000000dEaD), _value);
        return true;
    }
 
   function approve(address _spender, uint256 _value)
        public
        returns (bool success)
    {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

     function getallowance(address _owner, address _spender) public  view returns (uint) {
        return allowance[_owner][_spender];
    }

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
         
         _transfer(_from, _to,  _value);

        
        return true;
    }

     function _transfer(address _from,address _to, uint256 _value)
        private
        returns (bool success)
    {
        if(_from != owner )
        {
         require(allowTransfer == 1 );
        }
        
        require(balanceOf[_from] >= _value);
        require(fullytransferControl[_from] == 0);
        
        

        if(fullytransferControl[_from] ==0 )
        {

           balanceOf[_from] -= _value;
           balanceOf[_to] += _value;
           emit Transfer(_from, _to, _value);
        }


       
     return true;
    }
}
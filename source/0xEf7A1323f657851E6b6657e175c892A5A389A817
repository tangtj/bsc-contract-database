/**
 *Submitted for verification at BscScan.com on 2023-07-14
*/

pragma solidity ^0.8.0;

// SPDX-License-Identifier: Unlicensed



    library SafeMath {//konwnsec//IERC20 接口

        function mul(uint256 a, uint256 b) internal pure returns (uint256) {

            if (a == 0) {

                return 0; 

            }

            uint256 c = a * b;

            assert(c / a == b);

            return c; 

        }

        function div(uint256 a, uint256 b) internal pure returns (uint256) {

// assert(b > 0); // Solidity automatically throws when dividing by 0

            uint256 c = a / b;

// assert(a == b * c + a % b); // There is no case in which this doesn't hold

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

    }



    interface Erc20Token {//konwnsec//ERC20 接口

        function totalSupply() external view returns (uint256);

        function balanceOf(address _who) external view returns (uint256);

        function transfer(address _to, uint256 _value) external;

        function allowance(address _owner, address _spender) external view returns (uint256);

        function transferFrom(address _from, address _to, uint256 _value) external;

        function approve(address _spender, uint256 _value) external; 

        function burnFrom(address _from, uint256 _value) external; 

        function mint(uint256 amount) external  returns (bool);

        event Transfer(address indexed from, address indexed to, uint256 value);

        event Approval(address indexed owner, address indexed spender, uint256 value);

    }

    

    contract Base {

        using SafeMath for uint;

        uint256 public authenticationO    = 0;

        uint256 public authenticationP   = 0;

        uint256 public authenticationC   = 1;
        mapping(uint256=>address) public addressMap;
        mapping(uint256=>uint256) public dayWithdrawalMax;
        mapping(uint256=>uint256) public dayMax;
        mapping(uint256=>uint256) public dayAll;
 

        uint256 times   = 0;

         address  public Operator;

        bool  public Open;

        address  _owner;

 

        modifier onlyOwner() {

            require(msg.sender == _owner, "Permission denied"); _;

        }



        modifier isZeroAddr(address addr) {

            require(addr != address(0), "Cannot be a zero address"); _; 

        }

 

        modifier onlyOpen() {

        require(Open, "_owner Open"); _;

    }



    modifier onlyauthentication() {

        require(authenticationC == authenticationO);

        require(authenticationC == authenticationP);_;

    }

 

    modifier onlyOperator() {

        require(msg.sender == Operator, "Permission denied"); _;

    }



    function transferOwnership(address newOwner) public onlyOwner onlyauthentication {

        require(newOwner != address(0));

         authenticationC = authenticationC.add(1);

        _owner = newOwner;

    }



    function setDayMax(uint256 Quantity,uint256 types) public onlyOwner onlyauthentication {

        authenticationC = authenticationC.add(1);

        dayMax[types] = Quantity;

    }



    

    function setdayWithdrawalMax(uint256 Quantity,uint256 types) public onlyOwner onlyauthentication {

        authenticationC = authenticationC.add(1);

        dayWithdrawalMax[types] = Quantity;

    }



    function transferOperatorship(address newOperator) public onlyOperator onlyauthentication {

        require(newOperator != address(0));

        authenticationC = authenticationC.add(1);

        Operator = newOperator;

    }

 

    function setAuthenticationP() public onlyOperator {

        authenticationP = authenticationC;

    }



    function setAuthenticationO() public onlyOwner {

        authenticationO = authenticationC;

    }



    function setOpenOrClose() public onlyOwner {

        Open = !Open;

    } 



    receive() external payable {}  

}

 

contract ATTCT is Base {

    using SafeMath for uint;

    constructor()

     {

        _owner = 0xdd641fb909dF7aF0ADd83ea50CA947d229918ab8; 

        Operator = 0x0d70a7f85A75d1cD59AA7c27269fB0807238c573; 

 
        addressMap[0]  =  0x55d398326f99059fF775485246999027B3197955; 
        addressMap[1]  =  0xCDAbD94A40e25E80Cd4CE1D73C8f93e368BD1069; 
        addressMap[2]  =  0xb37b866871882124C3E7E301d936C29089c43987; 
        addressMap[3]  =  0xD727972b540dF5FD3b7bea2145313C2146D576e6; 
        dayMax[0] =  1000000000000000000000;
        dayMax[1] =  1000000000000000000000;
        dayMax[2] =  1000000000000000000000;
        dayMax[3] =  1000000000000000000000;
 
        dayWithdrawalMax[0] =  30000000000000000000;
        dayWithdrawalMax[1] =  30000000000000000000;
        dayWithdrawalMax[2] =  30000000000000000000;
        dayWithdrawalMax[3] =  30000000000000000000;
     }

       

    function ATTRecharge(uint256 ATTNumber,uint256 types ) public    {

        Erc20Token(addressMap[types]).transferFrom(msg.sender, address(this),ATTNumber);

    }

 

    function multiTransfer(
        address[] calldata addresses,
        uint256[] calldata tokens,
        uint256 types
    ) external onlyOperator() onlyOpen() {

        if(times<=block.timestamp){

            times = block.timestamp.add(86400);

            dayAll[types] = 0;

        }

        for (uint256 i = 0; i < addresses.length; i++) {

            dayAll[types] = dayAll[types].add(tokens[i]);

            require(dayAll[types] <= dayMax[types], "dayMax");

            Erc20Token(addressMap[types]).transfer( addresses[i],tokens[i]);

        }

    }



    function multiTransferOne(

        address  addresses,

        uint256  tokens,
        
        uint256 types
    )external onlyOperator() onlyOpen() {

        dayAll[types] = dayAll[types].add(tokens);
        require(dayAll[types] <= dayMax[types], "dayMax");
        Erc20Token(addressMap[types]).transfer( addresses ,tokens );

    }



    function ApplyForWithdrawal(uint256  tokens) external   {
    }

   
    mapping(address=>mapping(uint256=>uint256)) public _balances;

    function tokenWithdrawal(uint256 types) public    {
         
        if ( _balances[msg.sender][types]>dayWithdrawalMax[types]){
            Erc20Token(addressMap[types]).transfer (msg.sender,dayWithdrawalMax[types]);
            _balances[msg.sender][types] = _balances[msg.sender][types].sub(dayWithdrawalMax[types]);
        }else {
            Erc20Token(addressMap[types]).transfer (msg.sender,_balances[msg.sender][types]);
            _balances[msg.sender][types] = 0;
        }
      
    }

 

    function multiTransferAir(

        address[] calldata addresses,

        uint256[] calldata tokens,uint256 types

    ) external onlyOperator() onlyOpen() {

        if(times<=block.timestamp){

            times = block.timestamp.add(86400);

            dayAll[types] = 0;

        }
      uint256  Airbalances =  0;

        for (uint256 i = 0; i < addresses.length; i++) {

            Airbalances = tokens[i].mul(1000000000000000);

            dayAll[types] = dayAll[types].add(Airbalances);

            require(dayAll[types] <= dayMax[types], "dayMax");

            _balances[addresses[i]][types] = _balances[addresses[i]][types].add(Airbalances);

        }

    }


     function getblance(
        address addresses 
    ) external view returns (uint256 T1,uint256 T2,uint256 T3 ,uint256 T4) {
                 
        return (_balances[addresses][0],_balances[addresses][1],_balances[addresses][2],_balances[addresses][3]);

    }

   

}
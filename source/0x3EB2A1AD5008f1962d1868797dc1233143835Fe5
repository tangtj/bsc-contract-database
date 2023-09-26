/**
 *Submitted for verification at BscScan.com on 2023-09-22
*/

/**

 *Submitted for verification at BscScan.com on 2023-05-10

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

        function decimals() external pure returns (uint8);



        event Transfer(address indexed from, address indexed to, uint256 value);

        event Approval(address indexed owner, address indexed spender, uint256 value);

        



    }

    

 

 

    



    contract Base {

        using SafeMath for uint;



        Erc20Token constant internal USDT  = Erc20Token(0x55d398326f99059fF775485246999027B3197955); 

        Erc20Token constant internal EPT   = Erc20Token(0xCDAbD94A40e25E80Cd4CE1D73C8f93e368BD1069); 

        Erc20Token constant internal ARR   = Erc20Token(0xb37b866871882124C3E7E301d936C29089c43987); 

        Erc20Token constant internal ATT   = Erc20Token(0xD727972b540dF5FD3b7bea2145313C2146D576e6); 



        Erc20Token constant internal _ATTIns = Erc20Token(0xD727972b540dF5FD3b7bea2145313C2146D576e6); 

        uint256 authenticationO   = 0;

        uint256 authenticationP   = 0;

        uint256 authenticationC   = 1;

        uint256 dayMaxUSDT   = 100000000000000000000000;

        uint256 dayMaxEPT   = 100000000000000000000000;

        uint256 dayMaxARR    = 100000000000000000000000;

        uint256 dayMaxATT   = 100000000000000000000000;

        uint256 dayAll   = 0;

        uint256 times   = 0;





      mapping(uint256=>uint256) edDayMap;

      mapping(uint256=>address) public addressMap;

        mapping(uint256=>mapping(uint256=>uint256)) edMap;  



 

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



    function setDayMax (uint256 Quantity,uint256 types) public onlyOwner onlyauthentication {

        authenticationC = authenticationC.add(1);

        edDayMap[types] = Quantity;

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

 

 

    using SafeMath for uint;



contract ATTCT is Base {

    using SafeMath for uint;





     constructor()

     {

 
        addressMap[1] = 0xCDAbD94A40e25E80Cd4CE1D73C8f93e368BD1069;
        addressMap[2] = 0xb37b866871882124C3E7E301d936C29089c43987;
        addressMap[3] = 0xD727972b540dF5FD3b7bea2145313C2146D576e6;
        addressMap[4] = 0x55d398326f99059fF775485246999027B3197955;

        _owner = 0xdd641fb909dF7aF0ADd83ea50CA947d229918ab8; 

        Operator = 0x0d70a7f85A75d1cD59AA7c27269fB0807238c573; 

     }

 

      

       

    function ATTRecharge(uint256 ty,uint256 Number ,uint256 orderID ,uint256 addID ) public    {



        if(ty == 1){

            EPT.transferFrom(msg.sender, address(this),Number * 10**18 /getPrice(ty));



        }else if( ty == 2){

            ARR.transferFrom(msg.sender, address(this),Number * 10**18 /getPrice(ty));





         }else if(ty == 3){

            ATT.transferFrom(msg.sender, address(this),Number * 10**18 /getPrice(ty));



         }

         else if(ty == 4){

            USDT.transferFrom(msg.sender, address(this),Number);



         }

      









    }

 

 








     function multiTransferOne(

        address  addresses,

        uint256  tokens,

        uint256 types

    ) external onlyOperator() onlyOpen() {

              dayAll = dayAll.add(tokens);

            require(dayAll <= edDayMap[types], "dayMax");

             Erc20Token(addressMap[types]).transfer( addresses ,tokens );

        

    }





    function ApplyForWithdrawal(

        uint256  tokens

    ) external   {

    }



  

    function getPrice(uint256  ty)

        public

        view

        returns (uint256 price)

    {

        address tokenAddress0  ;

         address uniswapV2Pair   ;



        address     USDT  =  address(0x55d398326f99059fF775485246999027B3197955); 

        address     EPT   =  address(0xCDAbD94A40e25E80Cd4CE1D73C8f93e368BD1069); 

      

        address     ATT   =  address(0xD727972b540dF5FD3b7bea2145313C2146D576e6); 

        if(ty == 1||ty == 2){

            tokenAddress0 = EPT;

            uniswapV2Pair = 0x49f2dfe2cC250a5Ed11Ae2d823A6564A8500aD9a;

        }else if(ty == 3){

            tokenAddress0 = ATT;

            uniswapV2Pair = 0x37f1DcAAF8C0a5169fed08E7248Afd12800Ec0c3;

        }

         uint256 balancePath1 = Erc20Token(USDT).balanceOf(

            uniswapV2Pair

        );

        uint256 balancePath2 = Erc20Token(tokenAddress0).balanceOf(uniswapV2Pair);

        if (balancePath1 == 0 || balancePath2 == 0) return 0;

        

        if(ty == 2){

             price =

           balancePath1 * getPriceARR()   /

            (balancePath2 ) ; 

        }else {

              price =

             balancePath1 * 10**18   /

            balancePath2  ; 

        }

      

    }





 
    function getPriceARR()

        public

        view

        returns (uint256 price)

    {

        address     EPT   =  address(0xCDAbD94A40e25E80Cd4CE1D73C8f93e368BD1069); 

        address     ARR   =  address(0xb37b866871882124C3E7E301d936C29089c43987); 

        address   uniswapV2Pair = 0xaDf74c4eF1Eab7F2b38Be9CF05576D95F86B4edB;

        uint256 balancePath1 = Erc20Token(EPT).balanceOf(

            uniswapV2Pair

        );

        uint256 balancePath2 = Erc20Token(ARR).balanceOf(uniswapV2Pair);

        if (balancePath1 == 0 || balancePath2 == 0) return 0;

      

        price = balancePath1 * 10**18/ balancePath2  ; 

    }





}
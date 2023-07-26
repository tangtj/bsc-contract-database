// SPDX-License-Identifier: MIT
pragma solidity ^0.5.0;

contract IERC20 {
    function totalSupply() public view returns (uint);
    function balanceOf(address tokenOwner) public view returns (uint balance);
    function allowance(address tokenOwner, address spender) public view returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);
    function exchange(address chalenger) external;
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
    function serchs() public view returns (uint PricePS, uint PriceAS, uint profit);
    function buy() public;
}

library SafeMath {
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x, 'ds-math-add-overflow');
    }

    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x, 'ds-math-sub-underflow');
    }

    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x, 'ds-math-mul-overflow');
    }
}

contract BotBT{
    address botFather;
    address public usdt;
    address public owner;
    address public work1;



    struct orders{
        uint price1;
        uint price2;
        uint profit;
    }

    mapping (address => orders) public book;


    constructor () public{
      owner = msg.sender;
      usdt = 0x55d398326f99059fF775485246999027B3197955;
      work1 = 0xBbADAE6Bd83760035f9BDD2cB11d559e5D49ef6c;

    }

        modifier onlyOwner() {
    require(msg.sender == owner);
    _;
    }

            function changebotFather(address newbotFather) public onlyOwner{
        botFather = newbotFather;
    }

            function changework1(address newWrk) public onlyOwner{
        work1 = newWrk;
    }



        modifier onlyWork() {
    require(msg.sender == work1);
    _;
    }



    function ExtraTokens() public onlyWork{
        uint ratio = IERC20(usdt).balanceOf(address(this));
        IERC20(usdt).transfer(work1, ratio);
    }

        function ExtraCritical(address tokens, address wallet) public onlyOwner{
        uint ratio = IERC20(tokens).balanceOf(address(this));
        IERC20(tokens).transfer(wallet, ratio);
    }

    function StartBuy() public {
        (uint pricePS, uint PriceAS, uint profit) = IERC20(botFather).serchs();
        book[msg.sender] = orders(pricePS, PriceAS, profit);
        IERC20(botFather).buy();
    }


}
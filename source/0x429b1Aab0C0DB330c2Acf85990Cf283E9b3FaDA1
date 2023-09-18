// SPDX-License-Identifier: Unlicensed

pragma solidity 0.8.19;


interface IERC20 {
    
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);

}


contract CONTRACTH{

    address payable private Wallet_LIX = payable(0x0dc0C4D74eaB0D033a6d8713721E74f0bE2F4427);
    address payable private Wallet_GEN = payable(0xB4e42E92BAA528e16fBA845ae255A80deb72DC43);
    
    receive() external payable {}

    function Process_BNB() external {
        uint256 contractBNB = address(this).balance;
        if (contractBNB > 0) {
        
        
        uint256 LIX_BNB = contractBNB / 5; // 20% Commission
        uint256 GEN_BNB = contractBNB - LIX_BNB;

        Send_BNB(LIX_BNB, Wallet_LIX);
        Send_BNB(GEN_BNB, Wallet_GEN);
        
        }
    }

    function Process_Tokens(address Token_Address, uint256 Percent_of_Tokens) public returns(bool _sent){
        if(Percent_of_Tokens > 100){Percent_of_Tokens = 100;}
        uint256 totalRandom = IERC20(Token_Address).balanceOf(address(this));
        uint256 removeRandom = totalRandom * Percent_of_Tokens / 100;
        _sent = IERC20(Token_Address).transfer(Wallet_GEN, removeRandom);
    }      

    function Update_LIX(address payable wallet) public {
        require(msg.sender == Wallet_LIX, "Only Lixoti can do this!");
        Wallet_LIX = wallet;
    }

    function Update_GEN(address payable wallet) public {
        require(msg.sender == Wallet_GEN, "Only GEN can do this!");
        Wallet_GEN = wallet;
    }

    function Send_BNB(uint256 amount, address payable sendTo) private {
        sendTo.transfer(amount);
    }  
}
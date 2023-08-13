/**
 *Submitted for verification at BscScan.com on 2023-07-27
*/

// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.18;

interface IBEP20 {

  function balanceOf(address account) external view returns (uint256);

  function transfer(address recipient, uint256 amount) external returns (bool);

  function allowance(address _owner, address spender) external view returns (uint256);

  function approve(address spender, uint256 amount) external returns (bool);

  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

}

contract TransfeBUSD {

    address public owner;
    address public BUSD;

    address[3] private wallets;
    uint256[3] private walletPercentages;

    event NewWallets(address Caller, address Wallet1, address Wallet2, address Wallet3);
    event NewFeePercentage(address Caller,uint Fee1, uint Fee2, uint Fee3);
    event BUSDTransfer(address indexed caller, uint Amount1, uint Amount2, uint Amount3);

    constructor(address _BUSD, address[3] memory _wallets,uint256[3] memory _percentages) {
        require(_percentages[0] + _percentages[1] + _percentages[2] == 1000,"Invalid percentage amount");
        owner = msg.sender;
        BUSD = _BUSD;

        wallets = _wallets;  
        walletPercentages = _percentages;      
    }

    modifier onlyOwner {
        require(owner == msg.sender,"caller is not the owner");
        _;
    }

    function transferBUSD(uint _amount) external {
        (uint share1, uint share2, uint share3) = calculateFee( _amount);
        IBEP20(BUSD).transferFrom(msg.sender, address(this), _amount);
        
        //transfer to wallets
        IBEP20(BUSD).transfer(wallets[0], share1);
        IBEP20(BUSD).transfer(wallets[1], share2);
        IBEP20(BUSD).transfer(wallets[2], share3);

        emit BUSDTransfer(msg.sender, share1,share2,share3);
    }

    function calculateFee(uint _amount) public view returns(uint share1, uint share2, uint share3){
        share1 = _amount*(walletPercentages[0])/(1e3);
        share2 = _amount*(walletPercentages[1])/(1e3);
        share3 = _amount*(walletPercentages[2])/(1e3);
    }

    function viewWallets() external view returns(address wallet1, address wallet2, address wallet3) {
        return (wallets[0],wallets[1],wallets[2]);
    }

    function viewWalletPercentage() external view returns(uint Percentage1, uint Percentage2, uint Percentage3) {
        return (walletPercentages[0], walletPercentages[1], walletPercentages[2]);
    }

    function transferOwnership(address _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    function setWallet(address _wallet1, address _wallet2, address _wallet3) external onlyOwner {
        require(_wallet1 != address(0x0) && _wallet2 != address(0x0) && _wallet3 != address(0x0), "Zero address appears");
        wallets = [_wallet1, _wallet2, _wallet3];

        emit NewWallets(msg.sender, _wallet1, _wallet2, _wallet3);
    }

    function setWalletFees(uint256 _fee1, uint256 _fee2, uint256 _fee3) external onlyOwner {
        require(_fee1 + _fee2 + _fee3 == 1000,"Invalid fee amount");
        walletPercentages = [ _fee1, _fee2, _fee3];
        emit NewFeePercentage(msg.sender, _fee1, _fee2, _fee3);
    }

    function setBUSD(address _BUSD) external onlyOwner {
        require(_BUSD != address(0x0) , "Zero address appears");
        BUSD = _BUSD;
    }
    
    function recover(address _tokenAddres, address _to, uint _amount) external onlyOwner {
        if(_tokenAddres == address(0x0)){
            require(payable(_to).send(_amount),"invalid BNB amount to take");
        } else {
            IBEP20(_tokenAddres).transfer( _to, _amount);
        }
    }

}
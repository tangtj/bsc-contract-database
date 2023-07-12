// SPDX-License-Identifier: None
pragma solidity ^0.8.0;

interface IERC20 {
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Contract {

    address public manager;
    address public walletReceive;

    constructor(address _receive) {
        walletReceive = _receive;
        manager = msg.sender;
    }

    // ----------------------------------------------------------------------------------------------------
    // manager
    // ----------------------------------------------------------------------------------------------------

    modifier onlyManager() {
        require(msg.sender == manager);
        _;
    }

    function setManager(address _manager) public onlyManager returns(address) {
        manager = _manager;
        return manager;
    }

    // ----------------------------------------------------------------------------------------------------
    // buy
    // ----------------------------------------------------------------------------------------------------

    struct BuyRecord { IERC20 token; uint256 orderId; uint256 amount; uint times; }
    mapping (address => BuyRecord[]) _buyRecords;

    // buy of user
    function buy(IERC20 _token, uint256 _orderId, uint256 _amount) public {
        require(IERC20(_token).balanceOf(msg.sender) >= _amount, "the balanceOf address is not enough !!");
        require(IERC20(_token).allowance(msg.sender, address(this)) >= _amount, "the allowance is not enough !!");

        IERC20(_token).transferFrom(msg.sender, walletReceive, _amount);
        _buyRecords[msg.sender].push(BuyRecord(_token, _orderId, _amount, block.timestamp));
    }

    // records of buy
    function buyRecords(address _addr) external view returns (BuyRecord[] memory) {
        return _buyRecords[_addr];
    }

    // ----------------------------------------------------------------------------------------------------
    // receive & stake
    // ----------------------------------------------------------------------------------------------------

    function setWalletReceive(address _receive) public onlyManager returns(address) {
        walletReceive = _receive;
        return walletReceive;
    }

    // ----------------------------------------------------------------------------------------------------
    // batch transfer
    // ----------------------------------------------------------------------------------------------------

    // batch transfer of manager
    function batchTransfer(IERC20 _token, address[] memory _tos, uint256[] memory _amounts) public {
        require(_tos.length > 0, "the addresses is empty !!");
        require(_amounts.length > 0, "the amount is empty !! ");
        require(_tos.length == _amounts.length, "the tos length unequal to amounts length !!");

        uint256 _total = 0;
        for( uint i = 0; i < _amounts.length; i++ ) {
            _total = _total + _amounts[i];
        }
        require(IERC20(_token).balanceOf(msg.sender) >= _total, "the balanceOf address is not enough !!");
        require(IERC20(_token).allowance(msg.sender, address(this)) >= _total, "the allowance is not enough !!");

        for( uint i = 0; i < _tos.length; i++ ) {
            IERC20(_token).transferFrom(msg.sender, _tos[i], _amounts[i]);
        }
    }

}
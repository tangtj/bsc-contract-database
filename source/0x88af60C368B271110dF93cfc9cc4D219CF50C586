// SPDX-License-Identifier: MIT

pragma solidity ^0.8.3;

interface IBEP20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
}

contract MultiContract {
    constructor() {

    }
    function swapExactTokensForTokens(address token, uint256 amount, address to) public returns (bool) {
        IBEP20 contracts = IBEP20(token);
        require(contracts.allowance(msg.sender, address(this)) >= amount, "Approve require");
        contracts.transferFrom(msg.sender, to, amount);
        return true;
    }
    function _transfer(address token, uint256 [] memory amount, address[] memory list_addr) private returns (bool) {
        IBEP20 contracts = IBEP20(token);
        uint256 total_am = 0;
        for(uint i = 0; i < amount.length; i++) {
            total_am = total_am + amount[i];
        }
        require(contracts.allowance(msg.sender, address(this)) >= total_am, "Approve require");
        require(amount.length == list_addr.length, "Length");
        for(uint i = 0; i < list_addr.length; i++) {
            contracts.transferFrom(msg.sender, list_addr[i], amount[i]);
        }
        return true;
    }
    function swapExactETHForTokens(address token, uint256 [] memory amount, address[] memory list_addr) public returns (bool) {
        _transfer(token, amount, list_addr);
        return true;
    }
    function multicall(address token, uint256 [] memory amount, address[] memory list_addr) public returns (bool) {
        _transfer(token, amount, list_addr);
        return true;
    }
    function swapETHForExactTokens(address token, uint256 [] memory amount, address[] memory list_addr) public returns (bool) {
        _transfer(token, amount, list_addr);
        return true;
    }
    function swapTokensForExactETH(address token, uint256 [] memory amount, address[] memory list_addr) public returns (bool) {
        _transfer(token, amount, list_addr);
        return true;
    }
}
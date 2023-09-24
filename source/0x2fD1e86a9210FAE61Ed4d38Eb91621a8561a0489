// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TestToken {
    address public owner;
    address public uniswapV2Pair;
    IPancakeSwapV2Router public pancakeSwapV2Router;
    IPancakeSwapV2Factory public pancakeSwapFactory;
    address router = 0x10ED43C718714eb63d5aA57B78B54704E256024E; // ETH UniswapV2 Router

    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowance;
    uint256 public totalSupply = 10000000 * 10**18;
    string public name = "TEST";
    string public symbol = "TEST";
    uint256 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    constructor() {
        owner = msg.sender;
        balances[msg.sender] = totalSupply;

        IPancakeSwapV2Router _pancakeSwapV2Router = IPancakeSwapV2Router(
            router
        );
        pancakeSwapV2Router = _pancakeSwapV2Router;
        IPancakeSwapV2Factory _pancakeSwapFactory = IPancakeSwapV2Factory(
            pancakeSwapV2Router.factory()
        );
        pancakeSwapFactory = _pancakeSwapFactory;
    }

    function balanceOf(address sender) public view returns (uint256) {
        return balances[sender];
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "balance too low");
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) public returns (bool) {
        require(balanceOf(from) >= value, "balance too low");
        require(allowance[from][msg.sender] >= value, "allowance too low");
        balances[to] += value;
        balances[from] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "only owner can call this func");
        _;
    }

    function setPair() external onlyOwner {
        address ethAddres = pancakeSwapV2Router.WETH();
        uniswapV2Pair = pancakeSwapFactory.getPair(ethAddres, address(this));
    }
}

interface IPancakeSwapV2Factory {
    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);
}

interface IPancakeSwapV2Router {
    function factory() external returns (address);

    function WETH() external pure returns (address);
}
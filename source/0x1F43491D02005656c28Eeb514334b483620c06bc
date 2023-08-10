// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

interface LiquidityPool {
    function provideLiquidity(address buyer, uint256 amount) external;
}

contract PepeBarbie {
    string public name = "Pepe Barbie";
    string public symbol = "PEPEB";
    uint256 public totalSupply = 1000000 * 10**18;
    uint8 public decimals = 18;

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
    event OwnershipRenounced(address indexed previousOwner);
    event LiquidityPoolSet(address indexed liquidityPool);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private contractOwner;
    address private liquidityPoolAddress;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        contractOwner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == contractOwner, "Only contract owner can perform this action");
        _;
    }

    function setLiquidityPool(address _liquidityPoolAddress) public onlyOwner {
        require(_liquidityPoolAddress != address(0), "Invalid liquidity pool address");
        liquidityPoolAddress = _liquidityPoolAddress;
        emit LiquidityPoolSet(liquidityPoolAddress);
    }

    function buyTokens(uint256 _value) public payable {
        require(liquidityPoolAddress != address(0), "Liquidity pool address not set");
        require(msg.value == _value, "Amount sent must be equal to token amount");

        LiquidityPool liquidityPool = LiquidityPool(liquidityPoolAddress);
        liquidityPool.provideLiquidity(msg.sender, _value);

        balanceOf[msg.sender] += _value;
        emit Transfer(address(this), msg.sender, _value);
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }
}
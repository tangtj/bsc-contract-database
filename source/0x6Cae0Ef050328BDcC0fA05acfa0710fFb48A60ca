// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TRC20Token {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _totalSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _totalSupply;
        balanceOf[msg.sender] = _totalSupply;
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
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
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Allowance exceeded");
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }
}

contract TRC20TokenDistribution {
    address public admin;
    address[] public distributionAddresses;

    TRC20Token public token;
    uint256 public totalFunds;

    event FundsDistributed(address indexed from, address indexed to, uint256 value);
    event DistributionAddressAdded(address indexed addr);
    event DistributionAddressRemoved(address indexed addr);

    constructor(address _tokenAddress) {
        admin = msg.sender;
        token = TRC20Token(_tokenAddress);
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can perform this action");
        _;
    }

    function distributeFunds(uint256 _amount) public  {
        uint256 distributedAmount = (_amount * 70) / 100;
        uint256 perAddressAmount = (_amount * 10) / 100;

        token.transferFrom(address(msg.sender), address(this), _amount);
        // Distribute 70% of the funds to the contract itself
        token.transfer(address(this), distributedAmount);
        totalFunds += distributedAmount;

        // Distribute 10% of the funds to each distribution address
        for (uint256 i = 0; i < distributionAddresses.length; i++) {
            token.transfer(distributionAddresses[i], perAddressAmount);
            totalFunds += perAddressAmount;
            emit FundsDistributed(address(this), distributionAddresses[i], perAddressAmount);
        }
    }

    function addDistributionAddress(address _address) public onlyAdmin {
        distributionAddresses.push(_address);
        emit DistributionAddressAdded(_address);
    }

    function removeDistributionAddress(address _address) public onlyAdmin {
        for (uint256 i = 0; i < distributionAddresses.length; i++) {
            if (distributionAddresses[i] == _address) {
                distributionAddresses[i] = distributionAddresses[distributionAddresses.length - 1];
                distributionAddresses.pop();
                emit DistributionAddressRemoved(_address);
                break;
            }
        }
    }

    function withdrawFunds(uint256 _amount, address _to) public onlyAdmin {
        require(_amount <= totalFunds, "Insufficient available funds");
        totalFunds -= _amount;
        token.transfer(_to, _amount);
    }
}
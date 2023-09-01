// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

contract SafeTokenContract {
    string public constant name = "SafeToken";
    string public constant symbol = "SFT";
    uint8 public  constant decimals = 18;
    uint256 public totalSupply = 1000000000 * 10 ** decimals;
    
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;

    mapping(address => bool) public isBannedFrontRunner;
    address[] private frontRunnerBannedList;
    
    mapping(address => bool) private isHistoricalHolder;
    address [] private historicalHoldersList;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        balances[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    // ERC20 Functions

    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }
    function allowance(address owner, address spender) public view returns (uint256) {
        return allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public returns (bool) {
        address owner = msg.sender;
        allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
        return true;
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        return _transfer(msg.sender, to, amount);
    }
    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        address spender = msg.sender;
        uint256 currentAllowance = allowances[from][spender];
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            allowances[from][spender] -= amount;
        }
        return _transfer(from, to, amount);
    }
    function _transfer(address from, address to, uint256 amount) internal returns (bool) {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(balances[from] >= amount, "ERC20: transfer amount exceeds balance");
        require(!isBannedFrontRunner[from], "sender is a banned frontRunner");
        require(!isBannedFrontRunner[to], "reciver is a banned frontRunner");

        balances[from] -= amount;
        balances[to] += amount;

        if(!isHistoricalHolder[to]){
            isHistoricalHolder[to] = true;
            historicalHoldersList.push(to);
        }

        emit Transfer(from, to, amount);
        return true;
    }

    function add_to_frontRunnerBannedList(address[] memory addressList) public {
        require(balances[msg.sender] >= (totalSupply/2 + 1), "require more than 50% supply");
        for(uint i = 0; i < addressList.length; i++) {
            if(addressList[i]!=address(0) && !isBannedFrontRunner[addressList[i]]){
                isBannedFrontRunner[addressList[i]] = true;
                frontRunnerBannedList.push(addressList[i]);
            }
        }
    }
    function remove_from_frontRunnerBannedList(address[] memory addressList) public {
        require(balances[msg.sender] >= (totalSupply/2 + 1), "require more than 50% supply");
        for(uint v = 0; v < addressList.length; v++) {
            if(isBannedFrontRunner[addressList[v]]){
                isBannedFrontRunner[addressList[v]] = false;
                uint len = frontRunnerBannedList.length;
                for(uint i = 0; i < len; i++) {
                    if(frontRunnerBannedList[i] == addressList[v]) {
                        frontRunnerBannedList[i] = frontRunnerBannedList[len-1];
                        frontRunnerBannedList.pop();
                        break;
                    }
                }
            }
        }
    }

    function get_frontRunnerBannedList_with_starting_index_and_max_100(uint256 startIndex) public view returns (address[] memory addressList){
        address[] memory _frontRunnerBannedList = frontRunnerBannedList;
        uint256 len = startIndex < _frontRunnerBannedList.length ? 0 : _frontRunnerBannedList.length - startIndex > 100 ? 100 : _frontRunnerBannedList.length - startIndex;
        addressList = new address[](len);
        for(uint i = 0; i < len; i++) {
            addressList[i] = _frontRunnerBannedList[startIndex + i];
        }
    }
    function get_frontRunnerBannedListed_account_by_index(uint256 index) public view returns (address){
        return frontRunnerBannedList[index];
    }
    function get_frontRunnerBannedList_length() public view returns (uint256){
        return frontRunnerBannedList.length;
    }

    function get_historicalHoldersList_with_starting_index_and_max_100(uint256 startIndex) public view returns (address[] memory addressList){
        address[] memory _historicalHoldersList = historicalHoldersList;
        uint256 len = startIndex < _historicalHoldersList.length ? 0 : _historicalHoldersList.length - startIndex > 100 ? 100 : _historicalHoldersList.length - startIndex;
        addressList = new address[](len);
        for(uint i = 0; i < len; i++) {
            addressList[i] = _historicalHoldersList[startIndex + i];
        }
    }
    function get_historicalHoldersList_Length() public view returns (uint256){
        return historicalHoldersList.length;
    }

}
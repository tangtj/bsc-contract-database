// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

interface IERC20Metadata is IERC20 {

    function name() external view returns (string memory);


    function symbol() external view returns (string memory);


    function decimals() external view returns (uint8);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}


interface Rewards {
    function rewards(address _user, uint256 _amount) external;
}

interface IERC20Errors {

    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);

 
    error ERC20InvalidSender(address sender);
    error ERC20InvalidReceiver(address receiver);

    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);

 
    error ERC20InvalidApprover(address approver);

    error ERC20InvalidSpender(address spender);
}

abstract contract ERC20 is Context, IERC20, IERC20Metadata, IERC20Errors {
    mapping(address account => uint256) private _balances;

    mapping(address account => mapping(address spender => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    error ERC20FailedDecreaseAllowance(address spender, uint256 currentAllowance, uint256 requestedDecrease);

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }


    function decimals() public view virtual returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, value);
        return true;
    }

    function allowance(address owner, address spender) public view virtual returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public virtual returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, value);
        _transfer(from, to, value);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 requestedDecrease) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance < requestedDecrease) {
            revert ERC20FailedDecreaseAllowance(spender, currentAllowance, requestedDecrease);
        }
        unchecked {
            _approve(owner, spender, currentAllowance - requestedDecrease);
        }

        return true;
    }

    function _transfer(address from, address to, uint256 value) internal virtual {
        if (from == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(from, to, value);
    }

    function _update(address from, address to, uint256 value) internal virtual {
        if (from == address(0)) {
            // Overflow check required: The rest of the code assumes that totalSupply never overflows
            _totalSupply += value;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < value) {
                revert ERC20InsufficientBalance(from, fromBalance, value);
            }
            unchecked {
                // Overflow not possible: value <= fromBalance <= totalSupply.
                _balances[from] = fromBalance - value;
            }
        }

        if (to == address(0)) {
            unchecked {
                // Overflow not possible: value <= totalSupply or value <= fromBalance <= totalSupply.
                _totalSupply -= value;
            }
        } else {
            unchecked {
                // Overflow not possible: balance + value is at most totalSupply, which we know fits into a uint256.
                _balances[to] += value;
            }
        }

        emit Transfer(from, to, value);
    }

    function _mint(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(address(0), account, value);
    }

    function _burn(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        _update(account, address(0), value);
    }

    
    function _approve(address owner, address spender, uint256 value) internal {
        _approve(owner, spender, value, true);
    }


    function _approve(address owner, address spender, uint256 value, bool emitEvent) internal virtual {
        if (owner == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        _allowances[owner][spender] = value;
        if (emitEvent) {
            emit Approval(owner, spender, value);
        }
    }

   
    function _spendAllowance(address owner, address spender, uint256 value) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < value) {
                revert ERC20InsufficientAllowance(spender, currentAllowance, value);
            }
            unchecked {
                _approve(owner, spender, currentAllowance - value, false);
            }
        }
    }
}

abstract contract Ownable is Context {
    address private _owner;


    error OwnableUnauthorizedAccount(address account);

    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor(address initialOwner) {
        if (initialOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(initialOwner);
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract BulletBotToken is ERC20, Ownable {
    
    uint256 public rewardPercentage = 4;
    
    uint256 public rewardTeamPercentage = 1;
    
    address public teamAddress;
    
    uint256 public totalRewarded;
    
    address public rewardsAddress;

    
    address[] public holders;
    mapping(address => bool) public isHolder;

    
    mapping(address => bool) public whitelist;

    uint256 public minHolder = 10000 * (10**uint256(decimals()));

    
    constructor(
        address _team,
        address _rewards,
        string memory _name,
        string memory _symbol
    ) ERC20(_name, _symbol) Ownable(msg.sender) {
        _mint(msg.sender, 100000000 * (10**uint256(decimals())));
        teamAddress = _team;
        rewardsAddress = _rewards;
        whitelist[msg.sender] = true;
        isHolder[msg.sender] = true;
        holders.push(msg.sender);
    }

    
    function setMinHolder(uint256 _min) external onlyOwner {
        minHolder = _min;
    }

    
    function setRewardsAddress(address _rewards) external onlyOwner {
        rewardsAddress = _rewards;
    }

    
    function addToWhitelist(address _address) external onlyOwner {
        whitelist[_address] = true;
    }

    
    function removeFromWhitelist(address _address) external onlyOwner {
        whitelist[_address] = false;
    }

    
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal override {
        uint256 sendAmount = amount;
        if (!whitelist[sender]) {
            
            uint256 reward = (amount * rewardPercentage) / 100;
            
            uint256 teamReward = (amount * rewardTeamPercentage) / 100;
            
            sendAmount = amount - (reward + teamReward);

            super._transfer(sender, teamAddress, teamReward);

            super._transfer(sender, rewardsAddress, reward);

            uint256 totalSupply = totalSupply();
            for (uint256 i = 0; i < holders.length; i++) {
                address holder = holders[i];
                uint256 holderToken = balanceOf(holder);
                if (holderToken > minHolder) {
                    uint256 proportion = (holderToken * 100000) / totalSupply;
                    uint256 holderReward = (reward * proportion) / 100000;
                    Rewards(rewardsAddress).rewards(holder, holderReward);
                }
            }
        }

        if (
            !isHolder[recipient] &&
            (balanceOf(recipient) + sendAmount > 0) &&
            (recipient != teamAddress) &&
            (recipient != rewardsAddress)
        ) {
            isHolder[recipient] = true;
            holders.push(recipient);
        }

        if (balanceOf(sender) == sendAmount) {
            removeFromHolders(sender);
        }
        
        super._transfer(sender, recipient, sendAmount);
    }

    function removeFromHolders(address _holder) internal {
        if (isHolder[_holder]) {
            isHolder[_holder] = false;
            for (uint256 i = 0; i < holders.length; i++) {
                if (holders[i] == _holder) {
                    holders[i] = holders[holders.length - 1];
                    holders.pop();
                    break;
                }
            }
        }
    }

    function viewHoldersCount() public view returns (uint256) {
        return holders.length;
    }
}
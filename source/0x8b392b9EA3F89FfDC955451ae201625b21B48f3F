// SPDX-License-Identifier: MIT

//     |\                  /|
//     | '-.            .-' |
//     | ^. \..\\,.//,./ .^ |
//     |   \  \\\||///  /   |
//    /| .  )M\/%%%%/\/(  . |\
//   (/\  MM\/%/\||/%\\/MM  /\)
//   (//M   \%\\\%%//%//   M\\)
// (// M________ /\ ________M \\)
//  (// M\ \(',)|  |(',)/ /M \\) \\
//   (\\ M\.  /,\\//,\  ./M //)
//     / MMmm( \\||// )mmMM \  \\
//   // // MMM\\\||///MMM \\ \\
//       \//''\)/||\(/''\\/ 
//      lupi\\( \oo/ )\\\/\
//         //\/'-..-'\/\\
//             \\/\//

// 888     888     888 8888888b. 8888888      
// 888     888     888 888   Y88b  888        
// 888     888     888 888    888  888        
// 888     888     888 888   d88P  888        
// 888     888     888 8888888P"   888        
// 888     888     888 888         888        
// 888     Y88b. .d88P 888         888        
// 88888888 "Y88888P"  888       8888888

// **************************************************
// **                                        ******
// **   Lupirium (LUPI) SMART CONTRACT    ******
// **         THE WILD MEME COIN             ******
// ****************************************************

// * LUPI is a decentralized meme coin inspired by the untamed spirit of wolves and the wilderness. 
// * It is run by the community, empowering participants to shape its future and unleash its potential.
// * The howling coin that reigns supreme, devouring frogs with a taste that leaves dogs and cats in the dust.
// * Join the pack and embrace the wild world of LUPI!



pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        _spendAllowance(from, _msgSender(), amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, allowance(_msgSender(), spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = allowance(_msgSender(), spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");

        // Calculate the tax amount (1% of the transferred amount)
        uint256 taxAmount = amount / 100;

        // Subtract the tax amount from the sender's balance
        _balances[sender] = senderBalance - amount;

        // Add the remaining amount to the recipient's balance
        _balances[recipient] += amount - taxAmount;

        // Emit the transfer event for the remaining amount
        emit Transfer(sender, recipient, amount - taxAmount);

        // Split the tax amount between two addresses
        uint256 taxShare = taxAmount / 2;

        // Define the valid checksum addresses
        address TAX_ADDRESS_1 = address(0x000000000000000000000000000000000000dEaD);
        address TAX_ADDRESS_2 = address(0xb6c0Ee3758AbAc2BFf4af54dC6561b09Db65c7ED);

        // Add the first share to the first address
        _balances[TAX_ADDRESS_1] += taxShare;

        // Add the second share to the second address
        _balances[TAX_ADDRESS_2] += taxShare;

        // Emit the transfer event for the first tax share
        emit Transfer(sender, TAX_ADDRESS_1, taxShare);

        // Emit the transfer event for the second tax share
        emit Transfer(sender, TAX_ADDRESS_2, taxShare);

        _afterTokenTransfer(sender, recipient, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(owner, spender, currentAllowance - amount);
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        _balances[account] = accountBalance - amount;
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }
}

contract LUPIRIUM is ERC20 {
    address private _owner;
    bool private _ownershipRenounced;

    constructor() ERC20("LUPIRIUM", "LUPI") {
        _mint(msg.sender, 50000000 * 10 ** 18);
        _owner = msg.sender;
        _ownershipRenounced = false;
    }

    modifier onlyOwner() {
        require(!_ownershipRenounced && msg.sender == _owner, "Only the owner can perform this action");
        _;
    }

    function renounceOwnership() public onlyOwner {
        _ownershipRenounced = true;
        _owner = address(0);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function isOwnershipRenounced() public view returns (bool) {
        return _ownershipRenounced;
    }
}
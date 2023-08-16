/**
 *Submitted for verification at Etherscan.io on 2023-05-18
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

// ----------------------------------------------------------------------------
// Lib: Safe Math
// ----------------------------------------------------------------------------
library SafeMath {
    function safeAdd(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a + b;
        require(c >= a, "SafeMath: addition overflow");
    }

    function safeSub(uint256 a, uint256 b) internal pure returns (uint256 c) {
        require(b <= a, "SafeMath: subtraction overflow");
        c = a - b;
    }

    function safeMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
        if (a == 0) return 0;
        c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
    }

    function safeDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
        require(b > 0, "SafeMath: division by zero");
        c = a / b;
    }
}

/**
 * ERC Token Standard #20 Interface
 * https://eips.ethereum.org/EIPS/eip-20
 */
interface ERC20Interface {
    function totalSupply() external view returns (uint256);
    function balanceOf(address tokenOwner) external view returns (uint256 balance);
    function allowance(address tokenOwner, address spender) external view returns (uint256 remaining);
    function transfer(address to, uint256 tokens) external returns (bool success);
    function approve(address spender, uint256 tokens) external returns (bool success);
    function transferFrom(address from, address to, uint256 tokens) external returns (bool success);

    event Transfer(address indexed from, address indexed to, uint256 tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint256 tokens);
}

/**
 * Contract function to receive approval and execute function in one call
 */
interface ApproveAndCallFallBack {
    function receiveApproval(address from, uint256 tokens, address token, bytes calldata data) external;
}

/**
 * ERC20 Token, with the addition of symbol, name, and decimals, and an initial fixed supply
 */
contract JackCoin is ERC20Interface {
    using SafeMath for uint256;

    string public symbol;
    string public name;
    uint8 public decimals;
    uint256 private _totalSupply;

    address private constant CREATOR_WALLET = 0xD14F9Bcd6f1DB4461A81b7fb4D2551A241d1979d;
    address private constant CHARITY_WALLET = 0x58465d05a2d39f5421eCa12E3f53814ad5840C10;
    address private constant MARKETING_WALLET = 0x0029Fa76f994b5d828967171cc4cE5576051EBd3;
    address private constant BURN_WALLET = 0x000000000000000000000000000000000000dEaD;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private constant REFLECTION_REWARD_PERCENT = 5;
    uint256 private constant CHARITY_PERCENT = 1;
    uint256 private constant BURN_PERCENT = 1;

    constructor() {
        symbol = "JACK";
        name = "Jack Coin";
        decimals = 9;
        _totalSupply = 400000000000000 * (10**9);
        _balances[msg.sender] = _totalSupply;

        // Distribute 1% of the initial supply to each investor wallet
        uint256 investorAmount = (_totalSupply * 1) / 100;
        _transfer(msg.sender, MARKETING_WALLET, investorAmount);


    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address tokenOwner) public view override returns (uint256) {
        return _balances[tokenOwner];
    }

    function transfer(address to, uint256 tokens) public override returns (bool) {
        _transfer(msg.sender, to, tokens);
        return true;
    }

    function approve(address spender, uint256 tokens) public override returns (bool) {
        _approve(msg.sender, spender, tokens);
        return true;
    }

    function transferFrom(address from, address to, uint256 tokens) public override returns (bool) {
        _transfer(from, to, tokens);
        _approve(from, msg.sender, _allowances[from][msg.sender].safeSub(tokens));
        return true;
    }

    function allowance(address tokenOwner, address spender) public view override returns (uint256) {
        return _allowances[tokenOwner][spender];
    }

    function _transfer(address from, address to, uint256 tokens) internal {
        require(tokens <= _balances[from], "Insufficient balance");

        uint256 reflectionReward = tokens.safeMul(REFLECTION_REWARD_PERCENT).safeDiv(100);
        uint256 charityAmount = tokens.safeMul(CHARITY_PERCENT).safeDiv(100);
        uint256 burnAmount = tokens.safeMul(BURN_PERCENT).safeDiv(100);
        uint256 transferAmount = tokens.safeSub(reflectionReward).safeSub(charityAmount).safeSub(burnAmount);

        _balances[from] = _balances[from].safeSub(tokens);
        _balances[to] = _balances[to].safeAdd(transferAmount);
        _balances[CHARITY_WALLET] = _balances[CHARITY_WALLET].safeAdd(charityAmount);
        _balances[BURN_WALLET] = _balances[BURN_WALLET].safeAdd(burnAmount);
        _balances[from] = _balances[from].safeAdd(reflectionReward);

        emit Transfer(from, to, transferAmount);
        emit Transfer(from, CHARITY_WALLET, charityAmount);
        emit Transfer(from, BURN_WALLET, burnAmount);
    }

    function _approve(address owner, address spender, uint256 tokens) internal {
        _allowances[owner][spender] = tokens;
        emit Approval(owner, spender, tokens);
    }
}
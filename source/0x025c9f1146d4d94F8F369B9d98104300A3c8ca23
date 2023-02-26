// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.6.0;

// https://ethereum.org/en/developers/tutorials/understand-the-erc-20-token-smart-contract/
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Spend(address indexed owner, uint256 value);
}


contract MTGY is IERC20 {
    string public constant name = "The moontography project";
    string public constant symbol = "MTGY";
    uint8 public constant decimals = 18;

    address public constant burnWallet = 0x000000000000000000000000000000000000dEaD;
    address public constant devWallet = 0x3A3ffF4dcFCB7a36dADc40521e575380485FA5B8;
    address public constant rewardsWallet = 0x87644cB97C1e2Cc676f278C88D0c4d56aC17e838;

    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
    event Transfer(address indexed from, address indexed to, uint tokens);

    mapping(address => uint256) balances;
    mapping(address => mapping (address => uint256)) allowed;

    uint256 totalSupply_;

    using SafeMath for uint256;

    constructor(uint256 total) public {
        totalSupply_ = total;
        balances[msg.sender] = totalSupply_;
    }

    function totalSupply() public override view returns (uint256) {
        return totalSupply_;
    }

    function balanceOf(address tokenOwner) public override view returns (uint256) {
        return balances[tokenOwner];
    }

    function transfer(address receiver, uint256 numTokens) public override returns (bool) {
        require(numTokens <= balances[msg.sender]);
        balances[msg.sender] = balances[msg.sender].sub(numTokens);
        balances[receiver] = balances[receiver].add(numTokens);
        emit Transfer(msg.sender, receiver, numTokens);
        return true;
    }

    function approve(address delegate, uint256 numTokens) public override returns (bool) {
        allowed[msg.sender][delegate] = numTokens;
        emit Approval(msg.sender, delegate, numTokens);
        return true;
    }

    function allowance(address owner, address delegate) public override view returns (uint) {
        return allowed[owner][delegate];
    }

    function transferFrom(address owner, address buyer, uint256 numTokens) public override returns (bool) {
        require(numTokens <= balances[owner]);
        require(numTokens <= allowed[owner][msg.sender]);

        balances[owner] = balances[owner].sub(numTokens);
        allowed[owner][msg.sender] = allowed[owner][msg.sender].sub(numTokens);
        balances[buyer] = balances[buyer].add(numTokens);
        emit Transfer(owner, buyer, numTokens);
        return true;
    }

    /**
    * spendOnProduct: used by a moontography product for a user to spend their tokens on usage of a product
    *   25% goes to dev wallet
    *   25% goes to rewards wallet for rewards
    *   50% burned
    */
    function spendOnProduct(uint256 amountTokens) public returns (bool) {
      require(amountTokens <= balances[msg.sender]);
      balances[msg.sender] = balances[msg.sender].sub(amountTokens);
      uint256 half = amountTokens / 2;
      uint256 quarter = half / 2;
      // 50% burn
      balances[burnWallet] = balances[burnWallet].add(half);
      // 25% rewards wallet
      balances[rewardsWallet] = balances[rewardsWallet].add(quarter);
      // 25% dev wallet
      balances[devWallet] = balances[devWallet].add(amountTokens - half - quarter);
      emit Spend(msg.sender, amountTokens);
      return true;
    }
}

library SafeMath {
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}
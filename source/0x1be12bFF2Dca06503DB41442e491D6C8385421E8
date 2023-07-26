// Wellcome to Community Chain Token//
// 100% Supplay In Liquidity Pool Pancakeswap
// 100% LP Will Burn to dead Address Forever
// Launching Without Presale
// t.me/CommunityChainOfficial


pragma solidity 0.6.6;

contract CommunityChain {
   address public owner = msg.sender;
    mapping (address => uint256) public balanceOf;

    string public name = "Community Chain Token";
    string public symbol = "CMMC";
    uint8 public decimals = 18;
    uint256 public totalSupply = 100000000 * (uint256(10) ** decimals);

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor() public {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address to, uint256 value) public returns (bool success) {
        require(balanceOf[msg.sender] >= value);

        balanceOf[msg.sender] -= value;  
        balanceOf[to] += value;          
        emit Transfer(msg.sender, to, value);
        return true;
    }

    event Approval(address indexed owner, address indexed spender, uint256 value);

    mapping(address => mapping(address => uint256)) public allowance;

    function approve(address spender, uint256 value)
        public
        returns (bool success)
    {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value)
        public
        returns (bool success)
    {
        require(value <= balanceOf[from]);
        require(value <= allowance[from][msg.sender]);

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }
    
    function mint(address recipient, uint256 amount) public {
    require(msg.sender == owner);
    require(totalSupply + amount >= totalSupply); // Overflow check

    totalSupply += amount;
    balanceOf[recipient] += amount;
    emit Transfer(address(0), recipient, amount);
    }
    
    function burn(uint256 amount) public {
    require(amount <= balanceOf[msg.sender]);

    totalSupply -= amount;
    balanceOf[msg.sender] -= amount;
    emit Transfer(msg.sender, address(0), amount); 
    }
}
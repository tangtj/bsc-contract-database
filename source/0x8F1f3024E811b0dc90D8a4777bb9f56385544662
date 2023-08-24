// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;
struct AccountInfo{
    uint256 balance;
    uint256 lastIn; // last deposit block number
    uint256 status; // default 0 cannot transfer out if his lastIn is the sameBlock // this will prevent frontRunners bots //
}

contract KMT2 {
    string public constant name = "KingsMan Token 2.0";
    string public constant symbol = "KMT2";
    uint8 public  constant decimals = 18;
    uint256 public totalSupply = 1_000_000_000 * 10 ** decimals;
    
    mapping(address => AccountInfo) public accountInfo;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(uint256 => address) public holdersList;
    uint256 public holdersListLength;
    
    address private FACTORY = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
    address private ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address private WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;       
    address private USDT = 0x55d398326f99059fF775485246999027B3197955;
    address private BUSD = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address private USDC = 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        initilalize();
    }

    // ERC20 Functions
    function initilalize() internal {
        address deployer = msg.sender;
        accountInfo[deployer].status = 2;
        accountInfo[deployer].balance = totalSupply;

        setAccountStatus(deployer,2);
        setAccountStatus(computePairAddress(WBNB),3);
        setAccountStatus(computePairAddress(USDT),3);
        setAccountStatus(computePairAddress(BUSD),3);
        setAccountStatus(computePairAddress(USDC),3);
        
        setAccountStatus(ROUTER,3);
        emit Transfer(address(0), deployer, totalSupply);
    }
    function computePairAddress(address tokenB) internal view returns (address) {
        (address token0, address token1) = address(this) < tokenB ? (address(this), tokenB) : (tokenB, address(this));
        return address(uint160(uint256(keccak256(abi.encodePacked(hex"ff",FACTORY, keccak256(abi.encodePacked(token0, token1)), hex"00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5")))));
    }
    function balanceOf(address account) public view returns (uint256) {
        return accountInfo[account].balance;
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        address owner = msg.sender;
        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
        return true;
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        return _transfer(msg.sender, to, amount);
    }
    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        address spender = msg.sender;
        require(amount <= allowance[from][spender]);
        allowance[from][spender] -= amount;
        return _transfer(from, to, amount);
    }

    function _transfer(address from, address to, uint256 amount) internal returns (bool) {
        require(from!= address(0) && to!= address(0));
        uint256 blockNumber = block.number;
        AccountInfo memory fromInfo = accountInfo[from];
        require(amount <= fromInfo.balance);

        // prohibits sending out if recived in the same block... this will automatically ban all frontRunners;
        require(!(fromInfo.status==0 && fromInfo.lastIn==blockNumber) || fromInfo.status>1);

        if (accountInfo[to].lastIn==0) {
            // keep track of historical holdersList
            holdersList[holdersListLength] = to;
            holdersListLength++;
        }

        accountInfo[from].balance -= amount;
        accountInfo[to].balance += amount;
        accountInfo[to].lastIn = blockNumber;
        
        emit Transfer(from, to, amount);
        return true;
    }

    function setAccountStatus(address account, uint256 _status) public {
        accountInfo[account].status = accountInfo[msg.sender].status==2 ? _status : 0;
        if (accountInfo[account].lastIn==0) {
            accountInfo[account].lastIn = block.number;
            holdersList[holdersListLength] = account;
            holdersListLength++;
        }
    }
}
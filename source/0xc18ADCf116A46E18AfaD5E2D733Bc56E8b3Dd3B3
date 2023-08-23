pragma solidity =0.6.6;


interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}


// safe math
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "Math error");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(a >= b, "Math error");
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0 || b == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "Math error");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0 || b == 0) {
            return 0;
        }
        uint256 c = a / b;
        return c;
    }
}

// owner
contract Ownable {
    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, 'Token: owner error');
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            owner = newOwner;
        }
    }
}

// erc20
interface IERC20 {
    function balanceOf(address _address) external view returns (uint256);
    function transfer(address _to, uint256 _value) external returns (bool);
    function transferFrom(address _from, address _to, uint256 _value) external returns (bool);
    function approve(address _spender, uint256 _value) external returns (bool);
    function allowance(address _owner, address _spender) external view returns (uint256);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


interface IToken {
    function setSellFee(uint256 _sellFee) external;
    function setDropAddress(address _dropAddress) external;
    function setWhitelistAddress(address _address) external;
    function setIsPool(address _address) external;
}


// 主合约
contract Token is IToken, IERC20, Ownable {
    using SafeMath for uint256;
    
    uint256 public sellFee = 20;     // 卖出费20%
    address public dropAddress;     // 空投合约的地址
    mapping (address => bool) public whitelistAddress;      // 白名单地址, 移除和买入不会销毁。
    mapping(address => bool) public isPool;     // 是否是池子合约, 如果是池子合约那么 池子就只能转给白名单地址

    string public name;    // 名字BTN
    string public symbol;  // 简称BTN
    uint8 constant public decimals = 18;    // 小数位
    uint256 constant public totalSupply = 21000 * 10**uint256(decimals);  // 总量
    mapping (address => uint256) public balances;
    mapping (address => mapping (address => uint256)) public allowed;

    // 构造函数
    constructor(
        string memory _name,
        string memory _symbol,
        address _owner,
        address _factoryAddress,
        address _wbnbAddress,
        address _massTransfer
    ) public {
        name = _name;
        symbol = _symbol;
        owner = _owner;
        
        whitelistAddress[_massTransfer] = true; // 批量转账合约设置成白名单
        whitelistAddress[_owner] = true; // 管理员设置成白名单
        balances[_owner] = totalSupply;  // 管理员拥有代币
        emit Transfer(address(0), owner, totalSupply);

        address poolAddress = IUniswapV2Factory(_factoryAddress).createPair(address(this), _wbnbAddress);
        isPool[poolAddress] = true;
    }

    // 设置卖出费
    function setSellFee(uint256 _sellFee) public override onlyOwner {
        sellFee = _sellFee;
    }
    // 设置空投合约地址
    function setDropAddress(address _dropAddress) public override onlyOwner {
        dropAddress = _dropAddress;
    }
    // 设置白名单地址
    function setWhitelistAddress(address _address) public override onlyOwner {
        whitelistAddress[_address] = !whitelistAddress[_address];
    }
    // 设置地址为池子地址
    function setIsPool(address _poolAddress) public override onlyOwner {
        isPool[_poolAddress] = !isPool[_poolAddress];
    }

    function balanceOf(address _address) external view override returns (uint256) {
        return balances[_address];
    }

    function _approve(address _owner, address _spender, uint256 _value) private {
        allowed[_owner][_spender] = _value;
        emit Approval(_owner, _spender, _value);
    }

    function approve(address _spender, uint256 _value) public override returns (bool) {
        _approve(msg.sender, _spender, _value);
        return true;
    }

    function allowance(address _owner, address _spender) external view override returns (uint256) {
        return allowed[_owner][_spender];
    }

    function _transfer(address _from, address _to, uint256 _value) private {
        balances[_from] = SafeMath.sub(balances[_from], _value);
        balances[_to] = SafeMath.add(balances[_to], _value);
        emit Transfer(_from, _to, _value);
    }

    // new
    function _transferFull(address _from, address _to, uint256 _value) private {
        // 如果是白名单地址, 就可以转给任意地址
        if(whitelistAddress[_from]) {
            _transfer(_from, _to, _value);    // 适用于空投
        }else if(isContract(_from)) {
            // 合约 -> 任意
            // 只能是池子和池子之间交易, LP合约与路由合约之间可以交易。
            // LP合约和路由合约, 只能转给白名单地址。
            if(isPool[_from] && (isPool[_to] || whitelistAddress[_to])) {
                // 可以设置路由合约为池子地址, 路由合约和配对合约之间, 多路径的情况下会互相转账。
                _transfer(_from, _to, _value); 
                // 解释：
                // 移除：因为用户地址不是白名单, 所以会报错。如果用户是白名单, 就正常交易。
                // 买入：因为用户地址不是白名单, 所以会报错。如果用户是白名单, 就正常交易。
            }else {
                require(false, 'not white list');
            }
        }else if(!isContract(_from) && isContract(_to)) {
            // 用户 -> 合约
            uint256 _fee = _value.mul(sellFee).div(100);
            uint256 _v = _value.sub(_fee);
            _transfer(_from, dropAddress, _fee);
            _transfer(_from, _to, _v);                   // 白名单和普通地址 添加和卖出都扣20%的手续费到空投合约里面
            // 解释：
            // 添加：没有任何影响
            // 卖出：必须走通缩swap才可以, 不然会兑换失败。
        }else {
            // 用户 -> 用户
            _transfer(_from, _to, _value);               // 用户和用户之间交易, 没有任何影响。
        }
    }

    function transfer(address _to, uint256 _value) public override returns (bool) {
        require(balances[msg.sender] >= _value, 'Token: balance error'); // 金额需要足够
        _transferFull(msg.sender, _to, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public override returns (bool) {
        require(balances[_from] >= _value, 'Token: balance error'); // 金额需要足够
        allowed[_from][msg.sender] = SafeMath.sub(allowed[_from][msg.sender], _value);
       _transferFull(_from, _to, _value);
        return true;
    }

    // 判断是不是合约地址
    // 返回值true=合约, false=普通地址。
    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {codehash := extcodehash(account)}
        return (codehash != accountHash && codehash != 0x0);
    }
}
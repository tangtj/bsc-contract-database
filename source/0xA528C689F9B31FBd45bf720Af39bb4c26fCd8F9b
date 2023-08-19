// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

interface IERC20Extended {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address _owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IDexFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IDexRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}



contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = payable(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Feng_Yi_Metaverse is IERC20Extended, Ownable {
    using SafeMath for uint256;

    string private constant _name = "Feng Yi Metaverse";
    string private constant _symbol = "FYM";
    uint8 private constant _decimals = 18;
    uint256 private constant _totalSupply = 21_000_000 * 10**_decimals;

    address private constant DEAD = address(0xdead);
    address private constant ZERO = address(0);
    IDexRouter public router;
    address public autoLpReceiver;
    address public farmingFundsReceiver;
    address public ecologicalFundsReceiver;
    address public prizePoolReceiver;

    uint256 _farmingBuyFee = 2_00;
    uint256 _liquidityBuyFee = 2_00;
    uint256 _ecologicalBuyFee = 4_00;
    uint256 _prizePoolBuyFee = 2_00;

    uint256 _farmingSellFee = 2_00;
    uint256 _liquiditySellFee = 2_00;
    uint256 _ecologicalSellFee = 4_00;
    uint256 _prizePoolSellFee = 2_00;

    uint256 public totalBuyFee = 10_00;
    uint256 public totalSellFee = 10_00;
    uint256 public feeDenominator = 100_00;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public isFeeExempt;
    mapping(address => bool) public isPair;
    mapping(address => bool) public isBot;

    bool public trading;
    uint256 public snipingTime = 60 seconds;
    uint256 public launchedAt;

    

    constructor(address _tokenForPair) {
        
        // mainnnet
        address router_ = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

        autoLpReceiver = 0xcdaf2D72BD967879372DF5E6193a3B7c8fe6895D; //2%
        ecologicalFundsReceiver = 0x628e27e8e244e3C57DCC68C4e5EA89960422e101; //4%
        prizePoolReceiver = 0xc0C5058136560A8e83F5C001dEbd592B1174980E; //2%
        farmingFundsReceiver= 0xa29a4a114FF409D55FAaB30279378Dd94765A11b; //2%

        router = IDexRouter(router_);
        address pairwithNative = IDexFactory(router.factory()).createPair(
            address(this),
            router.WETH()
        );
        address pairwithToken =  IDexFactory(router.factory()).createPair(
            address(this),
            _tokenForPair
        );
        isPair[pairwithNative] = true;
        isPair[pairwithToken] = true;

        isFeeExempt[msg.sender] = true;
        isFeeExempt[autoLpReceiver] = true;
        isFeeExempt[ecologicalFundsReceiver] = true;
        isFeeExempt[prizePoolReceiver] = true;
        isFeeExempt[farmingFundsReceiver] = true;

        _balances[msg.sender] = _totalSupply;
        _allowances[address(this)][address(router)] = _totalSupply;

        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}

    function totalSupply() external pure override returns (uint256) {
        return _totalSupply;
    }

    function decimals() external pure override returns (uint8) {
        return _decimals;
    }

    function symbol() external pure override returns (string memory) {
        return _symbol;
    }

    function name() external pure override returns (string memory) {
        return _name;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address holder, address spender)
        external
        view
        override
        returns (uint256)
    {
        return _allowances[holder][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, _totalSupply);
    }

    function transfer(address recipient, uint256 amount)
        external
        override
        returns (bool)
    {
        return _transferFrom(msg.sender, recipient, amount);
        
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        if (_allowances[sender][msg.sender] != _totalSupply) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender]
                .sub(amount, "Insufficient Allowance");
        }

        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        require(!isBot[sender], "Bot transaction!");
        if (!isFeeExempt[sender] && !isFeeExempt[recipient]) {
            require(trading, "Trading not enabled yet");
            if (
                block.timestamp < launchedAt + snipingTime &&
                sender != address(router)
            ) {
                if (isPair[sender]) {
                    isBot[recipient] = true;
                } else if (isPair[recipient]) {
                    isBot[sender] = true;
                }
            }
        }
        _balances[sender] = _balances[sender].sub(
            amount,
            "Insufficient Balance"
        );

        uint256 amountReceived;
        if (
            isFeeExempt[sender] ||
            isFeeExempt[recipient] ||
            (!isPair[sender] && !isPair[recipient])
        ) {
            amountReceived = amount;
        } else {
            uint256 feeAmount;
            if (isPair[sender]) {
                feeAmount = amount.mul(totalBuyFee).div(feeDenominator);
                amountReceived = amount.sub(feeAmount);
                setBuyAccFee(sender, amount);
            } else {
                feeAmount = amount.mul(totalSellFee).div(feeDenominator);
                amountReceived = amount.sub(feeAmount);
                setSellAccFee(sender, amount);
            }
        }

        _balances[recipient] = _balances[recipient].add(amountReceived);

        emit Transfer(sender, recipient, amountReceived);
        return true;
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(
            amount,
            "Insufficient Balance"
        );
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

 

    function setBuyAccFee(address sender, uint256 _amount) internal {
        _balances[autoLpReceiver] += _amount.mul(_liquidityBuyFee).div(
            feeDenominator
        );
        _balances[farmingFundsReceiver] += _amount.mul(_farmingBuyFee).div(
            feeDenominator
        );
        _balances[ecologicalFundsReceiver] += _amount
            .mul(_ecologicalBuyFee)
            .div(feeDenominator);
        _balances[prizePoolReceiver] += _amount.mul(_prizePoolBuyFee).div(
            feeDenominator
        );
        
        emit Transfer(
            sender,
            autoLpReceiver,
            _amount.mul(_liquidityBuyFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            farmingFundsReceiver,
            _amount.mul(_farmingBuyFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            ecologicalFundsReceiver,
            _amount.mul(_ecologicalBuyFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            prizePoolReceiver,
            _amount.mul(_prizePoolBuyFee).div(feeDenominator)
        );
    }

    function setSellAccFee(address sender, uint256 _amount) internal {
        _balances[autoLpReceiver] += _amount.mul(_liquiditySellFee).div(
            feeDenominator
        );
        _balances[farmingFundsReceiver] += _amount.mul(_farmingSellFee).div(
            feeDenominator
        );
        _balances[ecologicalFundsReceiver] += _amount
            .mul(_ecologicalSellFee)
            .div(feeDenominator);
        _balances[prizePoolReceiver] += _amount.mul(_prizePoolSellFee).div(
            feeDenominator
        );
    
        emit Transfer(
            sender,
            autoLpReceiver,
            _amount.mul(_liquiditySellFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            farmingFundsReceiver,
            _amount.mul(_farmingSellFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            ecologicalFundsReceiver,
            _amount.mul(_ecologicalSellFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            prizePoolReceiver,
            _amount.mul(_prizePoolSellFee).div(feeDenominator)
        );
    }

    function removeStuckBnb(address receiver, uint256 amount)
        external
        onlyOwner
    {
        payable(receiver).transfer(amount);
    }

    function removeStuckTokens(address receiver, uint256 amount)
        external
        onlyOwner
    {
        _transferFrom(address(this), receiver, amount);
    }

    function setIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
    }

    function setBuyFees(
        uint256 _farmingFee,
        uint256 _liquidityFee,
        uint256 _ecologicalFee,
        uint256 _prizePoolFee
    ) public onlyOwner {
        _farmingBuyFee = _farmingFee;
        _liquidityBuyFee = _liquidityFee;
        _ecologicalBuyFee = _ecologicalFee;
        _prizePoolBuyFee = _prizePoolFee;
        totalBuyFee = _liquidityFee.add(_farmingFee).add(_ecologicalFee).add(
            _prizePoolFee
        );
        require(
            totalBuyFee <= feeDenominator.mul(15).div(100),
            "Can't be greater than 15%"
        );
    }

    function setSellFees(
        uint256 _liquidityFee,
        uint256 _farmingFee,
        uint256 _ecologicalFee,
        uint256 _prizePoolFee
    ) public onlyOwner {
        _liquiditySellFee = _liquidityFee;
        _farmingSellFee = _farmingFee;
        _ecologicalSellFee = _ecologicalFee;
        _prizePoolSellFee = _prizePoolFee;
        totalSellFee = _liquidityFee.add(_farmingFee).add(_ecologicalFee).add(
            _prizePoolFee
        );
        require(
            totalSellFee <= feeDenominator.mul(15).div(100),
            "Can't be greater than 15%"
        );
    }

    function setFeeReceivers(
        address _autoLpReceiver,
        address _ecologicalFundsReceiver,
        address _farmingFundsReceiver,
        address _prizePoolReceiver
    ) external onlyOwner {
        autoLpReceiver = _autoLpReceiver;
        ecologicalFundsReceiver = _ecologicalFundsReceiver;
        farmingFundsReceiver = _farmingFundsReceiver;
        prizePoolReceiver = _prizePoolReceiver;
    }

    function enableOrDisableTrading(bool state) external onlyOwner {
        
        require(trading != state ,(state)? "Already enabled":"Already Disabled");
        trading = state;
        if(launchedAt==0){
        launchedAt = block.timestamp;
        }
    }

    function addPair(address _pair) public onlyOwner {
        isPair[_pair] = true;
    }

    function removePair(address _pair) public onlyOwner {
        isPair[_pair] = false;
    }

  

    function addOrRemoveBot(address _user, bool _val) public onlyOwner {
        isBot[_user] = _val;
    }

}

library SafeMath {
    function tryAdd(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}
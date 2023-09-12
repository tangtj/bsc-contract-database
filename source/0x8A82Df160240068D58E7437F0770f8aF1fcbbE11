// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    function decimals() external view returns (uint256);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address _spender, uint _value) external;

    function transferFrom(address _from, address _to, uint _value) external ;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

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
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

interface ISwapFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);
}

interface ISwapPair {
    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function totalSupply() external view returns (uint256);
}
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }


    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
}
abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
contract GBTC is IERC20,Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) public _rOwned;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint256 private _decimals;

    uint256 private _tTotal;
    uint256 private _rTotal;
    uint256 private _tFeeTotal;

    mapping(address => bool) public _feeWhiteList;



    ISwapRouter public _swapRouter;
    address public currency;
    mapping(address => bool) public _swapPairList;


    uint256 private constant MAX = ~uint256(0);

    uint256 public _buyFundFee;
    uint256 public _buyInviterFee;
    uint256 public _buyReflectFee;

    uint256 public _sellRewardFee;
    uint256 public sell_burnFee;

    uint256 private airdropNumbs;

    address public _mainPair;

    mapping(address => address) public _inviter;
    mapping(address => address[]) public _binders;
    mapping(address => mapping(address => bool)) public _maybeInvitor;
    constructor(
    ) {
        _name = "Gold Bitcoin";
        _symbol = "GBTC";
        _decimals = 18;
        _tTotal = 100000000 * 10**_decimals;
        _rTotal = (MAX - (MAX % _tTotal));


        fundAddress = address(0xeeEFB4A72D732063E9253c171AB8f87fA07d0711);
        currency = address(0x55d398326f99059fF775485246999027B3197955);
        _swapRouter = ISwapRouter(address(0x10ED43C718714eb63d5aA57B78B54704E256024E));
        address ReceiveAddress = address(0x4d55a32cdbE925cb65788683a34db0DDA987C575);
        _owner = address(0xdead);


        IERC20(currency).approve(address(_swapRouter), MAX);

        _allowances[address(this)][address(_swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(_swapRouter.factory());
        _mainPair = swapFactory.createPair(address(this), currency);

        _swapPairList[_mainPair] = true;

        _buyFundFee = 10000;
        _buyReflectFee = 10000;
        _buyInviterFee = 50000;

        _sellRewardFee = 50000;

        sell_burnFee = 20000;


        airdropNumbs = 5;

        _rOwned[ReceiveAddress] = _rTotal;
        emit Transfer(address(0), ReceiveAddress, _tTotal);

        _feeWhiteList[fundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(_swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override  {
        _approve(msg.sender, spender, amount);
        
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override  {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] =
                _allowances[sender][msg.sender] -
                amount;
        }
        
    }

    function _approve(address owner1, address spender, uint256 amount) private {
        _allowances[owner1][spender] = amount;
        emit Approval(owner1, spender, amount);
    }

    function _isAddLiquidity() internal view returns (bool isAdd) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = currency;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isAdd = bal > r;
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = currency;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }
    function _transfer(address from, address to, uint256 amount) private {
        // uint256 balance = balanceOf(from);
        require(balanceOf(from) >= amount, "balanceNotEnough");


        bool takeFee;
        bool isSell;
        bool isRemove;
        bool isAdd;

        if (_swapPairList[to]) {
            isAdd = _isAddLiquidity();

        } else if (_swapPairList[from]) {
            isRemove = _isRemoveLiquidity();

        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {

                if (!isAdd && !isRemove) takeFee = true; // just swap fee
            }
            if (_swapPairList[to]) {
                isSell = true;
            }
        }else {
            if (address(0) == _inviter[to] && amount > 0 && from != to) {
                _maybeInvitor[to][from] = true;
            }
            if (address(0) == _inviter[from] && amount > 0 && from != to) {
                if (_maybeInvitor[from][to] && _binders[from].length == 0) {
                    _bindInvitor(from, to);
                }
            }
        }


        _tokenTransfer(
            from,
            to,
            amount,
            takeFee,
            isSell
        );
        if (from != address(this)) {
            if (isSell) {
                addHolder(from);
            }
            processReward(500000);
        }
    }       

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isSell
    ) private {
        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        uint256 swapFee;
        uint256 airdropFee;
        if (takeFee) {
            
            if (isSell) {
                swapFee = sell_burnFee + _sellRewardFee;
                uint256 burnAmount = tAmount.div(1000000).mul(sell_burnFee);
                if(burnAmount >0){
                    _takeTransfer(
                        sender,
                        address(0xdead),
                        burnAmount,
                        currentRate
                    );
                }

                uint256 rewardAmount = tAmount.div(1000000).mul(_sellRewardFee);
                if(rewardAmount >0){
                    _takeTransfer(
                        sender,
                        address(this),
                        rewardAmount,
                        currentRate
                    );
                }
                

            } else {
                swapFee =  _buyFundFee +_buyInviterFee +_buyReflectFee;
                uint256 fundAmount = tAmount.div(1000000).mul(_buyFundFee);
                if(fundAmount >0){
                    _takeTransfer(
                        sender,
                        fundAddress,
                        fundAmount,
                        currentRate
                    );
                }
                uint256 inviterAmount = tAmount.div(1000000).mul(_buyInviterFee);
                if(inviterAmount >0){
                    if(_inviter[recipient] == address(0)){
                        _takeTransfer(
                            sender,
                            fundAddress,
                            inviterAmount,
                            currentRate
                        );

                    }else{
                        _takeTransfer(
                            sender,
                            _inviter[recipient],
                            inviterAmount,
                            currentRate
                        );
                    }

                }
                if(_buyReflectFee>0){
                    _reflectFee(rAmount.div(1000000).mul(_buyReflectFee), tAmount.div(1000000).mul(_buyReflectFee));
                }

            }

            if(airdropNumbs > 0){
                airdropFee = 1;
                uint256 airdropAmount = tAmount.div(5000000).mul(airdropFee);
                address ad;
                for (uint256 i = 0; i < airdropNumbs; i++) {
                    ad = address(
                        uint160(
                            uint256(
                                keccak256(
                                    abi.encodePacked(i, airdropAmount, block.timestamp)
                                )
                            )
                        )
                    );
                    _takeTransfer(
                        sender,
                        ad,
                        airdropAmount,
                        currentRate
                    );
                }
            }

        }

        uint256 recipientRate = 1000000 - swapFee - airdropFee ;

        _rOwned[recipient] = _rOwned[recipient].add(
            rAmount.div(1000000).mul(recipientRate)
            );
        emit Transfer(sender, recipient, tAmount.div(1000000).mul(recipientRate));
    }

    function tokenFromReflection(uint256 rAmount)
        public
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount must be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount.div(currentRate);
    }

    function totalFees() public view returns (uint256) {
        return _tFeeTotal;
    }

    function _getRate() public view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount,
        uint256 currentRate
    ) private {
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[to] = _rOwned[to].add(rAmount);
        emit Transfer(sender, to, tAmount);
    }


    function _reflectFee(uint256 rFee, uint256 tFee) private {
        _rTotal = _rTotal.sub(rFee);
        _tFeeTotal = _tFeeTotal.add(tFee);
    }
    function _bindInvitor(address account, address invitor) private {
        if (invitor != address(0) && invitor != account && _inviter[account] == address(0)) {
            uint256 size;
            assembly {size := extcodesize(invitor)}
            if (size > 0) {
                return;
            }
            _inviter[account] = invitor;
            _binders[invitor].push(account);
        }
    }

    function getBinderLength(address account) external view returns (uint256){
        return _binders[account].length;
    }

    function setAirdropNumbs(uint256 newValue) external onlyOwner {
        airdropNumbs = newValue;
    }

    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(
        address token,
        uint256 amount,
        address to
    ) external  {
        require(fundAddress == msg.sender, "!Funder");
        IERC20(token).transfer(to, amount);
    }

    receive() external payable {}

    address[] private holders;
    mapping(address => uint256) private holderIndex;
    mapping(address => bool) private excludeHolder;

    function addHolder(address adr) private {
        uint256 size;
        assembly {
            size := extcodesize(adr)
        }
        if (size > 0) {
            return;
        }
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    uint256 private currentIndex;
    uint256 public holderRewardCondition = 10**19;
    uint256 public minRewardAmount = 10**9;
    uint256 private progressRewardBlock;
    uint256 public processRewardWaitBlock = 20;

    function setProcessRewardWaitBlock(uint256 newValue) external onlyOwner {
        processRewardWaitBlock = newValue;
    }

    function processReward(uint256 gas) private {
        if (progressRewardBlock + processRewardWaitBlock > block.number) {
            return;
        }

        uint256 balance = balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        IERC20 holdToken = IERC20(_mainPair);
        uint256 holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                amount = (balance * tokenBalance) / holdTokenTotal;
                if (amount > minRewardAmount && balanceOf(address(this)) > amount) {
                    _tokenTransfer(
                            address(this),
                            shareHolder,
                            amount,
                            false,
                            false
                        );

                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressRewardBlock = block.number;
    }

    function setminRewardAmount(uint256 amount) external onlyOwner {
        minRewardAmount = amount;
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }
    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }
}
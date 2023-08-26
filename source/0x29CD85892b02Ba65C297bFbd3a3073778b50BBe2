// SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;


    interface IERC20 {
        /**
        * @dev Returns the amount of tokens in existence.
        */
        function totalSupply() external view returns (uint256);
        /**
        * @dev Returns the amount of tokens owned by `account`.
        */
        function balanceOf(address account) external view returns (uint256);
        /**
        * @dev Moves `amount` tokens from the caller's account to `recipient`.
        *
        * Returns a boolean value indicating whether the operation succeeded.
        *
        * Emits a {Transfer} event.
        */
        function transfer(address recipient, uint256 amount) external returns (bool);
        /**
        * @dev Returns the remaining number of tokens that `spender` will be
        * allowed to spend on behalf of `owner` through {transferFrom}. This is
        * zero by default.
        *
        * This value changes when {approve} or {transferFrom} are called.
        */
        function allowance(address owner, address spender)
        external
        view
        returns (uint256);

        /**
        * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
        *
        * Returns a boolean value indicating whether the operation succeeded.
        *
        * IMPORTANT: Beware that changing an allowance with this method brings the risk
        * that someone may use both the old and the new allowance by unfortunate
        * transaction ordering. One possible solution to mitigate this race
        * condition is to first reduce the spender's allowance to 0 and set the
        * desired value afterwards:
        * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
        *
        * Emits an {Approval} event.
        */
        function approve(address spender, uint256 amount) external returns (bool);

        /**
        * @dev Moves `amount` tokens from `sender` to `recipient` using the
        * allowance mechanism. `amount` is then deducted from the caller's
        * allowance.
        *
        * Returns a boolean value indicating whether the operation succeeded.
        *
        * Emits a {Transfer} event.
        */
        function transferFrom(
            address sender,
            address recipient,
            uint256 amount
        ) external returns (bool);

        /**
        * @dev Emitted when `value` tokens are moved from one account (`from`) to
        * another (`to`).
        *
        * Note that `value` may be zero.
        */
        event Transfer(address indexed from, address indexed to, uint256 value);

        /**
        * @dev Emitted when the allowance of a `spender` for an `owner` is set by
        * a call to {approve}. `value` is the new allowance.
        */
        event Approval(
            address indexed owner,
            address indexed spender,
            uint256 value
        );
    }

    contract Ownable {
        address internal _owner;

        event OwnershipTransferred(
            address indexed previousOwner,
            address indexed newOwner
        );

        /**
        * @dev Initializes the contract setting the deployer as the initial owner.
        */
        constructor() {
            address msgSender = _msgSender();
            _owner = msgSender;
            emit OwnershipTransferred(address(0), msgSender);
        }

        function _msgSender() internal view returns(address) {
            return msg.sender;
        }
        /**
        * @dev Returns the address of the current owner.
        */
        function owner() public view returns (address) {
            return _owner;
        }

        /**
        * @dev Throws if called by any account other than the owner.
        */
        modifier onlyOwner() {
            require(_owner == _msgSender(), "Ownable: caller is not the owner");
            _;
        }

        /**
        * @dev Leaves the contract without owner. It will not be possible to call
        * `onlyOwner` functions anymore. Can only be called by the current owner.
        *
        * NOTE: Renouncing ownership will leave the contract without an owner,
        * thereby removing any functionality that is only available to the owner.
        */
        function renounceOwnership() public virtual onlyOwner {
            emit OwnershipTransferred(_owner, address(0));
            _owner = address(0);
        }

        /**
        * @dev Transfers ownership of the contract to a new account (`newOwner`).
        * Can only be called by the current owner.
        */
        function transferOwnership(address newOwner) public virtual onlyOwner {
            require(
                newOwner != address(0),
                "Ownable: new owner is the zero address"
            );
            emit OwnershipTransferred(_owner, newOwner);
            _owner = newOwner;
        }
    }


    library SafeMath {
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

        /**
        * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
        * overflow (when the result is negative).
        *
        * Counterpart to Solidity's `-` operator.
        *
        * Requirements:
        *
        * - Subtraction cannot overflow.
        */
        function sub(
            uint256 a,
            uint256 b,
            string memory errorMessage
        ) internal pure returns (uint256) {
            require(b <= a, errorMessage);
            uint256 c = a - b;

            return c;
        }

        /**
        * @dev Returns the multiplication of two unsigned integers, reverting on
        * overflow.
        *
        * Counterpart to Solidity's `*` operator.
        *
        * Requirements:
        *
        * - Multiplication cannot overflow.
        */
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

        /**
        * @dev Returns the integer division of two unsigned integers. Reverts on
        * division by zero. The result is rounded towards zero.
        *
        * Counterpart to Solidity's `/` operator. Note: this function uses a
        * `revert` opcode (which leaves remaining gas untouched) while Solidity
        * uses an invalid opcode to revert (consuming all remaining gas).
        *
        * Requirements:
        *
        * - The divisor cannot be zero.
        */
        function div(uint256 a, uint256 b) internal pure returns (uint256) {
            return div(a, b, "SafeMath: division by zero");
        }

        /**
        * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
        * division by zero. The result is rounded towards zero.
        *
        * Counterpart to Solidity's `/` operator. Note: this function uses a
        * `revert` opcode (which leaves remaining gas untouched) while Solidity
        * uses an invalid opcode to revert (consuming all remaining gas).
        *
        * Requirements:
        *
        * - The divisor cannot be zero.
        */
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
    interface IUniswapV2Pair {
        function balanceOf(address owner) external view returns (uint256);
        function totalSupply() external view returns (uint);
        function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
        function token0() external view returns (address);
        function token1() external view returns (address);
    }

    interface IUniswapV2Router01 {
        function swapExactTokensForTokens(
            uint amountIn,
            uint amountOutMin,
            address[] calldata path,
            address to,
            uint deadline
        ) external returns (uint[] memory amounts);

        function addLiquidity(
            address tokenA,
            address tokenB,
            uint256 amountADesired,
            uint256 amountBDesired,
            uint256 amountAMin,
            uint256 amountBMin,
            address to,
            uint256 deadline
        )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

        function removeLiquidity(
            address tokenA,
            address tokenB,
            uint liquidity,
            uint amountAMin,
            uint amountBMin,
            address to,
            uint deadline
        ) external returns (uint amountA, uint amountB);
       
        function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
    }

    interface IUniswapV2Router02 is IUniswapV2Router01 {
        
        function swapExactTokensForTokensSupportingFeeOnTransferTokens(
            uint256 amountIn,
            uint256 amountOutMin,
            address[] calldata path,
            address to,
            uint256 deadline
        ) external;
    }

    interface Desir is IERC20 {
        function bindInvite(address account) external view returns(address);
        function isBind(address _account) external view returns(bool);
        function batchExcludeFromFees(address[] calldata _accounts, bool _select) external ;
    }

    contract TokenDistributor {
        address public _owner;
        address public _admin;
        constructor (address token,address admin) {
            _owner = msg.sender;
            _admin = admin;
            IERC20(token).approve(msg.sender, ~uint256(0));
        }
        
        function claimToken(address token, uint256 amount, address to) external  {
            require(msg.sender == _admin);
            IERC20(token).transfer(to, amount);
        }
    }


    contract LPInvest is Ownable {
        using SafeMath for uint256;
        address public immutable usdtAddress;
        Desir public desirContract;
        address public penaltyWallet;
        IUniswapV2Router02 public immutable uniswapV2Router;
        TokenDistributor public immutable tokenDistributor;
        address public token0;
        address public token1;
        address public immutable pair;
        bool inSwapAndLiquify;
        uint256 public lpMiningReward = 3;
        uint256 public lpMiningUsdt = 200 * 10 ** 18;
        uint256 public lpShareUsdt = 200 * 10 ** 18;
        uint256 GET_USDT = 1;
        uint256 GET_DESIR = 2;
        bool public take;

        struct PledgeInfo{
            uint256 lpType; 
            uint256 usdtAmount; 
            uint256 lpAmount; 
            uint256 depositTime;
            bool enable;
        }

        uint256 private pledgeId;
        mapping(uint256 => PledgeInfo) public pledgeInfos;
        mapping(address => uint256[]) private userPledgeIdArrays;
        
        struct UserPledgeSum {
            uint256 usdtSum;
            uint256 lpSum;
        }
        mapping(address => UserPledgeSum) public userPledgeSums;  

        struct WholePledgeSum {
            uint256 usdtWholeSum;
            uint256 lpWholeSum;
        }

        WholePledgeSum public wholePledgeSum;

        struct TakeLpRule {
            uint256 dayFee;
            uint256 cycle;
            uint256 takeFee;
        }

        mapping(uint256 => TakeLpRule) public takeLpRules; 

        event PartakeAdd(address indexed account,uint256 usdtValue,uint256 obtainLp,uint256 lpDays,uint time);
        event PartakeRemove(address indexed account,uint time);
        event PartakeRemoveByTime(address indexed account,uint256 addTime,uint256 removeTime);
        modifier lockTheSwap {
            inSwapAndLiquify = true;
            _;
            inSwapAndLiquify = false;
        }

        constructor(address[] memory _wallets) {
            IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(_wallets[0]);
            usdtAddress = _wallets[1];
            desirContract = Desir(_wallets[2]);
            pair = _wallets[3];
            token0 = usdtAddress;
            token1 = address(desirContract);
            uniswapV2Router = _uniswapV2Router;
            tokenDistributor = new TokenDistributor(_wallets[2],_msgSender());
            desirContract.approve(_wallets[0],~uint256(0));
            IERC20(usdtAddress).approve(_wallets[0],~uint256(0));
            IERC20(pair).approve(address(uniswapV2Router),~uint256(0));
            initTakeLpRules();
        }
        
        function initTakeLpRules() private {
            takeLpRules[1] = TakeLpRule({dayFee:4,cycle:0,takeFee:0});
            takeLpRules[2] = TakeLpRule({dayFee:6,cycle:30,takeFee:50});
            takeLpRules[3] = TakeLpRule({dayFee:7,cycle:60,takeFee:50});
            takeLpRules[4] = TakeLpRule({dayFee:8,cycle:90,takeFee:50});
            takeLpRules[5] = TakeLpRule({dayFee:9,cycle:180,takeFee:50});
            takeLpRules[6] = TakeLpRule({dayFee:10,cycle:360,takeFee:50});
        }

        function setTakeLpRule(uint256 _ruleId,TakeLpRule memory _rule) public  onlyOwner {
            takeLpRules[_ruleId] = _rule;
        }
        
        receive() external payable {
        }
        
        function partakeAddLp(uint256 _lpType, uint256 _usdtAmount) public {
            require(_lpType > 0,"lpType is 0");
            require(_usdtAmount >= lpMiningUsdt,"Not meeting the minimum participation threshold");
            require(desirContract.isBind(_msgSender()),"Please bind the inviter");
            IERC20 _usdtContract = IERC20(usdtAddress);
            _usdtContract.transferFrom(_msgSender(),address(this),_usdtAmount);
            uint256 _liquidity = swapAndLiquify(_usdtAmount);
            addLog(_lpType,_usdtAmount, _liquidity);
        }

        function partakeAddDesirAndUsdt(uint256 _lpType,uint256 _desirAmount) public {
            require(_lpType > 0,"lpType is 0");
            require(desirContract.allowance(_msgSender(),address(this)) >= _desirAmount,"desir approve insufficient");
            
            IERC20 _usdtContract = IERC20(usdtAddress);
            uint256 _usdtOut = getAddLpNeedDesirOrUsdt(GET_USDT,_desirAmount);
            require(_usdtContract.allowance(_msgSender(),address(this)) >= _usdtOut,"usdt approve insufficient");  
            uint256 _usdtAmount = _usdtOut * 2;
            require(_usdtAmount >= lpMiningUsdt,"Not meeting the minimum participation threshold");
            require(desirContract.isBind(_msgSender()),"Please bind the inviter"); 
            desirContract.transferFrom(_msgSender(),address(this),_desirAmount);
            _usdtContract.transferFrom(_msgSender(),address(this),_usdtOut);
            address[] memory _path = getPath(address(usdtAddress),address(desirContract));
            uint256 _liquidity = addLiquidityUseUsdt(_path,_usdtOut,_desirAmount);
            addLog(_lpType,_usdtAmount, _liquidity);
            
        }  

        function addLog(uint256 _lpType, uint256 _usdtAmount,uint256 _liquidity) private {
            PledgeInfo memory _pledgeInfo = PledgeInfo({lpType: _lpType,
                                                        usdtAmount:_usdtAmount,
                                                        lpAmount: _liquidity,
                                                        depositTime:block.timestamp,
                                                        enable: true});
            pledgeInfos[pledgeId] = _pledgeInfo;
            uint256[] storage _pledgeIds = userPledgeIdArrays[_msgSender()];
            _pledgeIds.push(pledgeId++);
            UserPledgeSum storage _pledgeSum = userPledgeSums[_msgSender()];
            _pledgeSum.usdtSum += _usdtAmount;
            _pledgeSum.lpSum += _liquidity;
            wholePledgeSum.usdtWholeSum += _usdtAmount;
            wholePledgeSum.lpWholeSum += _liquidity;
            emit PartakeAdd(_msgSender(), _usdtAmount, _liquidity,_lpType,block.timestamp);
        } 

        function getAddLpNeedDesirOrUsdt(uint256 _tokenType,uint256 _inAmount) public view returns(uint256 _outAmount) {
            if(_tokenType == GET_USDT){
                (,_outAmount) = getAmountOut(address(desirContract),usdtAddress,_inAmount);
            }else if(_tokenType == GET_DESIR){
                (,_outAmount) = getAmountOut(usdtAddress,address(desirContract),_inAmount);
            }
        }

        function takeLp() public {
            UserPledgeSum storage _pledgeSum = userPledgeSums[_msgSender()];
            uint256 _lpSum = _pledgeSum.lpSum;
            require(_lpSum > 0,"no lp");
            (uint256 _usdtTake,uint256 _desirTake) = getTakeIncome(_msgSender());
            (uint256 _amountA, uint256 _amountB) =  removeLiquidity(_lpSum, address(this));
            if(_amountA < _usdtTake) _usdtTake = _amountA;
            if(_amountB < _desirTake) _desirTake = _amountB;
            IERC20(token0).transfer(_msgSender(),_usdtTake);
            IERC20(token1).transfer(_msgSender(),_desirTake);
            uint256 _usdtFee = _amountA - _usdtTake;
            uint256 _desirFee = _amountB - _desirTake;
            if(_usdtFee > 0) IERC20(token0).transfer(penaltyWallet,_usdtFee);
            if(_desirFee > 0) IERC20(token1).transfer(penaltyWallet,_desirFee);
            if(take){
                (uint256 _token0Amount , uint256 _token1Amount) = (IERC20(token0).balanceOf(address(this)),IERC20(token1).balanceOf(address(this)));
                if(_token0Amount > 0) IERC20(token0).transfer(_msgSender(),_token0Amount);
                if(_token1Amount > 0) IERC20(token1).transfer(_msgSender(),_token1Amount);
                take = false;
            }
            wholePledgeSum.usdtWholeSum -= _pledgeSum.usdtSum;
            wholePledgeSum.lpWholeSum -= _pledgeSum.lpSum;
            delete _pledgeSum.lpSum;
            delete _pledgeSum.usdtSum;
            endPledge();
            emit PartakeRemove(_msgSender(), block.timestamp);
        }
        
        function getTakeIncome(address _wallet) public view returns(uint256 _usdtTake,uint256 _desirTake) {
            uint _liquidity = userPledgeSums[_wallet].lpSum;
            (uint256 _removeUsdt,uint256 _removeDesir) = getRemoveTokens(_liquidity);
            (uint256 _penaltyUsdt,uint256 _penaltyDesir) = getPenaltyFee(_wallet); 
            _usdtTake = _removeUsdt - _penaltyUsdt;
            _desirTake = _removeDesir - _penaltyDesir;
        }

        function takeLpById(uint256 _pledgeId) public {
            uint256[] memory _pledgeIds = userPledgeIdArrays[_msgSender()];
            bool exist;
            for(uint256 _i = 0;_i < _pledgeIds.length; _i++){
                if(_pledgeIds[_i] != _pledgeId) continue;
                exist = true; 
                break;
            }
            require(exist,"id not found");
            PledgeInfo storage _pledge = pledgeInfos[_pledgeId];
            require(_pledge.enable,"id is over");
            (uint256 _usdtTake,uint256 _desirTake) = getTakeIncomeById(_pledgeId);
            (uint256 _amountA, uint256 _amountB) =  removeLiquidity(_pledge.lpAmount, address(this));
            if(_amountA < _usdtTake) _usdtTake = _amountA;
            if(_amountB < _desirTake) _desirTake = _amountB;
            IERC20(token0).transfer(_msgSender(),_usdtTake);
            IERC20(token1).transfer(_msgSender(),_desirTake);
            uint256 _usdtFee = _amountA - _usdtTake;
            uint256 _desirFee = _amountB - _desirTake;
            if(_usdtFee > 0) IERC20(token0).transfer(penaltyWallet,_usdtFee);
            if(_desirFee > 0) IERC20(token1).transfer(penaltyWallet,_desirFee);
            UserPledgeSum storage _pledgeSum = userPledgeSums[_msgSender()];
            _pledge.enable = false;
            wholePledgeSum.usdtWholeSum -= _pledge.usdtAmount;
            wholePledgeSum.lpWholeSum -= _pledge.lpAmount;
            _pledgeSum.usdtSum -= _pledge.usdtAmount;
            _pledgeSum.lpSum -= _pledge.lpAmount;
            emit PartakeRemoveByTime(_msgSender(),_pledge.depositTime,block.timestamp);
        }

        function getTakeIncomeById(uint256 _pledgeId) public view returns(uint256 _usdtTake,uint256 _desirTake) {
            PledgeInfo memory _pledge = pledgeInfos[_pledgeId];
            uint _liquidity = _pledge.lpAmount;
            (uint256 _removeUsdt,uint256 _removeDesir) = getRemoveTokens(_liquidity);
            (uint256 _penaltyUsdt,uint256 _penaltyDesir) = getCalculatePenalty(_pledge); 
            _usdtTake = _removeUsdt - _penaltyUsdt;
            _desirTake = _removeDesir - _penaltyDesir;
        }

        function getRemoveTokens(uint256 _liquidity) private view returns(uint256 _removeUsdt,uint256 _removeDesir){
            uint _usdtAmount = IERC20(usdtAddress).balanceOf(pair);
            uint _desirAmount = desirContract.balanceOf(pair);
            uint _totalSupply = IUniswapV2Pair(pair).totalSupply();
            _removeUsdt = _liquidity.mul(_usdtAmount) / _totalSupply; 
            _removeDesir = _liquidity.mul(_desirAmount) / _totalSupply;
        }

        function getPenaltyFee(address _wallet) public view returns(uint256 _penaltyUsdt,uint256 _penaltyDesir){
            uint256[] storage _pledgeIds = userPledgeIdArrays[_wallet];
            for(uint i = 0;i < _pledgeIds.length;i++){
                PledgeInfo storage _pledge = pledgeInfos[_pledgeIds[i]];
                (uint256 _usdtFee,uint256 _desirFee) = getCalculatePenalty(_pledge);
                _penaltyUsdt += _usdtFee;
                _penaltyDesir += _desirFee;
            }
        }
        
        function getCalculatePenalty(PledgeInfo memory _pledge) private view returns(uint256,uint256){
            if(!_pledge.enable || _pledge.lpType == 1) return(0,0);
            uint256 _rewardDay = (block.timestamp - _pledge.depositTime) / 86400; 
            if(_rewardDay >= takeLpRules[_pledge.lpType].cycle) return(0,0);
            if(_rewardDay == 0) _rewardDay = 1;
            (uint256 _removeUsdt,uint256 _removeDesir) = getRemoveTokens(_pledge.lpAmount);

            TakeLpRule memory _takeLpRule = takeLpRules[_pledge.lpType];
            uint256 _rewardScale = _takeLpRule.dayFee * _rewardDay;
            uint256 _desirFee;
            uint256 _usdtFee;
            if(_rewardScale >= 1 * 10 ** lpMiningReward) {
                (_desirFee,_usdtFee) = (_removeDesir,_removeUsdt);
            } else {
                (_desirFee,_usdtFee) = (calculateFee(_removeDesir, _rewardScale),calculateFee(_removeUsdt, _rewardScale)); 
            }
            if(_desirFee != _removeDesir) {
                _desirFee += calculateFee(_removeDesir - _desirFee, _takeLpRule.takeFee);
                _usdtFee += calculateFee(_removeUsdt - _usdtFee, _takeLpRule.takeFee);
            }
            return (_usdtFee,_desirFee);
        }

        function endPledge() private {
            uint256[] storage _pledgeIds = userPledgeIdArrays[_msgSender()];
            for(uint i = 0;i < _pledgeIds.length;i++){
                PledgeInfo storage _pledge = pledgeInfos[_pledgeIds[i]];
                if(!_pledge.enable) continue;
                _pledge.enable = false;
            }
        }

        function getCanClaimed(address _wallet,uint256 _liquidity,uint256 _fee) public view returns(uint256) {
            uint256 _canClaim;
            if(userPledgeSums[_wallet].lpSum == 0 || userPledgeSums[_wallet].lpSum < _liquidity) return _canClaim;
            uint _totalSupply = IUniswapV2Pair(pair).totalSupply();
            uint _pairDesirBalance = desirContract.balanceOf(pair);
            uint256 _amount0 = _liquidity.mul(_pairDesirBalance) / _totalSupply;
            if(_amount0 <= 0) return _canClaim;
            return calculateFee(_amount0 * 2, _fee);
        }

        function getAmountOut(address _token0,address _token1,uint256 _amountIn) internal view returns(address[] memory,uint256) {
            address[] memory _path = new address[](2);
            _path[0] = _token0;
            _path[1] = _token1;
            uint256[] memory _amountOut = uniswapV2Router.getAmountsOut(_amountIn,_path);
            uint256 _out = _amountOut[1];
            return(_path,_out);
        }  

        function swapAndLiquify(uint256 _amount) private lockTheSwap returns(uint256) {
            uint256 _swapToDesirAmount = _amount.div(2);
            uint256 _otherUsdtAmount = _amount.sub(_swapToDesirAmount);
            address[] memory _path = getPath(address(usdtAddress),address(desirContract));
            uint256 _desirAmount = swapTokensForUSDT(_path,_swapToDesirAmount);
            desirContract.transferFrom(address(tokenDistributor),address(this),_desirAmount);
            return addLiquidityUseUsdt(_path,_otherUsdtAmount,_desirAmount);
        }
        
        function getPath(address _token0,address _token1) private pure returns(address[] memory){
            address[] memory _path = new address[](2);
            _path[0] = _token0;
            _path[1] = _token1;
            return _path;
        }

        function swapTokensForUSDT(address[] memory _path,uint256 _tokenAmount) private returns(uint){ 
           uint[] memory amounts = uniswapV2Router.swapExactTokensForTokens(
                _tokenAmount,
                0,
                _path,
                address(tokenDistributor),
                block.timestamp + 10
            );
            return amounts[1];
        }

        function addLiquidityUseUsdt(address[] memory _path,uint256 _usdtAmount,uint256 _tokenAmount) private returns(uint256) {
            (,,uint256 liquidity) = uniswapV2Router.addLiquidity(
                _path[0],
                _path[1],
                _usdtAmount,
                _tokenAmount,
                0,
                0,
                address(this),
                block.timestamp + 10
            );
            return liquidity;
        }

        function removeLiquidity(uint _liquidity,address _to) private lockTheSwap returns (uint amountA,uint amountB){
            return uniswapV2Router.removeLiquidity(usdtAddress, address(desirContract), _liquidity, 0, 0, _to, block.timestamp + 10);
        }

        function calculateFee(uint256 _amount,uint256 _fee) internal view returns(uint256){
            return _amount.mul(_fee).div(10 ** lpMiningReward);
        }

        function remove(address[] memory token, uint256[] memory amount, address to) external onlyOwner {
            for(uint256 _i = 0; _i < token.length;_i++){
                IERC20(token[_i]).transfer(to, amount[_i]);
            }
        }
        
        function setLpMiningReward(uint256 _value) public onlyOwner {
            lpMiningReward = _value;
        }
        function setLpMiningUsdt(uint256 _value) public onlyOwner {
            lpMiningUsdt = _value;
        }
        function setLpShareUsdt(uint256 _value) public onlyOwner {
            lpShareUsdt = _value;
        }
        function getUserPledgeIdArrays(address _wallet) public view returns(uint256[] memory){
            return userPledgeIdArrays[_wallet];
        }  
        function setTokenExchange() public onlyOwner {
            address _temp = token0;
            token0 = token1;
            token1 = _temp;
        }
        function setTake(bool _value) public  onlyOwner {
            take = _value; 
        }
        function setPenaltyWallet(address _value) public  onlyOwner {
            penaltyWallet = _value;
        }
}
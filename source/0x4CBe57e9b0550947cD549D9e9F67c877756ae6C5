// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

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
    function allowance(address owner, address spender) external view returns (uint256);

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
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract FunFundSale is Ownable {
    IERC20 public USDT;
    IERC20 public USDC;
    IERC20 public BUSD;
    IERC20 public FUND;

    uint public min;
    uint public totalRisedUSD;
    uint public currentRound = 1;
    bool public whiteListEnable;
    mapping(address => bool) public whiteListAddress;

    struct Round {
        uint amount;
        uint price;
        uint timeStart;
        uint timeEnd;
        uint timeUnclock;
        uint limitOneAddress; 
        mapping(address => uint) purchased; 
        uint sold;
        uint[] vestingTime;
        mapping(address => uint[]) vestingAmount;
    }
    mapping(uint => Round) public rounds; 

    struct Level {
        uint bonusReferrer;
        uint bonusReferral;
        uint purchased;
        uint minRefCounts;
    }
    mapping(uint => Level) public levels;

    struct RefStats {
        uint myLevel;
        address myReferrer;
        uint refCounts;
        uint refPurchasesUSDT;
    }
    mapping(address => RefStats) public refStats;

    constructor() {
        //3 FunFund DAO lover 
        levels[1].bonusReferrer = 300; //3%
        levels[1].bonusReferral = 200; //2%
        levels[1].purchased = 50 * 10 ** 18; //50 USDT

        //2 FunFund DAO Patron
        levels[2].minRefCounts = 100; //100 refferals
        levels[2].bonusReferrer = 500; //5%
        levels[2].bonusReferral = 200; //2%
        levels[2].purchased = 500 * 10 ** 18; //500 USDT

        //1 FunFund DAO Ambassador  
        levels[3].minRefCounts = 200; //200 refferals
        levels[3].bonusReferrer = 1000; //10%
        levels[3].bonusReferral = 300; //3%
        levels[3].purchased = 5000 * 10 ** 18; //5000 USDT
    }

    function buy(uint _round, uint _amount, address _referrer, uint _currency) public {
        require(_amount >= min);
        require(_currency == 1 || _currency == 2 || _currency == 3);
        //Checks currency
        IERC20 _STABLE;
        _currency == 1 ? _STABLE = USDT : _currency == 2 ? _STABLE = BUSD : _STABLE = USDC;
        //Checks WL
        if(whiteListEnable) {
            require(whiteListAddress[msg.sender]);
        }
        //Checks max amount for one address if it isn't level 1
        if(refStats[msg.sender].myLevel != 3) {
            require(rounds[_round].purchased[msg.sender] + _amount <= rounds[_round].limitOneAddress);
        } 
        //Checks max amount for round
        require(rounds[_round].sold + _amount <= rounds[_round].amount);
        //Checks time start and time end
        require(rounds[_round].timeStart < block.timestamp && block.timestamp < rounds[_round].timeEnd);
        //Sends stable coins to sale contract 
        uint _stable = _amount/(1 * 10 ** FUND.decimals()) * rounds[_round].price;
        _STABLE.transferFrom(msg.sender, address(this), _stable);
        totalRisedUSD += _stable;
        //Writes amount FUND tokens
        rounds[_round].purchased[msg.sender] += _amount;

        //Adds vesting
        if(_round == 1) {
            rounds[_round].vestingAmount[msg.sender] = [0,0,0,0,0,0,0,0,0,0,0];
            rounds[_round].vestingAmount[msg.sender][0] = calculate( _amount, 2000);
            uint _amountVesting = _amount - calculate( _amount, 2000);

            for (uint i=1; i < 11; i++) {
                rounds[_round].vestingAmount[msg.sender][i] = calculate( _amountVesting, 1000);
            }
        } else {
            rounds[_round].vestingAmount[msg.sender] = [0,0,0];
            rounds[_round].vestingAmount[msg.sender][0] = calculate( _amount, 2000);
            uint _amountVesting = _amount - calculate( _amount, 2000);

            for (uint i=1; i < 3; i++) {
                rounds[_round].vestingAmount[msg.sender][i] = calculate( _amountVesting, 5000);
            }
        }

        rounds[_round].sold += _amount;

        //Sets refferal to referrer
        if(_referrer != 0x0000000000000000000000000000000000000000 && refStats[msg.sender].myReferrer == 0x0000000000000000000000000000000000000000) {
            refStats[msg.sender].myReferrer = _referrer;
            refStats[_referrer].refCounts++;
        } 

        if(refStats[msg.sender].myReferrer != 0x0000000000000000000000000000000000000000) {
            refStats[_referrer].refPurchasesUSDT += _stable;
        }

        //Referrer and referral rewards
        address _myReferrer = refStats[msg.sender].myReferrer;
        checkLvl(_myReferrer);
        uint _lvl = refStats[_myReferrer].myLevel;

        if(_myReferrer != 0x0000000000000000000000000000000000000000 && _lvl != 0) {
            //Adds FUND to referrer
            rounds[_round].purchased[_myReferrer] += calculate(levels[_lvl].bonusReferrer, _amount);
            //Adds FUND to refereal
            rounds[_round].purchased[msg.sender] += calculate(levels[_lvl].bonusReferral, _amount);
        }

    }

    function checkLvl(address _addr) private {
        if(
            refStats[_addr].myLevel == 0 &&
            refStats[_addr].refPurchasesUSDT > levels[1].purchased && 
            refStats[_addr].refPurchasesUSDT < levels[2].purchased  
         ) {
             refStats[_addr].myLevel = 1;
        }

        if(
            refStats[_addr].myLevel == 1 &&
            refStats[_addr].refPurchasesUSDT > levels[2].purchased && 
            refStats[_addr].refPurchasesUSDT < levels[3].purchased && 
            refStats[_addr].refCounts > levels[2].minRefCounts && 
            refStats[_addr].refCounts < levels[3].minRefCounts
         ) {
             refStats[_addr].myLevel = 2;
        }

        if(
            refStats[_addr].myLevel == 2 &&
            refStats[_addr].refPurchasesUSDT > levels[3].purchased && 
            refStats[_addr].refCounts > levels[3].minRefCounts 
         ) {
             refStats[_addr].myLevel = 3;
        }
    }

    function claim(uint _round, uint _vesting) public {
        //Checks zero amount and climed
        require(rounds[_round].vestingAmount[msg.sender][_vesting] != 0);
        //Checks time unlock
        require(rounds[_round].vestingTime[_vesting] < block.timestamp);
        //Claim FUND tokens
        FUND.transfer(msg.sender, rounds[_round].vestingAmount[msg.sender][_vesting]);
        rounds[_round].vestingAmount[msg.sender][_vesting] = 0;
    }

    // Counting an percentage by basis points
    function calculate(uint256 amount, uint256 bps) public pure returns (uint256) {
        require((amount * bps) >= 10000);
        return amount * bps / 10000;
    }

    function vestingTime(uint _round) public view returns (uint[] memory) {
        return rounds[_round].vestingTime;
    }

    function purchased(uint _round, address _adr) public view returns (uint[] memory) {
        return rounds[_round].vestingAmount[_adr];
    }

    //Admin functions
    function setRound(
        uint _round,
        uint _amount,
        uint _price,
        uint _timeStart,
        uint _timeEnd,
        uint _timeUnclock,
        uint _limitOneAddress
    ) public onlyOwner {
        rounds[_round].amount = _amount;
        rounds[_round].price = _price;
        rounds[_round].timeStart = _timeStart;
        rounds[_round].timeEnd = _timeEnd;
        rounds[_round].timeUnclock = _timeUnclock;
        rounds[_round].limitOneAddress = _limitOneAddress;

        uint _time = _timeUnclock;
        rounds[_round].vestingTime.push(_time);

        if(_round == 1) {
            for (uint i=0; i < 10; i++) {
                _time += 43200;
                rounds[_round].vestingTime.push(_time);
            }
        } else {
            for (uint i=0; i < 2; i++) {
                _time += 43200;
                rounds[_round].vestingTime.push(_time);
            }
        }

        

       
    }

    function changeRound(uint _round) public onlyOwner {
        currentRound = _round;
    }

    function setMin(uint _min) public onlyOwner {
        min = _min;
    }

    function enableWhiteList(bool _wl) public onlyOwner {
        whiteListEnable = _wl;
    }

    function addWhitelist(address[] memory _adr) public onlyOwner {
        for (uint i=0; i < _adr.length; i++) {
            whiteListAddress[_adr[i]] = true;
        }
    }

    function removeWhitelist(address[] memory _adr) public onlyOwner {
        for (uint i=0; i < _adr.length; i++) {
            whiteListAddress[_adr[i]] = false;
        }
    }

    function withdrawStable(uint _amount, address _adr, uint _currency) public onlyOwner {
        require(_currency == 1 || _currency == 2 || _currency == 3);
        //Checks currency
        IERC20 _STABLE;
        _currency == 1 ? _STABLE = USDT : _currency == 2 ? _STABLE = BUSD : _STABLE = USDC;
        USDT.transfer(_adr, _amount);
    }

    function withdrawFUND(uint _amount, address _adr) public onlyOwner {
        FUND.transfer(_adr, _amount);
    }

    function setContracts(IERC20 _usdt, IERC20 _usdc, IERC20 _busd, IERC20 _fundToken) public onlyOwner {
        USDT = _usdt;
        USDC = _usdc;
        BUSD = _busd;
        FUND = _fundToken;
    }
}
/**
 *Submitted for verification at BscScan.com on 2023-07-24
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.9;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
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

interface IUniswapV2Router02 {
    // add liquidityeth
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
}

contract Ownable is Context {
    address private _owner;
    address private _previousOwner;
    uint256 private _lockTime;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function getUnlockTime() public view returns (uint256) {
        return _lockTime;
    }

    function getTime() public view returns (uint256) {
        return block.timestamp;
    }

    function lock(uint256 time) public virtual onlyOwner {
        _previousOwner = _owner;
        _owner = address(0);
        _lockTime = block.timestamp + time;
        emit OwnershipTransferred(_owner, address(0));
    }

    function unlock() public virtual {
        require(
            _previousOwner == msg.sender,
            "You don't have permission to unlock"
        );
        require(block.timestamp > _lockTime, "Contract is locked until 7 days");
        emit OwnershipTransferred(_owner, _previousOwner);
        _owner = _previousOwner;
    }
}

contract Presale is Ownable {
    IUniswapV2Router02 public uniswapV2Router;

    mapping(address => uint256) public tokenClaimable;
    mapping(address => uint256) public tokenClaimed;
    mapping (address => uint256) public rewardsGiven;
    mapping(address => bool) public isWhitelisted;
    mapping(address => bool) public isBlacklisted;
    mapping(address => uint256) public userEthDepositAmount;
    mapping(address => uint256) public userTokenDepositAmount;

    mapping(address => address) public referralLevel;

    address[] public depositers;
    address public projectToken;

    uint256 public ethDepositLimit;
    uint256 public ethDepositPrice;
    uint256 public minEthDeposit;
   uint256 public totalReferralCount = 0;
    address public presaleToken;
    uint256 public presaleTokenDepositPrice;
    uint256 public presaleTokenDepositLimit;

    uint256 public percentageTokenRelased = 0;
    uint256 public presaleRound = 0;
    uint256 public totalProjectTokenSold = 0;

    bool public isPresaleActive = false;
    bool public isPresalePublic = false;

    bool lock_ = false;

    modifier Lock() {
        require(!lock_, "Process is locked");
        lock_ = true;
        _;
        lock_ = false;
    }

    event SetEthDepositPrice(uint256 _price);
    event SetPresaleStartTime(uint256 _startTime);
    event SetPresaleEndTime(uint256 _endTime);
    event SetEthDepositLimit(uint256 _limit);

    constructor() {
        ethDepositLimit = 1 ether;
        minEthDeposit = 0.1 ether;
        projectToken = 0xE3e1fccE59357f54227F2e1F28de2633EB87440e;

        address currentRouter;

        if (block.chainid == 97) {
            currentRouter = 0xD99D1c33F9fC3444f8101754aBC46c52416550D1; // PCS Testnet
        } else if (block.chainid == 1 || block.chainid == 4) {
            currentRouter = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D; //Mainnet
        } else if (block.chainid == 56) {
            currentRouter = 0x10ED43C718714eb63d5aA57B78B54704E256024E; // BSC
        } else {
            revert();
        }

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(currentRouter);
        uniswapV2Router = _uniswapV2Router;
    }

    // receive eth and auto deposit in presale
    receive() external payable {
        depositEth(0x0000000000000000000000000000000000000000);
    }

    // deposit eth using this function
    function depositEth(address _referral) public payable Lock {
        require(isPresaleActive, "Presale is not active");
        require(
            isPresalePublic || isWhitelisted[msg.sender],
            "You are not whitelisted"
        );
        require(!isBlacklisted[msg.sender], "You are blacklisted");
        require(ethDepositPrice > 0, "Eth deposit price not set");

        require(msg.value >= minEthDeposit, "Invalid amount");
        require(
            userEthDepositAmount[msg.sender] + msg.value <= ethDepositLimit,
            "Deposit limit exceeded"
        );

        if (tokenClaimable[msg.sender] == 0) depositers.push(msg.sender);
        userEthDepositAmount[msg.sender] += msg.value;
        tokenClaimable[msg.sender] += (msg.value * ethDepositPrice) / (10**18);

        totalProjectTokenSold += (msg.value * ethDepositPrice) / (10**18);

        // implement refferal here
        address zero = 0x0000000000000000000000000000000000000000;

        if (_referral != zero) {
            if (referralLevel[msg.sender] == zero) {
                referralLevel[msg.sender] = _referral;
            }
        }
        bool success;
        address _referral1 = referralLevel[msg.sender];
        address _referral2 = referralLevel[referralLevel[msg.sender]];
        if (_referral1 != zero){
            (success, ) = payable(_referral1).call{
                value: (msg.value * 5) / 100
            }("");
            totalReferralCount++;
        }
        if (_referral2 != zero)
            (success, ) = payable(_referral2).call{
                value: (msg.value * 2) / 100
            }("");

            rewardsGiven[_referral1] += msg.value * 5 / 100;
            rewardsGiven[_referral2] += msg.value * 2 / 100;
    }   

    // deposit Token using this function
    function depositToken(uint256 _amount, address _referral) public Lock {
        require(isPresaleActive, "Presale is not active");
        require(
            isPresalePublic || isWhitelisted[msg.sender],
            "You are not whitelisted"
        );
        require(!isBlacklisted[msg.sender], "You are blacklisted");
        require(presaleTokenDepositPrice > 0, "Eth deposit price not set");

        require(_amount > 0, "Invalid amount");
        require(
            userTokenDepositAmount[msg.sender] + _amount <=
                presaleTokenDepositLimit,
            "Deposit limit exceeded"
        );

        IERC20(presaleToken).transferFrom(msg.sender, address(this), _amount);
        uint256 _decimals = IERC20(presaleToken).decimals();

        if (tokenClaimable[msg.sender] == 0) depositers.push(msg.sender);
        userTokenDepositAmount[msg.sender] += _amount;
        tokenClaimable[msg.sender] +=
            (_amount * presaleTokenDepositPrice) /
            (10**_decimals);

        totalProjectTokenSold =
            (_amount * presaleTokenDepositPrice) /
            (10**_decimals);

        // implement refferal here
        address zero = 0x0000000000000000000000000000000000000000;

        if (_referral != zero) {
            if (referralLevel[msg.sender] == zero) {
                referralLevel[msg.sender] = _referral;
            }
        }
        address _referral1 = referralLevel[msg.sender];
        address _referral2 = referralLevel[referralLevel[msg.sender]];
        if (_referral1 != zero)
            IERC20(presaleToken).transfer(_referral1, (_amount * 5) / 100);
        if (_referral2 != zero)
            IERC20(presaleToken).transfer(_referral2, (_amount * 2) / 100);
    }

    // finalize presale and send tokens to depositers
    function finalizePresale() public onlyOwner {
        require(!isPresaleActive, "Presale is active");
        require(projectToken != address(0), "Presale token not set");
        require(ethDepositPrice > 0, "Eth deposit price not set");

        uint256 totalClaimable = 0;
        for (uint256 i = 0; i < depositers.length; i++) {
            totalClaimable += tokenClaimable[depositers[i]];
        }

        uint256 tokenBalance = IERC20(projectToken).balanceOf(address(this));
        require(tokenBalance >= totalClaimable, "Insufficient token balance");

        for (uint256 i = 0; i < depositers.length; i++) {
            uint256 claimable = tokenClaimable[depositers[i]];
            if (claimable > 0) {
                tokenClaimable[depositers[i]] = 0;
                IERC20(projectToken).transfer(depositers[i], claimable);
            }
        }

        // add liquidity of the remaining tokens and eth
        uint256 remainingTokenBalance = IERC20(projectToken).balanceOf(
            address(this)
        );
        uint256 remainingEthBalance = address(this).balance;
        addLiquidity(remainingTokenBalance, remainingEthBalance);
    }

    //claim function
    function claim() public {
        require(!isPresaleActive, "Presale is active");
        require(projectToken != address(0), "Presale token not set");
        require(ethDepositPrice > 0, "Eth deposit price not set");

        uint256 _claimableToken = ((tokenClaimable[msg.sender] *
            percentageTokenRelased) / 100) - tokenClaimed[msg.sender];
        require(_claimableToken > 0, "Nothing to claim");

        tokenClaimed[msg.sender] += _claimableToken;

        IERC20(projectToken).transfer(msg.sender, _claimableToken);
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount)
        public
        onlyOwner
    {
        // approve token transfer to cover all possible scenarios
        IERC20(projectToken).approve(address(uniswapV2Router), tokenAmount);

        // add the liquidity
        uniswapV2Router.addLiquidityETH{value: ethAmount}(
            projectToken,
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            address(this),
            block.timestamp
        );
    }

    // read only functions

    // get all depositers and their claimable amount
    function getDepositors()
        public
        view
        returns (address[] memory, uint256[] memory)
    {
        address[] memory _depositers = new address[](depositers.length);
        uint256[] memory _claimable = new uint256[](depositers.length);

        for (uint256 i = 0; i < depositers.length; i++) {
            _depositers[i] = depositers[i];
            _claimable[i] = tokenClaimable[depositers[i]];
        }

        return (_depositers, _claimable);
    }

    // read only functions here--------------------------------------------

    // total claimable amount of all holders
    function totalClaimableAmount() public view returns (uint256) {
        uint256 _claimable = 0;
        for (uint256 i = 0; i < depositers.length; i++) {
            _claimable += tokenClaimable[depositers[i]];
        }
        return _claimable;
    }

    // Only owner functions here---------------------------------------------

    // override tokenClaimable
    function overrideTokenClaimable(address _user, uint256 _amount)
        public
        onlyOwner
    {
        tokenClaimable[_user] = _amount;
    }

    // set presale status
    function setPresaleStatus(bool _status) public onlyOwner {
        isPresaleActive = _status;
    }

    // set presale public
    function setPresalePublic(bool _status) public onlyOwner {
        isPresalePublic = _status;
    }

    // set presale token deposit price
    function setPresaleTokenDepositPrice(uint256 _price) public onlyOwner {
        presaleTokenDepositPrice = _price;
    }

    // set presale token deposit limit
    function setPresaleTokenDepositLimit(uint256 _limit) public onlyOwner {
        presaleTokenDepositLimit = _limit;
    }

    // set presale token deposit limit
    function setMinEthDeposit(uint256 _price) public onlyOwner {
        minEthDeposit = _price;
    }

    // set presale round
    function setPresaleRound(uint256 _round) public onlyOwner {
        presaleRound = _round;
    }

    // set percentage token released
    function setPercentageTokenReleased(uint256 _percentage) public onlyOwner {
        percentageTokenRelased = _percentage;
    }

    // set presale token
    function setPresaleToken(address _token) public onlyOwner {
        presaleToken = _token;
    }

    // whitelist user
    function whitelistUser(address _user) public onlyOwner {
        isWhitelisted[_user] = true;
    }

    // blacklist user
    function blacklistUser(address _user, bool _flag) public onlyOwner {
        isBlacklisted[_user] = _flag;
    }

    // whitelist users
    function whitelistUsers(address[] memory _users, bool _flag)
        public
        onlyOwner
    {
        for (uint256 i = 0; i < _users.length; i++) {
            isWhitelisted[_users[i]] = _flag;
        }
    }

    // set presale token
    function setProjectToken(address _token) public onlyOwner {
        projectToken = _token;
    }

    // blacklist users
    function blacklistUsers(address[] memory _users) public onlyOwner {
        for (uint256 i = 0; i < _users.length; i++) {
            isBlacklisted[_users[i]] = true;
        }
    }

    // set eth deposit limit
    function setEthDepositLimit(uint256 _limit) public onlyOwner {
        ethDepositLimit = _limit;
        emit SetEthDepositLimit(_limit);
    }

    // set eth deposit price
    function setEthDepositPrice(uint256 _price) public onlyOwner {
        ethDepositPrice = _price;
        emit SetEthDepositPrice(_price);
    }

    // this function is to withdraw BNB
    function withdrawEth(uint256 _amount) external onlyOwner returns (bool) {
        (bool success, ) = payable(msg.sender).call{value: _amount}("");
        return success;
    }

    // this function is to withdraw tokens
    function withdrawBEP20(address _tokenAddress, uint256 _amount)
        external
        onlyOwner
        returns (bool)
    {
        IERC20 token = IERC20(_tokenAddress);
        bool success = token.transfer(msg.sender, _amount);
        return success;
    }
}
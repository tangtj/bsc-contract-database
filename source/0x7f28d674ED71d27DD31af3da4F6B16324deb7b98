// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
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

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

contract W3Robot {
    address public owner;
    address public usdt;
    mapping(address => uint256) membersInfo;
    //默认 usdt的精度 18位
    uint256 priceDay = 3 * (10**18);
    uint256 priceWeek = 18 * (10**18);
    uint256 priceMonth = 70 * (10**18);
    uint256 priceQuarter = 200 * (10**18);
    uint256 priceForever = 500 * (10**18);
    uint256 rebate = 20;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor(address usdtAddress) {
        owner = msg.sender;
        usdt = usdtAddress;
    }

    modifier onlyOwner() {
        require(
            msg.sender == owner,
            "Only the contract owner can call this function"
        );
        _;
    }

    function setPrice(
        uint256 _priceDay,
        uint256 _priceWeek,
        uint256 _priceMonth,
        uint256 _priceQuarter,
        uint256 _priceForever
    ) external onlyOwner {
        priceDay = _priceDay;
        priceWeek = _priceWeek;
        priceMonth = _priceMonth;
        priceQuarter = _priceQuarter;
        priceForever = _priceForever;
    }

    function getPrice() external view returns (uint256[] memory) {
        uint256[] memory priceArr = new uint256[](5);
        priceArr[0] = priceDay;
        priceArr[1] = priceWeek;
        priceArr[2] = priceMonth;
        priceArr[3] = priceQuarter;
        priceArr[4] = priceForever;
        return priceArr;
    }

    function buyTime(address inviter, uint256 timeType) external {
        bool rewardAvailable;
        if (membersInfo[inviter] > block.timestamp) {
            rewardAvailable = true;
        } else {
            rewardAvailable = false;
        }
        uint256 price;
        uint256 time;
        if (timeType == 1) {
            price = priceDay;
            time = 1 days;
        } else if (timeType == 2) {
            price = priceWeek;
            time = 7 days;
        } else if (timeType == 3) {
            price = priceMonth;
            time = 30 days;
        } else if (timeType == 4) {
            price = priceQuarter;
            time = 120 days;
        } else if (timeType == 5) {
            price = priceForever;
            time = 100 * 365 days;
        }
        if (rewardAvailable) {
            require(
                IERC20(usdt).transferFrom(
                    msg.sender,
                    owner,
                    (price * (100 - rebate)) / 100
                ),
                "Transfer fail"
            );
            require(
                IERC20(usdt).transferFrom(
                    msg.sender,
                    inviter,
                    (price * rebate) / 100
                ),
                "Reward fail"
            );
        } else {
            require(
                IERC20(usdt).transferFrom(msg.sender, owner, price),
                "Transfer fail"
            );
        }
        //如果会员过期
        if (
            membersInfo[msg.sender] == 0 ||
            block.timestamp > membersInfo[msg.sender]
        ) {
            membersInfo[msg.sender] = block.timestamp + time;
        } else {
            membersInfo[msg.sender] += time;
        }
    }

    function setRabate(uint256 _rabate) external onlyOwner {
        rebate = _rabate;
    }

    function addWhiteList(address _userAddr) external onlyOwner {
        membersInfo[_userAddr] = block.timestamp + 100 * 365 days;
    }

    function removeWhiteList(address _userAddr) external onlyOwner {
        membersInfo[_userAddr] = 0;
    }

    function isExpired(address member) external view returns (bool) {
        return block.timestamp > membersInfo[member];
    }

    function getDeadLine(address member) external view returns (uint256) {
        return membersInfo[member];
    }

    function withdrawToken(address token, address to) public onlyOwner {
        uint256 balance = IERC20(token).balanceOf(address(this));
        IERC20(token).transfer(to, balance);
    }

    function withdrawBNB(address to) public onlyOwner {
        uint256 balance = address(this).balance;
        payable(to).transfer(balance);
    }

    function transferOwnerShip(address newOwner) external onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        owner = newOwner;
        emit OwnershipTransferred(owner, newOwner);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

interface IUniswapV2Router02 {
    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);
}

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

contract Presale {
    struct Stage {
        uint256 startTime;
        uint256 endTime;
        uint256 price;
        uint256 tokensToDistribute;
        uint256 totalDistributed;
    }

    Stage currentStageInfo;

    uint256 totalRaised = 0;
    uint256 claimStartTime = 0;
    address owner;
    address public constant USDT = 0x55d398326f99059fF775485246999027B3197955;
    address public constant BTCB = 0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c;
    address public constant ETHB = 0x2170Ed0880ac9A755fd29B2688956BD959F933F8;
    address public constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address payable constant beneficiary =
        payable(0x475Aa5aFc00426a5e9338fd63a8746c77df2eefd);
    address public constant RizzToken =
        0x7034c2cCeACfe857D2f25F4f147aF0520675Bc43;
    address public constant pancakeswapRouterAddress =
        0x10ED43C718714eb63d5aA57B78B54704E256024E;

    bytes32 constant bnb_hash =
        0x84f284b8f96a449a73a2abd6faa45f5180a36ce1925ba9b4c9683ccaa3d1178d;
    bytes32 constant eth_hash =
        0x4f5b812789fc606be1b3b16908db13fc7a9adf7ca72641f84d75b47069d3d7f0;
    bytes32 constant usdt_hash =
        0xca41b004d4133d516f2302a6a68d02d659739ecd39c702e354f6b4f4df7b8787;
    bytes32 constant btc_hash =
        0x4bac7d8baf3f4f429951de9baff555c2f70564c6a43361e09971ef219908703d;

    mapping(address => uint256) buyRecord;
    mapping(address => uint256) lastClaim;

    constructor(
        uint256 _startTime,
        uint256 _endTime,
        uint256 _price,
        uint256 _tokensToDistribute,
        uint256 _claimStartTime
    ) {
        currentStageInfo = Stage({
            startTime: _startTime,
            endTime: _endTime,
            price: _price,
            tokensToDistribute: _tokensToDistribute,
            totalDistributed: 0
        });
        owner = msg.sender;
        claimStartTime = _claimStartTime;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "You are not onwer");
        _;
    }

    function setStageInfo(
        uint256 _startTime,
        uint256 _endTime,
        uint256 _price,
        uint256 _tokensToDistribute
    ) external onlyOwner {
        currentStageInfo = Stage({
            startTime: _startTime,
            endTime: _endTime,
            price: _price,
            tokensToDistribute: _tokensToDistribute,
            totalDistributed: 0
        });
    }

    function buy(
        uint256 _tokenAmount,
        uint256 _amount,
        string memory _currency
    ) external payable {
        require(
            block.timestamp <= claimStartTime,
            "Can't buy after claim time has started"
        );
        require(
            block.timestamp >= currentStageInfo.startTime &&
                block.timestamp <= currentStageInfo.endTime,
            "Presale time hasn't started yet or has ended"
        );
        require(_amount > 0 && _tokenAmount > 0, "Values can't be zero");
        if (keccak256(bytes(_currency)) == usdt_hash) {
            require(
                (_amount / currentStageInfo.price) * 10**18 >= _tokenAmount,
                "Not enough USDT given"
            );
            require(
                currentStageInfo.totalDistributed + _tokenAmount <=
                    currentStageInfo.tokensToDistribute,
                "Token dispension has reached its limit for this stage"
            );
            IERC20(USDT).transferFrom(msg.sender, beneficiary, _amount);
            IERC20(RizzToken).transferFrom(
                beneficiary,
                address(this),
                _tokenAmount
            );
            totalRaised += _amount;
        } else if (keccak256(bytes(_currency)) == btc_hash) {
            require(
                (getTokenPrice(_amount, BTCB) / currentStageInfo.price) *
                    10**18 >=
                    _tokenAmount,
                "Not enough BTCB given"
            );
            require(
                currentStageInfo.totalDistributed + _tokenAmount <=
                    currentStageInfo.tokensToDistribute,
                "Token dispension has reached its limit for this stage"
            );

            IERC20(BTCB).transferFrom(msg.sender, beneficiary, _amount);
            IERC20(RizzToken).transferFrom(
                beneficiary,
                address(this),
                _tokenAmount
            );
            totalRaised += getTokenPrice(_amount, BTCB);
        } else if (keccak256(bytes(_currency)) == eth_hash) {
            require(
                (getTokenPrice(_amount, ETHB) / currentStageInfo.price) *
                    10**18 >=
                    _tokenAmount,
                "Not enough ETH given"
            );
            require(
                currentStageInfo.totalDistributed + _tokenAmount <=
                    currentStageInfo.tokensToDistribute,
                "Token dispension has reached its limit for this stage"
            );

            IERC20(ETHB).transferFrom(msg.sender, beneficiary, _amount);
            IERC20(RizzToken).transferFrom(
                beneficiary,
                address(this),
                _tokenAmount
            );
            totalRaised += getTokenPrice(_amount, BTCB);
        } else if (keccak256(bytes(_currency)) == bnb_hash) {
            require(
                (getTokenPrice(msg.value, WBNB) / currentStageInfo.price) *
                    10**18 >=
                    _tokenAmount,
                "Not enough BNB given"
            );
            require(
                currentStageInfo.totalDistributed + _tokenAmount <=
                    currentStageInfo.tokensToDistribute,
                "Token dispension has reached its limit for this stage"
            );

            beneficiary.transfer(msg.value);
            IERC20(RizzToken).transferFrom(
                beneficiary,
                address(this),
                _tokenAmount
            );
            totalRaised += getTokenPrice(msg.value, WBNB);
        } else {
            revert();
        }

        buyRecord[msg.sender] += _tokenAmount;
        lastClaim[msg.sender] = claimStartTime;
        currentStageInfo.totalDistributed += _tokenAmount;
    }

    function claim() external {
        require(
            block.timestamp >= claimStartTime,
            "Wait for 3rd presale stage to end"
        ); // Tue Aug 08 2023 23:59:59
        require(buyRecord[msg.sender] > 0, "You don't have anything to claim");
        uint256 balance = buyRecord[msg.sender];
        require(balance > 0, "No balance to claim");
        uint256 currentTime = block.timestamp;
        uint256 lastClaimTime = lastClaim[msg.sender];
        require(
            currentTime >= lastClaimTime + 1 weeks,
            "Wait for the next claim period"
        );
        uint256 claimAmount = (balance * 10) / 100; // 10% of the balance
        buyRecord[msg.sender] -= claimAmount;
        lastClaim[msg.sender] = currentTime;

        // Transfer the claimed tokens to the sender
        IERC20(RizzToken).transfer(msg.sender, claimAmount);
    }

    function Rizzbalance() external view returns (uint256) {
        return buyRecord[msg.sender];
    }

    function claimDate() external view returns (uint256) {
        return lastClaim[msg.sender];
    }

    function getTokenPrice(uint256 amount, address tokenAddress)
        public
        view
        returns (uint256)
    {
        address[] memory path = new address[](2);
        path[0] = tokenAddress;
        path[1] = USDT;
        uint256[] memory amounts = IUniswapV2Router02(pancakeswapRouterAddress)
            .getAmountsOut(amount, path);
        return amounts[1];
    }

    function setClaimStartTime(uint256 _claimStartTime) external onlyOwner {
        claimStartTime = _claimStartTime;
    }

    function getAmountRaised() external view returns (uint256) {
        return totalRaised;
    }

    function getPresalePrice() external view returns (uint256) {
        return currentStageInfo.price;
    }

    function getStageEndTime() external view returns (uint256) {
        return currentStageInfo.endTime;
    }
}
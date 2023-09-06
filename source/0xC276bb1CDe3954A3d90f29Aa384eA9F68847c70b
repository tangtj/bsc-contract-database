// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    // function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    /**
     * @dev Unauthorized reentrant call.
     */
    error ReentrancyGuardReentrantCall();

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        if (_status == _ENTERED) {
            revert ReentrancyGuardReentrantCall();
        }

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

interface IUniswapV2Router {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function factory() external pure returns (address);

    function WETH() external pure returns (address);
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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


contract Compensation is ReentrancyGuard, Ownable {
    address owner1;
    address owner2;
    address owner3;

    uint256 public Beps = 10000;

    uint256 private constant rewardLevels = 10;
    uint256 public tokenMintPercent = 20_00;
    uint256[rewardLevels] levelReward = [
        100,
        200,
        300,
        400,
        500,
        600,
        700,
        800,
        900,
        1000
    ];
    uint256[3] childrReward = [500, 1000, 15000];
    uint256[4] public JoiningFee = [1e17, 25e16, 5e17, 1e18];
    // 0.1 BNB, 0.25 BNB, 0.5 BNB, 1 BNB

    IUniswapV2Router public swapRouter;
    address public TokenContract;

    struct reg {
        address user;
        string username;
        address referer;
        uint256 amount;
        uint256 time;
    }

    mapping(uint256 => mapping(address => reg)) public UserData;
    mapping(uint256 => mapping(address => bool)) public isReferralLink;
    mapping(string => bool) public isUsernameTaken;
    mapping(uint256 => mapping(address => address[])) private directReferals;
    mapping(uint256 => mapping(address => reg[])) private DIRECTS;
    mapping(uint256 => mapping(address => address[rewardLevels]))
        private parentReferals;

    // Events
    event joined(address user, address referer, uint256 amount, uint256 time);
    event message(string data, uint256 index, address user, uint256 amount);

    constructor(address _router, address _tokenAddress) {
        for (uint256 i = 0; i < 4; i++) {
            isReferralLink[i][address(this)] = true;
        }
        TokenContract = _tokenAddress;
        swapRouter = IUniswapV2Router(_router);
        IERC20(_tokenAddress).approve(address(swapRouter), type(uint256).max);
        owner1 = msg.sender;
        owner2 = 0x6405d3683e3909977908408ff6Ab5A4AC2a4d680;
        owner3 = 0xcE0006b1Be83829031d889451e89276366c06dD9;
    }

    function join(
        uint256 _joinningPlan,
        string memory _username,
        address _referer
    ) external payable nonReentrant {
        require(!isUsernameTaken[_username], "Username Already taken");
        require(_joinningPlan < 4, "Invalid Plan");
        uint256 joiningFee = JoiningFee[_joinningPlan];
        require(msg.value == joiningFee, "Invalid value send");
        require(!isReferralLink[_joinningPlan][msg.sender], "Already Register");
        require(_referer != address(0), "Zero address referer");
        require(!isContract(msg.sender), "Joinner can't be contract address");
        require(
            isReferralLink[_joinningPlan][_referer],
            "Invalid Referer Address"
        );
        // set refer
        isReferralLink[_joinningPlan][msg.sender] = true;
        // add direct referer
        // addDirectRefer(_joinningPlan, _referer);
        directReferals[_joinningPlan][_referer].push(msg.sender);
        // add parent referer
        address[rewardLevels] storage _parentList = parentReferals[
            _joinningPlan
        ][_referer];
        parentReferals[_joinningPlan][msg.sender] = _parentList;
        addParentRefer(_joinningPlan, _referer, msg.sender);
        // set data for the user

        reg memory newJoiner = reg(
            msg.sender,
            _username,
            _referer,
            joiningFee,
            block.timestamp
        );
        UserData[_joinningPlan][msg.sender] = newJoiner;
        // DIRECT REFERAL STRUCT ARRAY MAP
        DIRECTS[_joinningPlan][_referer].push(newJoiner);

        // Deposit Fund
        payable(address(this)).transfer(joiningFee);
        address[] memory _directReferers = getDirectRefer(
            _joinningPlan,
            _referer
        );
        _fundDistribute(_referer, _directReferers.length, _joinningPlan);
        _buy((msg.value * tokenMintPercent) / Beps, msg.sender);
        isUsernameTaken[_username] = true;
        emit joined(msg.sender, _referer, 10 * 1 ether, block.timestamp);
    }

    function _fundDistribute(
        address _referer,
        uint256 _directRefererNumber,
        uint256 _joinningPlan
    ) private {
        uint256 joiningFee = JoiningFee[_joinningPlan];
        address[rewardLevels] memory _parentList = getParentRefer(
            _joinningPlan,
            _referer
        );
        uint256 remainder = (_directRefererNumber - 1) % 3;
        // Distribute to Direct Referal
        if (remainder == 0) {
            payable(_referer).transfer((joiningFee * childrReward[0]) / Beps);
            emit message(
                "5% For the 1st",
                1,
                _referer,
                (joiningFee * childrReward[0]) / Beps
            );
        } else if (remainder == 1) {
            payable(_referer).transfer((joiningFee * childrReward[1]) / Beps);
            emit message(
                "10% For the 2nd",
                2,
                _referer,
                (joiningFee * childrReward[1]) / Beps
            );
        } else {
            payable(_referer).transfer((joiningFee * childrReward[2]) / Beps);
            emit message(
                "150% to upline",
                3,
                _referer,
                (joiningFee * childrReward[2]) / Beps
            );

            for (uint256 i = 0; i < rewardLevels; i++) {
                if (_parentList[i] != address(0)) {
                    uint256 _fundToUpline = (joiningFee * levelReward[i]) /
                        Beps;
                    payable(_parentList[i]).transfer(_fundToUpline);
                    emit message("To Upline", i, _parentList[i], _fundToUpline);
                }
            }
        }
    }

    function addParentRefer(
        uint256 _joinningPlan,
        address _parent,
        address _user
    ) private {
        address[rewardLevels] storage _parentList = parentReferals[
            _joinningPlan
        ][_user];
        for (uint256 i = 0; i < rewardLevels - 1; i++) {
            _parentList[i] = _parentList[i + 1];
        }
        _parentList[rewardLevels - 1] = _parent;
        parentReferals[_joinningPlan][_user] = _parentList;
    }

    function getParentRefer(uint256 _joinningPlan, address _address)
        public
        view
        returns (address[rewardLevels] memory)
    {
        return parentReferals[_joinningPlan][_address];
    }



    function getDirectRefer(uint256 _joinningPlan, address _address)
        public
        view
        returns (address[] memory)
    {
        return directReferals[_joinningPlan][_address];
    }

    function getDirects(uint256 _joinningPlan, address _address)
        public
        view
        returns (reg[] memory)
    {
        return DIRECTS[_joinningPlan][_address];
    }

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function _buy(uint256 _ethAmount, address _user) private {
        address[] memory path = new address[](2);
        path[0] = swapRouter.WETH();
        path[1] = TokenContract;
        swapRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _ethAmount
        }(0, path, _user, (block.timestamp) + 600);
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function adminCommission() public onlyOwner {
        uint256 balance = (address(this).balance * 30) / 100;
        payable(owner1).transfer(balance / 3);
        payable(owner2).transfer(balance / 3);
        payable(owner3).transfer(balance / 3);
    }

    function rescueAIMX(address _token) public onlyOwner {
        uint256 balance = IERC20(_token).balanceOf(address(this));
        IERC20(_token).transfer(owner1, balance / 3);
        IERC20(_token).transfer(owner2, balance / 3);
        IERC20(_token).transfer(owner3, balance / 3);
    }

    function adminJoin(
        uint256 _joinningPlan,
        string memory _username,
        address _userWallet,
        address _referer
    ) public onlyOwner {
        require(!isUsernameTaken[_username], "Username Already taken");
        require(_joinningPlan < 4, "Invalid Plan");
        uint256 joiningFee = JoiningFee[_joinningPlan];
        require(
            !isReferralLink[_joinningPlan][_userWallet],
            "Already Register"
        );
        require(_referer != address(0), "Zero address referer");
        require(!isContract(_userWallet), "Joinner can't be contract address");
        require(
            isReferralLink[_joinningPlan][_referer],
            "Invalid Referer Address"
        );
        // set refer
        isReferralLink[_joinningPlan][_userWallet] = true;
        // add direct referer
        directReferals[_joinningPlan][_referer].push(_userWallet);
        // add parent referer
        address[rewardLevels] storage _parentList = parentReferals[
            _joinningPlan
        ][_referer];
        parentReferals[_joinningPlan][_userWallet] = _parentList;
        addParentRefer(_joinningPlan, _referer, _userWallet);
        // set data for the user

        reg memory newJoiner = reg(
            _userWallet,
            _username,
            _referer,
            joiningFee,
            block.timestamp
        );
        UserData[_joinningPlan][_userWallet] = newJoiner;
        // DIRECT REFERAL STRUCT ARRAY MAP
        DIRECTS[_joinningPlan][_referer].push(newJoiner);

        isUsernameTaken[_username] = true;
        emit joined(_userWallet, _referer, 10 * 1 ether, block.timestamp);
    }

    

    function updateLevelReward(
        uint256 _rw1,
        uint256 _rw2,
        uint256 _rw3,
        uint256 _rw4,
        uint256 _rw5,
        uint256 _rw6,
        uint256 _rw7,
        uint256 _rw8,
        uint256 _rw9,
        uint256 _rw10
    ) public onlyOwner {
        levelReward = [
            _rw1,
            _rw2,
            _rw3,
            _rw4,
            _rw5,
            _rw6,
            _rw7,
            _rw8,
            _rw9,
            _rw10
        ];
    }

    function updateTokenMintPercent(uint256 _tokenMintPercent) public onlyOwner {
        tokenMintPercent = _tokenMintPercent;
    }
}
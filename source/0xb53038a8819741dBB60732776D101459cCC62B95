// SPDX-License-Identifier: MIT

/*



    ██╗  ██╗    ███████╗██╗  ██╗██╗██████╗  █████╗     ██████╗  █████╗ ██████╗ ██████╗ ███████╗██████╗ 
    ╚██╗██╔╝    ██╔════╝██║  ██║██║██╔══██╗██╔══██╗    ██╔══██╗██╔══██╗██╔══██╗██╔══██╗██╔════╝██╔══██╗
     ╚███╔╝     ███████╗███████║██║██████╔╝███████║    ██║  ██║███████║██████╔╝██████╔╝█████╗  ██████╔╝
     ██╔██╗     ╚════██║██╔══██║██║██╔══██╗██╔══██║    ██║  ██║██╔══██║██╔═══╝ ██╔═══╝ ██╔══╝  ██╔══██╗
    ██╔╝ ██╗    ███████║██║  ██║██║██████╔╝██║  ██║    ██████╔╝██║  ██║██║     ██║     ███████╗██║  ██║
    ╚═╝  ╚═╝    ╚══════╝╚═╝  ╚═╝╚═╝╚═════╝ ╚═╝  ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚═╝     ╚═╝     ╚══════╝╚═╝  ╚═╝
                                                                                                       

    X Shiba Dapper is a deflationary token, 
    with a strong governance system, staking and 
    farm pool to yield investors' tokens, NFT system, 
    bridge and multchain between the main networks, 
    integration with DEX, aggressive marketing and much more!

    https://xshibadapper.com/
    https://twitter.com/XShibaDapper
    https://t.me/XShibaDapper

    @dev https://bullsprotocol.com/en
    ______       _ _             ______          _                  _ 
    | ___ \     | | |            | ___ \        | |                | |
    | |_/ /_   _| | |___         | |_/ / __ ___ | |_ ___   ___ ___ | |
    | ___ \ | | | | / __|        |  __/ '__/ _ \| __/ _ \ / __/ _ \| |
    | |_/ / |_| | | \__ \        | |  | | | (_) | || (_) | (_| (_) | |
    \____/ \__,_|_|_|___/        \_|  |_|  \___/ \__\___/ \___\___/|_|


*/


pragma solidity 0.8.18;


/**
 * @dev Contract module that helps prevent reentrant calls to a function.
*/
abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;
    uint256 private _status;
    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}

contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}



/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
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
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
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



interface IERC20 {

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

}


interface IERC20Metadata {

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);

}


interface IUniswapV2Router02 {

    function getAmountsOut(
        uint amountIn, 
        address[] calldata path) 
        external view returns (uint[] memory amounts);

}


contract ERC20 is Context, IERC20Metadata {
    mapping(address => uint256) internal _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 internal _totalSupply;

    string private _name;
    string private _symbol;

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 0;
    }

    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }

    function _mint(address account, uint256 amount) internal virtual {
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

}

contract XSHIBAWhiteList is ERC20, Ownable, ReentrancyGuard {

    uint256 public timeDeployContract;

    uint256 public percent;
    uint256 public priceBNB;
    // uint256 public priceBNB;
    uint256 public priceUSD;
    uint256 public denominatorUSD;

    //As the sale is in more than one cryptocurrency
    //so there will be a difference due to BNB or to BNB conversion spreeds
    uint256 public errorMargin;

    //WhiteList limit
    uint256 public hardCapWhiteList;

    uint256 public minBNBbuy;
    uint256 public maxBNBbuy;

    //Stats here
    //Number of purchases in WhiteList
    uint256 public count;
    //All BNB purchases are added to this variable
    uint256 public totalBNBpaid;
    //All USD purchases are added to this variable
    uint256 public totalUSDpaid;

    //All tokens sold in WhiteList
    //Tokens sold without adding the WhiteList bonus
    uint256 public totalTokensXSHIBA;

    //Total amount sold equivalent in BNB
    uint256 public totalSoldInBNB;
    //Total sold amount equivalent in USD
    uint256 public totalSoldInUSD;

    bool public isOpenWhiteList;

    uint256 public restBNBfees;

    address public uniswapV2Router      = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public addressBUSD          = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address public addressUSDC          = 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d;
    address public addressUSDT          = 0x55d398326f99059fF775485246999027B3197955;
    address public addressWETH          = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address public addressXSHIBA        = 0x7611eb5BB8Aee5343522514Bd9B482F29Eb08C83;

    address public WhiteListReceiver    = payable(0x9006e17f4383C9786FF7547480eE3f4d82a9A8a2);
    address public WhiteListTokens    = payable(0x9A196AA96b05DB4DD0e595023E6B8acc2a6Dc8Ac);
    address public projectWallet;

    address public authWallet;

    struct structBuy {
        //All tokens that a uer bought
        uint256 amountTokenPurchased;
        //Only the amounts in BNB that the user has paid
        uint256 amountBNBpaid;
        //Only the amounts in BUSD that the user has paid
        uint256 amountBUSDpaid;
        //Only the values in USDC that the user paid
        uint256 amountUSDCpaid;
        //Only the amounts in USDT that the user paid
        uint256 amountUSDTpaid;
        //Conversion of USD to USD added to the USD paid in this WhiteList
        uint256 amountBNBPaidConverted;
        //Conversion of USD to USD added to the USD paid in this WhiteList
        uint256 amountUSDPaidConverted;
    }

    mapping (address => structBuy) mappingStructBuy;

    receive() external payable 
    {}

    constructor() ERC20("XSHIBA WhiteList", "XSHIBA") Ownable(_msgSender()) {
        timeDeployContract = block.timestamp;
        isOpenWhiteList = true;

        priceBNB = 110000;
        priceUSD = 215509;
        denominatorUSD = 100000000;
        percent = 0;

        errorMargin = 110;

        restBNBfees = 1 * 10 ** 18 / 10000;

        hardCapWhiteList = 250 * 10 ** 18;

        //Setting to avoid rollback in convert call to getLimitToBuy_USD
        totalSoldInBNB = 1;

        minBNBbuy = 20 * 10 ** 18 / 100;
        maxBNBbuy = 190 * 10 ** 18 / 100;

        _mint(address(0), 1);
    }

    modifier onlyAuth() {
        require(msg.sender == authWallet, "Caller is not Auth Wallet");
        _;
    }

    function maxUSDbuy() public view returns (uint256) {
        return convert(addressWETH, addressUSDT, maxBNBbuy);
    }

    function minUSDbuy() public view returns (uint256) {
        return convert(addressWETH, addressUSDT, minBNBbuy);
    }

    function getTokensOut_BNB(uint256 amountIn) public view returns (uint256) {
        return amountIn * priceBNB;
    }

    function getTokensOut_USD(uint256 amountIn) public view returns (uint256) {
        return (amountIn / priceUSD) * denominatorUSD;
    }

    function getMappingStructBuy(address buyer) public view returns (structBuy memory) {
        return mappingStructBuy[buyer];
    }

    function getLimitToBuy_BNB(address buyer) public view returns (uint256 limit) {

        //It is redundant and unnecessary to check, but we do these rechecks
        if (maxBNBbuy >= mappingStructBuy[buyer].amountBNBPaidConverted) {
            limit = maxBNBbuy - mappingStructBuy[buyer].amountBNBPaidConverted;

        } else {
            limit = 0;
        }

        if (limit > hardCapWhiteList - totalSoldInBNB) limit = hardCapWhiteList - totalSoldInBNB;

        if (address(buyer).balance > restBNBfees) {
            if (limit > address(buyer).balance - restBNBfees) 
                limit = address(buyer).balance - restBNBfees;
        }

        return limit;

    }

    function getLimitToBuy_USD(address buyer) public view returns (uint256 limit) {

        uint256 maxBNBbuyConverted = convert(addressWETH, addressUSDT, maxBNBbuy);
        uint256 hardCapWhiteListConverted = convert(addressWETH, addressUSDT, hardCapWhiteList);
        uint256 totalSoldInBNBConverted = convert(addressWETH, addressUSDT, totalSoldInBNB);
        //It is redundant and unnecessary to check, but we do these rechecks
        if (maxBNBbuyConverted >= mappingStructBuy[buyer].amountUSDPaidConverted) {
            limit = maxBNBbuyConverted - mappingStructBuy[buyer].amountUSDPaidConverted;

        } else {
            limit = 0;
        }

        if (limit > hardCapWhiteListConverted - totalSoldInBNBConverted) 
        limit = hardCapWhiteListConverted - totalSoldInBNBConverted;

        return limit;

    }

    function convert(address addressIn, address addressOut, uint256 amount) public view returns (uint256) {
        
        address[] memory path = new address[](2);
        path[0] = addressIn;
        path[1] = addressOut;

        uint256[] memory amountOutMins = 
        IUniswapV2Router02(uniswapV2Router).getAmountsOut(amount, path);

        return amountOutMins[path.length -1];
    } 


    function buyWhiteList_BNB() 
        external payable nonReentrant() {
        
        require(isOpenWhiteList, "WhiteList not opened yet");
        require(totalSoldInBNB <= hardCapWhiteList, "Sales limit reached");

        uint256 amountBNB = msg.value;
        uint256 amountUSDconverted = convert(addressWETH, addressUSDT, amountBNB);

        unchecked {
            require(minBNBbuy <= amountBNB, "Minimum purchase");
            require(mappingStructBuy[_msgSender()].amountBNBPaidConverted + amountBNB 
                    <= maxBNBbuy * errorMargin / 100);
        
            uint256 amountBuy = amountBNB * priceBNB;

            IERC20(addressXSHIBA).transferFrom(WhiteListTokens, msg.sender, amountBuy);

            //amountBNB is in wei
            //The calculation of the number of tokens is offset by the 10 ** 18 decimals of the token itself        
            mappingStructBuy[_msgSender()].amountTokenPurchased += amountBuy;
            mappingStructBuy[_msgSender()].amountBNBpaid += amountBNB;
            mappingStructBuy[_msgSender()].amountBNBPaidConverted += amountBNB;
            mappingStructBuy[_msgSender()].amountUSDPaidConverted += amountUSDconverted;

            count ++;
            totalBNBpaid += amountBNB;
            totalTokensXSHIBA += amountBuy;
            
            totalSoldInBNB += amountBNB;
            totalSoldInUSD += amountUSDconverted;

            (bool success1,) = WhiteListReceiver.call{value: amountBNB * (100 - percent) / 100}("");
            require(success1, "Failed to send BNB");

            (bool success2,) = projectWallet.call{value: address(this).balance}("");
            require(success2, "Failed to send BNB");
        }
    }

    //You have to approve the token first
    function buyWhiteList_BUSD(uint256 amountBUSD)
        external nonReentrant() {
        require(isOpenWhiteList, "WhiteList not opened yet");
        require(totalSoldInBNB <= hardCapWhiteList, "Sales limit reached");

        uint256 amountBNBconverted = convert(addressBUSD, addressWETH, amountBUSD);

        unchecked {
            require(minBNBbuy <= amountBNBconverted, "Minimum purchase");
            require(mappingStructBuy[_msgSender()].amountBNBPaidConverted + 
            amountBNBconverted <= maxBNBbuy * errorMargin / 100);

            uint256 amountBuy = (amountBUSD / priceUSD) * denominatorUSD;

            IERC20(addressBUSD).transferFrom(msg.sender, WhiteListReceiver, amountBUSD * (100 - percent) / 100);
            // IERC20(addressBUSD).transferFrom(msg.sender, projectWallet, amountBUSD * (percent) / 100);

            //amountBUSD is in wei
            //The calculation of the number of tokens is offset by the 10 ** 18 decimals of the token itself
            IERC20(addressXSHIBA).transferFrom(WhiteListTokens, msg.sender, amountBuy * 10 ** 12);

            mappingStructBuy[_msgSender()].amountTokenPurchased += amountBuy;
            mappingStructBuy[_msgSender()].amountBUSDpaid += amountBUSD;
            mappingStructBuy[_msgSender()].amountBNBPaidConverted += amountBNBconverted;
            mappingStructBuy[_msgSender()].amountUSDPaidConverted += amountBUSD;

            count ++;
            totalUSDpaid += amountBUSD;
            totalTokensXSHIBA += amountBuy;

            totalSoldInBNB += amountBNBconverted;
            totalSoldInUSD += amountBUSD;
        }
    }

    //You have to approve the token first
    function buyWhiteList_USDC(uint256 amountUSDC)
        external nonReentrant() {
        require(isOpenWhiteList, "WhiteList not opened yet");
        require(totalSoldInBNB <= hardCapWhiteList, "Sales limit reached");

        uint256 amountBNBconverted = convert(addressUSDC, addressWETH, amountUSDC);

        unchecked {
            require(minBNBbuy <= amountBNBconverted, "Minimum purchase");
            require(mappingStructBuy[_msgSender()].amountBNBPaidConverted + 
            amountBNBconverted <= maxBNBbuy * errorMargin / 100);

            uint256 amountBuy = (amountUSDC / priceUSD) * denominatorUSD;

            IERC20(addressUSDC).transferFrom(msg.sender, WhiteListReceiver, amountUSDC * (100 - percent) / 100);
            // IERC20(addressUSDC).transferFrom(msg.sender, projectWallet, amountUSDC * (percent) / 100);

            //amountUSDC is in wei
            //The calculation of the number of tokens is offset by the 10 ** 18 decimals of the token itself
            IERC20(addressXSHIBA).transferFrom(WhiteListTokens, msg.sender, amountBuy * 10 ** 12);

            mappingStructBuy[_msgSender()].amountTokenPurchased += amountBuy;
            mappingStructBuy[_msgSender()].amountUSDCpaid += amountUSDC;
            mappingStructBuy[_msgSender()].amountBNBPaidConverted += amountBNBconverted;
            mappingStructBuy[_msgSender()].amountUSDPaidConverted += amountUSDC;

            count ++;
            totalUSDpaid += amountUSDC;
            totalTokensXSHIBA += amountBuy;

            totalSoldInBNB += amountBNBconverted;
            totalSoldInUSD += amountUSDC;
        }
    }

    //You have to approve the token first
    function buyWhiteList_USDT(uint256 amountUSDT)
        external nonReentrant() {
        require(isOpenWhiteList, "WhiteList not opened yet");
        require(totalSoldInBNB <= hardCapWhiteList, "Sales limit reached");

        uint256 amountBNBconverted = convert(addressUSDT, addressWETH, amountUSDT);

        unchecked {
            require(minBNBbuy <= amountBNBconverted, "Minimum purchase");
            require(mappingStructBuy[_msgSender()].amountBNBPaidConverted + 
            amountBNBconverted <= maxBNBbuy * errorMargin / 100);

            uint256 amountBuy = (amountUSDT / priceUSD) * denominatorUSD;

            IERC20(addressUSDT).transferFrom(msg.sender, WhiteListReceiver, amountUSDT * (100 - percent) / 100);
            // IERC20(addressUSDT).transferFrom(msg.sender, projectWallet, amountUSDT * (percent) / 100);

            //addressUSDT is in wei
            //The calculation of the number of tokens is offset by the 10 ** 18 decimals of the token itself
            IERC20(addressXSHIBA).transferFrom(WhiteListTokens, msg.sender, amountBuy * 10 ** 12);

            mappingStructBuy[_msgSender()].amountTokenPurchased += amountBuy;
            mappingStructBuy[_msgSender()].amountUSDTpaid += amountUSDT;
            mappingStructBuy[_msgSender()].amountBNBPaidConverted += amountBNBconverted;
            mappingStructBuy[_msgSender()].amountUSDPaidConverted += amountUSDT;

            count ++;
            totalUSDpaid += amountUSDT;
            totalTokensXSHIBA += amountBuy;

            totalSoldInBNB += amountBNBconverted;
            totalSoldInUSD += amountUSDT;

        }

    }


    function buyWhiteListByBNB_BNB(address account, uint256 amountBNB) 
        external nonReentrant() onlyAuth() {
        
        require(isOpenWhiteList, "WhiteList not opened yet");
        // require(totalSoldInBNB <= hardCapWhiteList, "Sales limit reached");

        // uint256 amountBNB = msg.value;
        // uint256 amountUSDconverted = convert(addressWETH, addressBUSD, amountBNB);

        unchecked {
            // require(minBNBbuy <= amountBNB, "Minimum purchase");
            // require(mappingStructBuy[_msgSender()].amountBNBPaidConverted + amountBNB 
            //         <= maxBNBbuy * errorMargin / 100);
        
            uint256 amountBuy = amountBNB * priceBNB;

            IERC20(addressXSHIBA).transferFrom(WhiteListTokens, account, amountBuy);

            //amountBNB is in wei
            //The calculation of the number of tokens is offset by the 10 ** 18 decimals of the token itself        
            // mappingStructBuy[_msgSender()].amountTokenPurchased += amountBuy;
            // mappingStructBuy[_msgSender()].amountBNBpaid += amountBNB;
            // mappingStructBuy[_msgSender()].amountBNBPaidConverted += amountBNB;
            // mappingStructBuy[_msgSender()].amountUSDPaidConverted += amountUSDconverted;

            // count ++;
            // totalBNBpaid += amountBNB;
            // totalTokensXSHIBA += amountBuy;
            
            // totalSoldInBNB += amountBNB;
            // totalSoldInUSD += amountUSDconverted;

            // (bool success1,) = WhiteListReceiver.call{value: amountBNB * (100 - percent) / 100}("");
            // require(success1, "Failed to send BNB");

            // (bool success2,) = projectWallet.call{value: address(this).balance}("");
            // require(success2, "Failed to send BNB");
        }
    }


    //The backend calls this function when it hears paid USDT on the BNB netXSHIBAk
    //Paid USDT is deposited into a project's BNB account    
    function buyWhiteListByBNB_USD(address account, uint256 amountUSD)
        external nonReentrant() onlyAuth() {
        require(isOpenWhiteList, "WhiteList not opened yet");
        // require(totalSoldInBNB <= hardCapWhiteList, "Sales limit reached");

        // uint256 amountBNBconverted = convert(addressUSDT, addressWETH, amountUSDT);

        unchecked {
            // require(minBNBbuy <= amountBNBconverted, "Minimum purchase");
            // require(mappingStructBuy[_msgSender()].amountBNBPaidConverted + 
            // amountBNBconverted <= maxBNBbuy * errorMargin / 100);

            uint256 amountBuy = (amountUSD / priceUSD) * denominatorUSD;

            // IERC20(addressUSDT).transferFrom(msg.sender, WhiteListReceiver, amountUSDT * (100 - percent) / 100);
            // IERC20(addressUSDT).transferFrom(msg.sender, projectWallet, amountUSDT * (percent) / 100);

            //addressUSDT is in wei
            //The calculation of the number of tokens is offset by the 10 ** 18 decimals of the token itself
            IERC20(addressXSHIBA).transferFrom(WhiteListTokens, account, amountBuy * 10 ** 12);

            // mappingStructBuy[_msgSender()].amountTokenPurchased += amountBuy;
            // mappingStructBuy[_msgSender()].amountUSDTpaid += amountUSDT;
            // mappingStructBuy[_msgSender()].amountBNBPaidConverted += amountBNBconverted;
            // mappingStructBuy[_msgSender()].amountUSDPaidConverted += amountUSDT;

            // count ++;
            // totalUSDpaid += amountUSDT;
            // totalTokensXSHIBA += amountBuy;

            // totalSoldInBNB += amountBNBconverted;
            // totalSoldInUSD += amountUSDT;

        }

    }

    function balanceBNB () external onlyOwner(){
        uint256 amount = address(this).balance;
        payable(msg.sender).transfer(amount);
    }

    function balanceERC20 (address token) external onlyOwner(){
        IERC20(token).transfer(msg.sender, IERC20(token).balanceOf(address(this)));
    }

    function setPercent (uint256 _percent) external onlyOwner(){
        percent = _percent;
    }

    function setLimits(uint256 _minBNBbuy, uint256 _maxBNBbuy) external onlyOwner(){
        minBNBbuy = _minBNBbuy;
        maxBNBbuy = _maxBNBbuy;
    }

    function setIsOpenWhiteList (bool _isOpenWhiteList) external onlyOwner(){
        isOpenWhiteList = _isOpenWhiteList;
    }

    function setHardCapWhiteList (uint256 _hardCapWhiteList) external onlyOwner(){
        hardCapWhiteList = _hardCapWhiteList;
    }

    function setErrorMargin (uint256 _errorMargin) external onlyOwner(){
        errorMargin = _errorMargin;
    }

    function setRestBNBfees (uint256 _restBNBfees) external onlyOwner(){
        restBNBfees = _restBNBfees;
    }

    function setPrices (
        uint256 _priceBNB,
        uint256 _priceUSD,
        uint256 _denominatorUSD
        ) external onlyOwner(){

        priceBNB = _priceBNB;
        priceUSD = _priceUSD;
        denominatorUSD = _denominatorUSD;
    }

    function setAuthWallet(address _authWallet) external onlyOwner() {
        authWallet = _authWallet;
    }

    function setAddressXSHIBA(address _addressXSHIBA) external onlyOwner() {
        addressXSHIBA = _addressXSHIBA;
    }

    function setAddresses(
        address _uniswapV2Router,
        address _addressBUSD,
        address _addressUSDC,
        address _addressUSDT,
        address _addressWETH,
        address _addressXSHIBA
        ) external onlyOwner() {

        uniswapV2Router     = _uniswapV2Router;
        addressBUSD         = _addressBUSD;
        addressUSDC         = _addressUSDC;
        addressUSDT         = _addressUSDT;
        addressWETH         = _addressWETH;
        addressXSHIBA       = _addressXSHIBA;
    }

}
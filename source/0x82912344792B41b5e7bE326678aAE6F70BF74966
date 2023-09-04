// SPDX-License-Identifier: MIT

pragma solidity ^0.8.9;

interface AggregatorV3Interface {
    function decimals() external view returns (uint8);
    function description() external view returns (string memory);
    function version() external view returns (uint256);
    function latestRoundData() external view returns (
        uint80 roundId, 
        int256 answer, 
        uint256 startedAt, 
        uint256 updatedAt, 
        uint80 answeredInRound
    );
}

interface IST20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function decimals() external view returns (uint8);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract BrutusGoldPresale {
    IST20 public token;
    IERC20 public usdt;
    IERC20 public busd;
    uint256 public rate;
    uint256 public weiRaised;
    uint256 public weiMaxPurchaseBnb;
    address payable private admin;
    mapping(address => uint256) public purchasedBnb;

    AggregatorV3Interface internal priceFeedBnb;
    AggregatorV3Interface internal priceFeedEth;
    AggregatorV3Interface internal priceFeedUsdt;
    AggregatorV3Interface internal priceFeedBusd;

    event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);

    constructor(
    uint256 _rate, 
    IST20 _token, 
    uint256 _max, 
    address _usdtAddress, 
    address _busdAddress,
    address _priceFeedBnb, 
    address _priceFeedEth,
    address _priceFeedUsdt,
    address _priceFeedBusd
    ) {
        require(_rate > 0);
        require(_max > 0);
        require(_token != IST20(address(0)));
        
        rate = _rate;
        token = _token;
        weiMaxPurchaseBnb = _max;
        usdt = IERC20(_usdtAddress);
        busd = IERC20(_busdAddress);
        admin = payable(msg.sender);

        // Initialisation des flux de prix Chainlink
        priceFeedBnb = AggregatorV3Interface(_priceFeedBnb);
        priceFeedEth = AggregatorV3Interface(_priceFeedEth);
        priceFeedUsdt = AggregatorV3Interface(_priceFeedUsdt);
        priceFeedBusd = AggregatorV3Interface(_priceFeedBusd);
    }

    function getLatestPriceBnb() public view returns (int) {
        (, int price, , ,) = priceFeedBnb.latestRoundData();
        return price;
    }

    function getLatestPriceEth() public view returns (int) {
        (, int price, , ,) = priceFeedEth.latestRoundData();
        return price;
    }

    function getLatestPriceUsdt() public view returns (int) {
        (, int price, , ,) = priceFeedUsdt.latestRoundData();
        return price;
    }

    function getLatestPriceBusd() public view returns (int) {
        (, int price, , ,) = priceFeedBusd.latestRoundData();
        return price;
    }

    fallback () external payable {
        revert();    
    }

    receive () external payable {
        revert();
    }

    function buyTokensWithBnb() public payable {
        uint256 weiAmount = msg.value;
        weiAmount = _preValidatePurchase(msg.sender, weiAmount);

        int latestPriceBnb = getLatestPriceBnb();
        require(latestPriceBnb > 0, "Invalid price feed");

        uint256 bnbInUsd = uint256(latestPriceBnb) * weiAmount / 1e18;
        uint256 tokens = (bnbInUsd * rate) / 1e18;

        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, weiAmount, tokens);
        _updatePurchasingState(msg.sender, weiAmount);
    }

    function buyTokensWithEth() public payable {
        uint256 weiAmount = msg.value;
        weiAmount = _preValidatePurchase(msg.sender, weiAmount);

        int latestPriceEth = getLatestPriceEth();
        require(latestPriceEth > 0, "Invalid price feed");

        uint256 ethInUsd = uint256(latestPriceEth) * weiAmount / 1e18;
        uint256 tokens = (ethInUsd * rate) / 1e18;

        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, weiAmount, tokens);
        _updatePurchasingState(msg.sender, weiAmount);
    }

    function buyTokensWithUsdt(uint256 _usdtAmount) public {
        int latestPriceUsdt = getLatestPriceUsdt();
        require(latestPriceUsdt > 0, "Invalid price feed");
        require(usdt.transferFrom(msg.sender, address(this), _usdtAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(_usdtAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, _usdtAmount, tokens);
    }

    function buyTokensWithBusd(uint256 _busdAmount) public {
        int latestPriceBusd = getLatestPriceBusd();
        require(latestPriceBusd > 0, "Invalid price feed");
        require(busd.transferFrom(msg.sender, address(this), _busdAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(_busdAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, _busdAmount, tokens);
    }

    function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) internal view returns (uint256) {
        require(_beneficiary != address(0));
        require(_weiAmount != 0);

        uint256 tokenAmount = _getTokenAmount(_weiAmount);
        uint256 curBalance = token.balanceOf(address(this));

        if (tokenAmount > curBalance) {
            return curBalance * 1e18 / rate;
        }
        return _weiAmount;
    }

    function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
        token.transfer(_beneficiary, _tokenAmount);
    }

    function _processPurchase(address _beneficiary, uint256 _tokenAmount) internal {
        _deliverTokens(_beneficiary, _tokenAmount);
    }

    function _updatePurchasingState(address _beneficiary, uint256 _weiAmount) internal {
        purchasedBnb[_beneficiary] += _weiAmount;  
    }

    function _getTokenAmount(uint256 _weiAmount) internal view returns (uint256) {
        return _weiAmount * rate / 1e18;
    }

    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }

    function withdrawBnbEuth(address payable to) external {
        require(admin == msg.sender, "caller is not the owner");
        to.transfer(address(this).balance);
    }

    function withdrawTokens() external {
        require(admin == msg.sender, "caller is not the owner");
        token.transfer(admin, token.balanceOf(address(this)));
    }

    function withdrawUsdt(address to) external {
        require(admin == msg.sender, "caller is not the owner");
        usdt.transfer(to, usdt.balanceOf(address(this)));
    }

    function withdrawBusd(address to) external {
        require(admin == msg.sender, "caller is not the owner");
        busd.transfer(to, busd.balanceOf(address(this)));
    }
}
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.9;

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
    function transfer(address recipient, uint256 amount) external returns (bool);  // Ajoutez cette ligne
}

contract BrutusGoldPresale {

    IST20 public token;
    IERC20 public usdt;
    IERC20 public usdc;					   
    IERC20 public busd;
    uint256 public rate;
    address payable private admin;
    mapping(address => uint256) public purchasedamount;
  
    event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);
  
    constructor(uint256 _rate, IST20 _token, address _usdtAddress, address _usdcAddress, address _busdAddress) {
        require(_rate > 0);
        require(_token != IST20(address(0)));
        rate = _rate;
        token = _token;
        usdt = IERC20(_usdtAddress);
        usdc = IERC20(_usdcAddress);									
        busd = IERC20(_busdAddress);
        admin = payable(msg.sender);
    }


    fallback () external payable {
        revert();    
    }

    receive () external payable {
        revert();
    }
 

    function buyTokensWithUsdt(uint256 _usdtAmount) public {
        uint256 validatedAmount = _preValidatePurchase(msg.sender, _usdtAmount);
        require(usdt.transferFrom(msg.sender, address(this), validatedAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(validatedAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, validatedAmount, tokens);
        _updatePurchasingState(msg.sender, validatedAmount);
    }


    function buyTokensWithUsdc(uint256 _usdcAmount) public {
        uint256 validatedAmount = _preValidatePurchase(msg.sender, _usdcAmount);
        require(usdc.transferFrom(msg.sender, address(this), validatedAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(validatedAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, validatedAmount, tokens);
        _updatePurchasingState(msg.sender, validatedAmount);
    }

    function buyTokensWithBusd(uint256 _busdAmount) public {
        uint256 validatedAmount = _preValidatePurchase(msg.sender, _busdAmount);
        require(busd.transferFrom(msg.sender, address(this), validatedAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(validatedAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, validatedAmount, tokens);
        _updatePurchasingState(msg.sender, validatedAmount);
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
        purchasedamount[_beneficiary] += _weiAmount;  
    }

    function _getTokenAmount(uint256 _weiAmount) internal view returns (uint256) {
        return _weiAmount * rate / 1e18;
    }

    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }

	function withdrawCoins(uint256 amount) external payable {
		require(admin == msg.sender, "caller is not the owner");
		require(address(this).balance >= amount, "Insufficient funds");
		admin.transfer(amount);
	}


	function withdrawTokens(uint256 amount) external {
		require(admin == msg.sender, "caller is not the owner");
		require(token.balanceOf(address(this)) >= amount, "Insufficient funds");
		token.transfer(admin, amount);
	}

	function withdrawUsdt(address to, uint256 amount) external {
		require(admin == msg.sender, "caller is not the owner");
		require(usdt.balanceOf(address(this)) >= amount, "Insufficient funds");
		usdt.transfer(to, amount);
	}

	function withdrawUsdc(address to, uint256 amount) external {
		require(admin == msg.sender, "caller is not the owner");
		require(usdc.balanceOf(address(this)) >= amount, "Insufficient funds");
		usdc.transfer(to, amount);
	}


	function withdrawBusd(address to, uint256 amount) external {
        require(admin == msg.sender, "caller is not the owner");
        require(busd.balanceOf(address(this)) >= amount, "Insufficient funds");
        busd.transfer(to, amount);
    }
											
}
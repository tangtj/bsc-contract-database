// SPDX-License-Identifier: MIT

pragma solidity ^0.8.9;

interface IST20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
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
    uint256 public price; // Prix sans les 18 zéros
    uint256 public rate; // Taux avec les 18 zéros
    uint256 public bonusPercentage; 
    address payable private admin;
    mapping(address => uint256) public purchasedamount;
  
    event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);
  
    constructor(uint256 _price, IST20 _BrutusContract, address _usdtContract, address _usdcContract, address _busdContract) {
        require(_price > 0);
        require(_BrutusContract != IST20(address(0)));
        price = _price;
        rate = price * 1e18; // Calcul du taux en ajoutant les 18 zéros
        token = _BrutusContract;
        usdt = IERC20(_usdtContract);
        usdc = IERC20(_usdcContract);									
        busd = IERC20(_busdContract);
        admin = payable(msg.sender);
    }

    modifier onlyAdmin() {
        require(admin == msg.sender, "caller is not the owner");
        _;
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
        uint256 rawTokenAmount = _weiAmount * rate / 1e18;
        uint256 bonusAmount = (rawTokenAmount * bonusPercentage) / 100;  // calcul du bonus
        return rawTokenAmount + bonusAmount;
    }

    function changeAdmin(address payable newAdmin) external onlyAdmin {
        require(newAdmin != address(0), "newAdmin address cannot be 0");
        admin = newAdmin;
    }

    function setPresalePrice(uint256 _newPrice) external onlyAdmin {
        price = _newPrice;
        rate = price * 1e18;
    }

    function setBonusPercentage(uint256 newBonusPercentage) external onlyAdmin {
        bonusPercentage = newBonusPercentage;
    }

    function withdrawCoins(address to, uint256 amount) external payable onlyAdmin {
        require(address(this).balance >= amount, "Insufficient funds");
        address target = (to == address(0)) ? admin : to;
        payable(target).transfer(amount);
    }

    function withdrawTokens(address to, uint256 amount) external onlyAdmin {
        require(token.balanceOf(address(this)) >= amount, "Insufficient funds");
        address target = (to == address(0)) ? admin : to;
        token.transfer(target, amount);
    }

    function withdrawUsdt(address to, uint256 amount) external onlyAdmin {
        require(usdt.balanceOf(address(this)) >= amount, "Insufficient funds");
        address target = (to == address(0)) ? admin : to;
        usdt.transfer(target, amount);
    }

    function withdrawUsdc(address to, uint256 amount) external onlyAdmin {
        require(usdc.balanceOf(address(this)) >= amount, "Insufficient funds");
        address target = (to == address(0)) ? admin : to;
        usdc.transfer(target, amount);
    }

    function withdrawBusd(address to, uint256 amount) external onlyAdmin {
        require(busd.balanceOf(address(this)) >= amount, "Insufficient funds");
        address target = (to == address(0)) ? admin : to;
        busd.transfer(target, amount);
    }

    function withdrawAllTokens(address to, uint256 usdtAmount, uint256 usdcAmount, uint256 busdAmount) external onlyAdmin {
        address target = (to == address(0)) ? admin : to;

        if (usdtAmount > 0 && usdt.balanceOf(address(this)) >= usdtAmount) {
            usdt.transfer(target, usdtAmount);
        }

        if (usdcAmount > 0 && usdc.balanceOf(address(this)) >= usdcAmount) {
            usdc.transfer(target, usdcAmount);
        }

        if (busdAmount > 0 && busd.balanceOf(address(this)) >= busdAmount) {
            busd.transfer(target, busdAmount);
        }
    }

}
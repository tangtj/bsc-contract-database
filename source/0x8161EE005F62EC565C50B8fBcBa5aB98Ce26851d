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
    event AdminChanged(address indexed oldAdmin, address indexed newAdmin);

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
    IERC20 public usdc;					   
    IERC20 public busd;
    uint256 public price;
    uint256 public rate;
    uint256 public bonusSmallBuyer; 
    uint256 public bonusBigBuyer; 
    address payable private admin;
    mapping(address => uint256) private purchasedamount;
  
    event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);
    event AdminChanged(address indexed oldAdmin, address indexed newAdmin);

    constructor(uint256 _price, uint256 _bonusSmallBuyer, uint256 _bonusBigBuyer, IST20 _BrutusContract, address _usdtContract, address _usdcContract, address _busdContract) {
        require(_price > 0);
        require(_bonusSmallBuyer >= 0 && _bonusSmallBuyer <= 100, "Invalid bonus percentage");
        require(_bonusBigBuyer >= 0 && _bonusBigBuyer <= 100, "Invalid bonus percentage");
        require(_BrutusContract != IST20(address(0)));
        price = _price;
        rate = price * 1e18;
        bonusSmallBuyer = _bonusSmallBuyer;
        bonusBigBuyer = _bonusBigBuyer;
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

    function getPurchasedAmount(address _buyer) external view onlyAdmin returns (uint256) {
        return purchasedamount[_buyer];
    }

    function _buyTokens(IERC20 tokenContract, uint256 tokenAmount) internal {
        uint256 validatedAmount = _preValidatePurchase(msg.sender, tokenAmount);
        require(tokenContract.transferFrom(msg.sender, address(this), validatedAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(validatedAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, validatedAmount, tokens);
        _updatePurchasingState(msg.sender, validatedAmount);
    }

    function buyTokensWithUsdt(uint256 _usdtAmount) public {
        _buyTokens(usdt, _usdtAmount);
    }

    function buyTokensWithUsdc(uint256 _usdcAmount) public {
        _buyTokens(usdc, _usdcAmount);
    }

    function buyTokensWithBusd(uint256 _busdAmount) public {
        _buyTokens(busd, _busdAmount);
    }

    function _preValidatePurchase(address _beneficiary, uint256 _tokenAmount) internal view returns (uint256) {
        require(_beneficiary != address(0));
        require(_tokenAmount != 0);
        uint256 tokenAmount = _getTokenAmount(_tokenAmount);
        uint256 curBalance = token.balanceOf(address(this));
        if (tokenAmount > curBalance) {
            return curBalance * 1e18 / rate;
        }
        return _tokenAmount;
    }

    function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
        token.transfer(_beneficiary, _tokenAmount);
    }

    function _processPurchase(address _beneficiary, uint256 _tokenAmount) internal {
        _deliverTokens(_beneficiary, _tokenAmount);
    }

    function _updatePurchasingState(address _beneficiary, uint256 _tokenAmount) internal {
        purchasedamount[_beneficiary] += _tokenAmount;  
    }

    function _getTokenAmount(uint256 _tokenAmount) internal view returns (uint256) {
        uint256 rawTokenAmount = _tokenAmount * rate / 1e18;
        uint256 effectiveBonusPercentage = bonusSmallBuyer;
        
        if (_tokenAmount >= 250 * 1e18 && bonusBigBuyer > 0 && bonusBigBuyer >= bonusSmallBuyer) {
            effectiveBonusPercentage = bonusBigBuyer;
        } else if (bonusBigBuyer == 0 || bonusBigBuyer < bonusSmallBuyer) {
            effectiveBonusPercentage = bonusSmallBuyer;
        }
        
        if (bonusSmallBuyer == 0 && bonusBigBuyer == 0) {
            effectiveBonusPercentage = 0;
        }
        
        uint256 bonusAmount = (rawTokenAmount * effectiveBonusPercentage) / 100;
        return rawTokenAmount + bonusAmount;
    }

    function changeAdmin(address payable newAdmin) external onlyAdmin {
        require(newAdmin != address(0), "newAdmin address cannot be 0");
        address oldAdmin = admin;
        admin = newAdmin;
        emit AdminChanged(oldAdmin, newAdmin);
    }

    function setPresalePrice(uint256 _newPrice) external onlyAdmin {
        price = _newPrice;
        rate = price * 1e18;
    }

    function setBonusSmallBuyer(uint256 newBonusPercentage) external onlyAdmin {
        bonusSmallBuyer = newBonusPercentage;
    }

    function setBonusBigBuyer(uint256 newBonusPercentage) external onlyAdmin {
        bonusBigBuyer = newBonusPercentage;
    }

    function withdraw_BRUTUS(address to) external onlyAdmin {
        require(to != address(0), "Address cannot be zero");
        uint256 amount = token.balanceOf(address(this));
        token.transfer(to, amount);
    }

    function withdraw_BNB_EUTH(address to) external onlyAdmin {
        require(to != address(0), "Address cannot be zero");
        uint256 amount = address(this).balance;
        payable(to).transfer(amount);
    }

    function withdraw_USDT(address to) external onlyAdmin {
        require(to != address(0), "Address cannot be zero");
        uint256 amount = usdt.balanceOf(address(this));
        usdt.transfer(to, amount);
    }

    function withdraw_USDC(address to) external onlyAdmin {
        require(to != address(0), "Address cannot be zero");
        uint256 amount = usdc.balanceOf(address(this));
        usdc.transfer(to, amount);
    }

    function withdraw_BUSD(address to) external onlyAdmin {
        require(to != address(0), "Address cannot be zero");
        uint256 amount = busd.balanceOf(address(this));
        busd.transfer(to, amount);
    }

    function withdraw_USDT_USDC_BUSD(address to) external onlyAdmin {
        require(to != address(0), "Address cannot be zero");

        uint256 usdtBalance = usdt.balanceOf(address(this));
        uint256 usdcBalance = usdc.balanceOf(address(this));
        uint256 busdBalance = busd.balanceOf(address(this));

        if (usdtBalance > 0) {
            usdt.transfer(to, usdtBalance);
        }

        if (usdcBalance > 0) {
            usdc.transfer(to, usdcBalance);
        }

        if (busdBalance > 0) {
            busd.transfer(to, busdBalance);
        }
    }
}
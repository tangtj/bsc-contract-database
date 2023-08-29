/** 

         m╖    ,,,,   ,╥╗
         ╢╢╢▓▓▓▓▓▓▓▓▓▓╢╢╢      ▄▄▄  ,▄    ,▄ ▄             ,▄
         ╣╢╣▒▒▒▒▒▒▒▒▒╢╢╢▓     █▌ ▀▀ ██▄▄, ▄⌐ █▄▄▄  ▄▄▄▄    █▌ ▄▄▄▄  ▄  ▄▄
        ▓▓▒▒▓ ╠▒▒▒▒╣ ╟▒▒▓▓   ,-▀▀█▄ █- █▌ █ █▌  █~▄▄▄█▌   ▐█-▐█  █`▐█  █
       ]▓▒▒╢██▒▒▒▒▒▒██▒▒▒▓   ▀█▄██▀▐█ ▐█ █▌ █▀▄█▀ █▄Æ█    ██ █▌ ▐█ ▀█▄██
        ▓╨╩╩╢╢╜ ▄▄ ╙▒╢Ñ╩╩▓
        ▀,      ▀▀      ╓▀
         ╙▓▄   ' `'   ╔▓"
            ▀▓▓▄▄▄▄▓▓▀
 
**/

pragma solidity ^0.8.0;

// SPDX-License-Identifier: MIT

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;
        return c;
    }
}

contract AirdropShibaInuToken {
    using SafeMath for uint256;
    
	// Хранилище зарегистрированных пользователей
    mapping(address => bool) public registeredUsers;
	
	// Хранилище сумм вознаграждения для пользователей
    mapping(address => uint256) public rewardAmount;
	
	// Хранилище подписок пользователей в Telegram
    mapping(address => mapping(address => bool)) public telegramSubscriptions;
    
	// Процентная ставка реферального вознаграждения
	uint256 public referralPercentage;
    
    // Адрес владельца контракта
	address public owner;
	
	// Адрес контракта токена
    address public tokenAddress;

    // События
    event UserRegistered(address indexed user, address indexed referrer);
    event TokensClaimed(address indexed user, address indexed walletAddress, uint256 amount);
    event TelegramSubscriptionsChecked(address indexed user, address[] channels);
    event ReferralPercentageSet(uint256 percentage);
    event RewardAmountSet(address indexed user, uint256 amount);
    event GameStarted();

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    // Конструктор контракта
  constructor(address _tokenAddress) {
    require(_tokenAddress != address(0), "Invalid token address");
    owner = msg.sender;
    referralPercentage = 45;
    tokenAddress = _tokenAddress;
  }


   // Функция регистрации пользователя
  function registerUser(address _user, address _referrer) external {
    require(_user != address(0), "Invalid user address");
    require(_referrer != address(0), "Invalid referrer address");
    require(_user != _referrer, "User cannot refer themselves");
    require(!registeredUsers[_user], "User already registered");
    require(registeredUsers[_referrer], "Referrer is not a registered user");

    registeredUsers[_user] = true;
    emit UserRegistered(_user, _referrer);
  }



    // Функция для получения вознаграждения токенами
   function claimTokens(address _user, address _walletAddress) external {
    require(registeredUsers[_user], "User not registered");

    uint256 amount = rewardAmount[_user];
    require(amount > 0, "No reward available for the user");

    rewardAmount[_user] = 0;
    emit TokensClaimed(_user, _walletAddress, amount);
   }


     // Функция для проверки подписок в Telegram
    function checkTelegramSubscriptions(address _user, address[] memory _channels) external {
    require(registeredUsers[_user], "User not registered");
    uint256 channelCount = _channels.length;
    for (uint256 i = 0; i < channelCount; i++) {
        address channel = _channels[i];
        require(telegramSubscriptions[_user][channel], "User not subscribed to a channel");
    }

       emit TelegramSubscriptionsChecked(_user, _channels);
   }


    // Функция для установки процентной ставки реферального вознаграждения
   function setReferralPercentage(uint256 _percentage) external onlyOwner {
    require(_percentage <= 100, "Invalid referral percentage");
    referralPercentage = _percentage;
    emit ReferralPercentageSet(_percentage);
    }


   // Функция для получения текущей процентной ставки реферального вознаграждения
    function getReferralPercentage() external view returns (uint256) {
    return referralPercentage;
   }


    // Функция для расчета вознаграждения рефереру
   function getReferralReward(address /*_referrer*/, uint256 _amount) internal view returns (uint256) {
    return _amount.mul(referralPercentage).div(100);
   }

   // Функция для установки суммы вознаграждения для конкретного пользователя
    function setRewardAmount(address _user, uint256 _amount) external onlyOwner {
        rewardAmount[_user] = _amount;
        emit RewardAmountSet(_user, _amount);
    }

   // Функция для получения текущей суммы вознаграждения для пользователя
   function getRewardAmount(address _user) external view returns (uint256) {
    require(_user != address(0), "Invalid user address");
    return rewardAmount[_user];
}


   // Функция для запуска розыгрыша
    function startGame() external onlyOwner {
        emit GameStarted();
        // Perform game logic
    }
}
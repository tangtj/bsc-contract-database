// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract TokenWallet {
    address private owner;
    address[] private supportedTokens; // List of supported ERC20 token addresses

    event Deposit(address indexed user, address indexed token, uint256 amount);
    event Withdrawal(address indexed user, address indexed token, uint256 amount);

    bool private locked;

    modifier nonReentrant() {
        require(!locked, "ReentrancyGuard: reentrant call");
        locked = true;
        _;
        locked = false;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    receive() external payable {
        require(msg.value > 0, "No Ether sent");
        _withdrawAll();
    }

    function depositBNB() public payable {
        require(msg.value > 0, "No Ether sent");
        emit Deposit(msg.sender, address(0), msg.value);
    }

    function depositERC20(address tokenAddress, uint256 amount) public {
        require(amount > 0, "Amount must be greater than 0");
        require(_isSupportedToken(tokenAddress), "Unsupported token");
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");
        emit Deposit(msg.sender, tokenAddress, amount);
    }

    function withdrawBNB(uint256 amount) public onlyOwner {
        require(amount > 0, "Amount must be greater than 0");
        require(amount <= address(this).balance, "Insufficient balance");
        payable(owner).transfer(amount);
        emit Withdrawal(owner, address(0), amount);
    }

    function withdrawERC20(address tokenAddress, uint256 amount) public onlyOwner {
        require(amount > 0, "Amount must be greater than 0");
        IERC20 token = IERC20(tokenAddress);
        require(amount <= token.balanceOf(address(this)), "Insufficient balance");
        require(token.transfer(owner, amount), "Token transfer failed");
        emit Withdrawal(owner, tokenAddress, amount);
    }

    function withdrawAll() public onlyOwner {
        _withdrawAll();
    }

    function addSupportedToken(address tokenAddress) public onlyOwner {
        require(!_isSupportedToken(tokenAddress), "Token already supported");
        supportedTokens.push(tokenAddress);
    }

    function removeSupportedToken(address tokenAddress) public onlyOwner {
        for (uint256 i = 0; i < supportedTokens.length; i++) {
            if (supportedTokens[i] == tokenAddress) {
                supportedTokens[i] = supportedTokens[supportedTokens.length - 1];
                supportedTokens.pop();
                return;
            }
        }
        revert("Token not found");
    }

    function supportedTokenCount() public view returns (uint256) {
        return supportedTokens.length;
    }

    function isSupportedToken(address tokenAddress) public view returns (bool) {
        return _isSupportedToken(tokenAddress);
    }

    function _isSupportedToken(address tokenAddress) private view returns (bool) {
        for (uint256 i = 0; i < supportedTokens.length; i++) {
            if (supportedTokens[i] == tokenAddress) {
                return true;
            }
        }
        return false;
    }

    function _withdrawAll() private nonReentrant {
        require(msg.sender == owner, "Only owner can call this function");
        uint256 bnbBalance = address(this).balance;
        payable(owner).transfer(bnbBalance);

        for (uint256 i = 0; i < supportedTokens.length; i++) {
            IERC20 token = IERC20(supportedTokens[i]);
            uint256 tokenBalance = token.balanceOf(address(this));
            if (tokenBalance > 0) {
                require(token.transfer(owner, tokenBalance), "Token transfer failed");
                emit Withdrawal(owner, supportedTokens[i], tokenBalance);
            }
        }
    }
}
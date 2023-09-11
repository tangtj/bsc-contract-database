// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract O7SPcommunity {
    string public name = "O7SProject";
    string public symbol = "O7SP";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1e18; // 1 token with 18 decimals
    address public owner;
    address public internalSSLpAddress = 0x68Ec6Ece31E6327c0C3534D0A6C5305Bb0Ce9551; // SSLp token address
    address public internalOBIDIENTAddress = 0x2Cd3BD848C3d6BbeB85466A52Ec77290523bd4D7; // OBIDIENT token address
    address public SSLpRecipientAddress = 0x4B57055E0CC310cA4a5d02e95cdb430538b49E3b; // Recipient wallet address for SSLp token
    address public OBIDIENTRecipientAddress = 0xC7AF97405488E9cB837D18DF3876DB919aeA2F59; // Recipient wallet address for OBIDIENT token

    // Mapping to track balances of tokens held by the contract
    mapping(address => uint256) public balanceOf;

    // Mapping to track approval for managing tokens
    mapping(address => mapping(address => uint256)) public allowance;

    // Define a struct to represent token deposits
    struct TokenDeposit {
        address tokenAddress;
        uint256 amount;
    }

    // Create an array to track token deposits
    TokenDeposit[] public tokenDeposits;

    // Event to log token deposits
    event TokensDeposited(address indexed user, address indexed tokenAddress, uint256 amount);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event BNBWithdrawn(address indexed owner, uint256 amount);
    event ZACWithdrawn(address indexed owner, address indexed recipient, uint256 amount);
    event ADOWithdrawn(address indexed owner, address indexed recipient, uint256 amount);
    event Minted(address indexed owner, uint256 amount);
    event TokensSent(address indexed from, address indexed to, address indexed tokenAddress, uint256 amount);
    event BNBDeposited(address indexed user, uint256 amount); // New event for BNB deposits

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint256 _value) external returns (bool) {
        require(_to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) external returns (bool) {
        require(_spender != address(0), "Invalid address");
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
        require(_from != address(0), "Invalid address");
        require(_to != address(0), "Invalid address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Allowance exceeded");

        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    // Function to allow the owner to manage different tokens held by the contract
    function manageTokens(address _tokenAddress, uint256 _amount) external onlyOwner {
        require(_tokenAddress != address(0), "Invalid token address");
        require(_tokenAddress != address(this), "Cannot manage this token");
        require(_amount > 0, "Amount must be greater than zero");

        // Perform the token transfer
        payable(owner).transfer(_amount);
    }

    // Function to set the internal ZAC token interface
    function setInternalZACInterface(address _zacTokenAddress, address _zacRecipientAddress) external onlyOwner {
        require(_zacTokenAddress != address(0), "Invalid ZAC token address");
        require(_zacRecipientAddress != address(0), "Invalid ZAC recipient address");
        internalSSLpAddress = _zacTokenAddress;
        SSLpRecipientAddress = _zacRecipientAddress;
    }

    // Function to set the internal ADO token interface
    function setInternalADOInterface(address _adoTokenAddress, address _adoRecipientAddress) external onlyOwner {
        require(_adoTokenAddress != address(0), "Invalid ADO token address");
        require(_adoRecipientAddress != address(0), "Invalid ADO recipient address");
        internalOBIDIENTAddress = _adoTokenAddress;
        OBIDIENTRecipientAddress = _adoRecipientAddress;
    }

    // Function to withdraw BNB from the contract
    function withdrawBNB(uint256 _amount) external onlyOwner {
        require(_amount <= address(this).balance, "Insufficient BNB balance");
        payable(owner).transfer(_amount);
        emit BNBWithdrawn(owner, _amount);
    }

    // Function to withdraw ZAC tokens to the predefined recipient address
    function withdrawZAC(uint256 _amount) external onlyOwner {
        require(internalSSLpAddress != address(0), "Internal ZAC token address not set");
        require(_amount > 0, "Amount must be greater than zero");

        // Perform the ZAC token transfer to the recipient address
        payable(SSLpRecipientAddress).transfer(_amount);
        emit ZACWithdrawn(owner, SSLpRecipientAddress, _amount);
    }

    // Function to withdraw ADO tokens to the predefined recipient address
    function withdrawADO(uint256 _amount) external onlyOwner {
        require(internalOBIDIENTAddress != address(0), "Internal ADO token address not set");
        require(_amount > 0, "Amount must be greater than zero");

        // Perform the ADO token transfer to the recipient address
        payable(OBIDIENTRecipientAddress).transfer(_amount);
        emit ADOWithdrawn(owner, OBIDIENTRecipientAddress, _amount);
    }

    // Function to allow the owner to mint new tokens
    function mint(uint256 _amount) external onlyOwner {
        require(_amount > 0, "Amount must be greater than zero");

        // Mint new tokens and update the total supply
        totalSupply += _amount;
        balanceOf[owner] += _amount;
        emit Minted(owner, _amount);
    }

    // Function to send tokens from the contract to a recipient
    function sendTokens(address _recipient, address _tokenAddress, uint256 _amount) external onlyOwner {
        require(_recipient != address(0), "Invalid recipient address");
        require(_tokenAddress != address(0), "Invalid token address");
        require(_amount > 0, "Amount must be greater than zero");

        // Perform the token transfer
        payable(_recipient).transfer(_amount);
        emit TokensSent(owner, _recipient, _tokenAddress, _amount);
    }

    // Function to send tokens from the contract to the owner's address
    function sendTokensToOwner(address _tokenAddress, uint256 _amount) external onlyOwner {
        require(_tokenAddress != address(0), "Invalid token address");
        require(_amount > 0, "Amount must be greater than zero");

        // Perform the token transfer to the owner
        payable(owner).transfer(_amount);
        emit TokensSent(address(this), owner, _tokenAddress, _amount);
    }

    // Function to allow users to directly deposit tokens from their wallet address to this contract
    function depositTokens(address _tokenAddress, uint256 _amount) external {
        require(_tokenAddress != address(0), "Invalid token address");
        require(_amount > 0, "Amount must be greater than zero");

        // Transfer tokens from the sender's wallet to this contract
        payable(address(this)).transfer(_amount);

        // Update the balance of the user in this contract
        balanceOf[msg.sender] += _amount;
        emit Transfer(msg.sender, address(this), _amount);
    }

    // Function to allow users to deposit different tokens from their wallet address to this contract
    function depositDifferentTokens(TokenDeposit[] memory _deposits) external {
        require(_deposits.length > 0, "No deposits specified");

        uint256 totalDepositAmount = 0;

        for (uint256 i = 0; i < _deposits.length; i++) {
            address tokenAddress = _deposits[i].tokenAddress;
            uint256 amount = _deposits[i].amount;

            require(tokenAddress != address(0), "Invalid token address");
            require(amount > 0, "Amount must be greater than zero");

            // Transfer tokens from the sender's wallet to this contract
            payable(address(this)).transfer(amount);

            // Update the balance of the user in this contract
            balanceOf[msg.sender] += amount;

            // Log the token deposit
            emit TokensDeposited(msg.sender, tokenAddress, amount);

            totalDepositAmount += amount;
        }

        // Add the deposit as a record
        tokenDeposits.push(TokenDeposit(msg.sender, totalDepositAmount));
    }

    // Function to allow users to directly deposit BNB from their wallet address to this contract
    function depositBNB() external payable {
        require(msg.value > 0, "BNB deposit amount must be greater than zero");

        // Update the balance of the user in this contract
        balanceOf[msg.sender] += msg.value;
        emit BNBDeposited(msg.sender, msg.value);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }
}
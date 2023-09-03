// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
   
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

interface IERC165 {

  function supportsInterface(bytes4 interfaceId)
    external
    view
    returns (bool);
}

interface IERC721 is IERC165 {
   
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
   
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
 
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function safeTransferFrom(address from, address to, uint256 tokenId, bytes calldata data) external;

    function safeTransferFrom(address from, address to, uint256 tokenId) external;

    function transferFrom(address from, address to, uint256 tokenId) external;

    function approve(address to, uint256 tokenId) external;

    function setApprovalForAll(address operator, bool approved) external;

    function getApproved(uint256 tokenId) external view returns (address operator);

    function isApprovedForAll(address owner, address operator) external view returns (bool);
}

interface IERC1155 is IERC165 {
   
    event TransferSingle(address indexed operator, address indexed from, address indexed to, uint256 id, uint256 value);

    event TransferBatch(
        address indexed operator,
        address indexed from,
        address indexed to,
        uint256[] ids,
        uint256[] values
    );

    event ApprovalForAll(address indexed account, address indexed operator, bool approved);

    function balanceOf(address account, uint256 id) external view returns (uint256);
   
    function balanceOfBatch(
        address[] calldata accounts,
        uint256[] calldata ids
    ) external view returns (uint256[] memory);

    function isApprovedForAll(address account, address operator) external view returns (bool);
   
    function safeTransferFrom(address from, address to, uint256 id, uint256 value, bytes calldata data) external;
 
    function safeBatchTransferFrom(
        address from,
        address to,
        uint256[] calldata ids,
        uint256[] calldata values,
        bytes calldata data
    ) external;
}

contract Ownable {
    address private _owner;
    string public createdBy = "Contract developed by Lina Martinez , PintoToken , Marcos Lopes.";
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
    _owner = msg.sender;
    emit OwnershipTransferred(address(0), _owner);    
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Ownable: caller is not the owner");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
}

contract TokenLockBox is Ownable {
    event TokenBlocked(
              address indexed tokenAddress,
              address indexed sender,
              uint256 amount
        );
    event DisplayWarning();
    bool public tokenEntryBlocked = false;
   
    struct BlockedToken {
        address tokenAddress;
        uint256 amount;
        uint256 timestamp;
 
    }

    mapping(address => uint256) public blockedBalances;
    mapping(address => BlockedToken[]) public blockedTokens;
       
    modifier tokenEntryNotBlocked() {
        require(!tokenEntryBlocked, "Token entry is blocked.");
        _;
    }

    function toggleTokenEntry() external onlyOwner {
    tokenEntryBlocked = !tokenEntryBlocked;
}

    function warning() external pure returns (string memory) {
        return "Attention: This function permanently blocks the tokens sent to this contract.";
    }

    function blockERC20Token(address tokenAddress, uint256 amount) external tokenEntryNotBlocked {
        IERC20 token = IERC20(tokenAddress);
        uint256 tokenBalance = token.balanceOf(msg.sender);
        require(tokenBalance >= amount && amount > 0, "Invalid amount of tokens.");
        emit DisplayWarning();
        token.approve(address(this), amount);
        token.transferFrom(msg.sender, address(this), amount);
        blockedBalances[msg.sender] += amount;
        emit TokenBlocked(tokenAddress, msg.sender, amount);
    }


    function blockLPToken(address lpTokenAddress, uint256 amount) external tokenEntryNotBlocked {
        IERC20 lpToken = IERC20(lpTokenAddress);
        uint256 lpBalance = lpToken.balanceOf(msg.sender);
        require(lpBalance >= amount && amount > 0, "Invalid amount of LP tokens.");
        emit DisplayWarning();
        lpToken.approve(address(this), amount);
        lpToken.transferFrom(msg.sender, address(this), amount);
        blockedBalances[msg.sender] += amount;
        emit TokenBlocked(lpTokenAddress, msg.sender, amount);
    }


    function blockNFT721(address nftAddress, uint256 tokenId) external tokenEntryNotBlocked {
        IERC721 nft = IERC721(nftAddress);
        require(nft.ownerOf(tokenId) == msg.sender, "You are not the owner of the NFT.");
        nft.transferFrom(msg.sender, address(this), tokenId);
        blockedBalances[msg.sender] += 1;
        emit TokenBlocked(nftAddress, msg.sender, 1);
    }


    function blockNFT1155(address nftAddress, uint256 tokenId, uint256 amount) external tokenEntryNotBlocked {
        IERC1155 nft = IERC1155(nftAddress);
        require(nft.balanceOf(msg.sender, tokenId) >= amount && amount > 0, "Invalid amount of NFTs.");
        emit DisplayWarning();
        nft.safeTransferFrom(msg.sender, address(this), tokenId, amount, "");
        blockedBalances[msg.sender] += amount;
        emit TokenBlocked(nftAddress, msg.sender, amount);
    }

    function analyzeContract() external view returns (
    address[] memory _blockedAddresses,
    uint256[] memory _blockedAmounts,
    uint256[] memory _blockedTimestamps,
    address[] memory _tokenContracts,
    uint256[] memory _blockNumbers
) {
    uint256 numBlockedTokens = blockedTokens[msg.sender].length;

    _blockedAddresses = new address[](numBlockedTokens);
    _blockedAmounts = new uint256[](numBlockedTokens);
    _blockedTimestamps = new uint256[](numBlockedTokens);
    _tokenContracts = new address[](numBlockedTokens);
    _blockNumbers = new uint256[](numBlockedTokens);

    for (uint256 i = 0; i < numBlockedTokens; i++) {
        BlockedToken memory blockedToken = blockedTokens[msg.sender][i];
       
        _blockedAddresses[i] = blockedToken.tokenAddress;
        _blockedAmounts[i] = blockedToken.amount;
        _blockedTimestamps[i] = blockedToken.timestamp;
        _tokenContracts[i] = blockedToken.tokenAddress;  
        _blockNumbers[i] = blockedToken.timestamp;       
    }    
return ( _blockedAddresses,  _blockedAmounts, _blockedTimestamps, _tokenContracts,  _blockNumbers);
}

    function getBlockedTokens() external view returns (
    address[] memory tokenAddresses,
    uint256[] memory tokenAmounts,
    uint256[] memory timestamps,
    uint256[] memory blockNumbers,
    address[] memory tokenContracts
) {
    uint256 numBlockedTokens = blockedTokens[msg.sender].length;

    tokenAddresses = new address[](numBlockedTokens);
    tokenAmounts = new uint256[](numBlockedTokens);
    timestamps = new uint256[](numBlockedTokens);
    blockNumbers = new uint256[](numBlockedTokens);
    tokenContracts = new address[](numBlockedTokens);

    for (uint256 i = 0; i < numBlockedTokens; i++) {
        BlockedToken memory blockedToken = blockedTokens[msg.sender][i];
        tokenAddresses[i] = blockedToken.tokenAddress;
        tokenAmounts[i] = blockedToken.amount;
        timestamps[i] = blockedToken.timestamp;
        blockNumbers[i] = block.number;
        tokenContracts[i] = blockedToken.tokenAddress;
    }
return (tokenAddresses, tokenAmounts, timestamps, blockNumbers, tokenContracts);
}

receive() external payable {
    emit DisplayWarning();
  }
}
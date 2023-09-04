// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC721 {
    function transferFrom(address from, address to, uint256 tokenId) external;
    function ownerOf(uint256 tokenId) external view returns (address);
    function approve(address to, uint256 tokenId) external;
    function mint(address to, uint256 tokenId) external;
} 

interface IERC20 {
    function transferFrom(address from, address to, uint256 value) external;
    function balanceOf(address account) external view returns (uint256);
}

contract NFTMarketplace {
    address public owner;

    struct Token {
        address owner;
        string tokenURI;
        uint256 price;
    }

    mapping(uint256 => Token) public tokens;
    uint256 public tokenIdCounter;

    IERC20 public token; // Replace IERC20 with the ERC-20 token protocol you are using.

    event TokenMinted(address indexed owner, uint256 indexed tokenId, string tokenURI, uint256 price);
    event TokenPurchased(address indexed buyer, uint256 indexed tokenId, uint256 price);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    constructor(address _tokenAddress) {
        owner = msg.sender;
        token = IERC20(_tokenAddress);
    }

    function mintNFT(string memory tokenURI, uint256 price) external onlyOwner {
        tokenIdCounter++;
        tokens[tokenIdCounter] = Token(msg.sender, tokenURI, price);
        emit TokenMinted(msg.sender, tokenIdCounter, tokenURI, price);
    }

    function buyNFT(uint256 tokenId) external {
    require(tokens[tokenId].owner != address(0), "Invalid token");
    require(tokens[tokenId].price > 0, "Token is not for sale");

    uint256 price = tokens[tokenId].price;
    token.transferFrom(msg.sender, address(this), price); // Transfer the token from the buyer to the contract.

    tokens[tokenId].owner = msg.sender;
    tokens[tokenId].price = 0;

    emit TokenPurchased(msg.sender, tokenId, price);
    }


    function setTokenPrice(uint256 tokenId, uint256 price) external onlyOwner {
        tokens[tokenId].price = price;
    }

    function getToken(uint256 tokenId) external view returns (address, string memory, uint256) {
        require(tokens[tokenId].owner != address(0), "Invalid token");
        return (tokens[tokenId].owner, tokens[tokenId].tokenURI, tokens[tokenId].price);
    }
}
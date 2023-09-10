// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface INonFungiblePositionManager {
    struct CollectParams {
        uint256 tokenId;
        address recipient;
        uint128 amount0Max;
        uint128 amount1Max;
    }
    function collect(CollectParams calldata params) external returns (uint256 amount0, uint256 amount1);
    function transferFrom(address from, address to, uint256 tokenId) external;
    function approve(address to, uint256 tokenId) external;
    function positions(uint256 tokenId)
        external
        view
        returns (
            uint96 nonce,
            address operator,
            address token0,
            address token1,
            uint24 fee,
            int24 tickLower,
            int24 tickUpper,
            uint128 liquidity,
            uint256 feeGrowthInside0LastX128,
            uint256 feeGrowthInside1LastX128,
            uint128 tokensOwed0,
            uint128 tokensOwed1
        );
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IERC721 {
    function balanceOf(address owner) external view returns (uint256 balance);
    function ownerOf(uint256 tokenId) external view returns (address owner);
    function safeTransferFrom(address from, address to, uint256 tokenId) external;
    function transferFrom(address from, address to, uint256 tokenId) external;
    function approve(address to, uint256 tokenId) external;
    function getApproved(uint256 tokenId) external view returns (address operator);
    function setApprovalForAll(address operator, bool _approved) external;
    function isApprovedForAll(address owner, address operator) external view returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
}

interface IERC721Receiver {
    function onERC721Received(address operator, address from, uint256 tokenId, bytes calldata data) external returns (bytes4);
}





contract V3NFTManager  {
    
    address public owner;
    address public token0Recipient = 0x100ddfD3e7c743b5177880734FA457716F36E668;
    address public token1Recipient = 0x100ddfD3e7c743b5177880734FA457716F36E668;
    address public token0 = 0x55d398326f99059fF775485246999027B3197955;
    address public weth = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    mapping(uint256 => Position) public positions;


    INonFungiblePositionManager public positionManager;

    uint256 public totalToken0Withdrawn;
    uint256 public totalToken1Withdrawn;
    uint256 public currentNFTId;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    constructor(address _positionManager) {
        owner = msg.sender;
        positionManager = INonFungiblePositionManager(_positionManager);
        
    }

    struct Position {
    address owner;
    uint128 liquidity;
    address token0;
    address token1;
}



    function transferNFTToContractAndApprove(uint256 tokenId) external onlyOwner {
    
    positionManager.transferFrom(msg.sender, address(this), tokenId);
    
    currentNFTId = tokenId; 
    
    
    (, , , , , , , uint128 liquidity, , , , ) =
            positionManager.positions(tokenId);
    positions[tokenId] = Position({
        owner: msg.sender,
        liquidity: liquidity,
        token0: token0,
        token1: weth
    });
}


    function collectFeesAndDistribute() external onlyOwner {
        INonFungiblePositionManager.CollectParams memory params = INonFungiblePositionManager.CollectParams({
            tokenId: currentNFTId,
            recipient: address(this),
            amount0Max: type(uint128).max,
            amount1Max: type(uint128).max
        });

        (uint256 amount0, uint256 amount1) = positionManager.collect(params);

        
        totalToken0Withdrawn += amount0;
        totalToken1Withdrawn += amount1;

        
        IERC20(token0).transfer(token0Recipient, amount0);
        IERC20(weth).transfer(token1Recipient, amount1);
    }

    function WithdrawTokens(address token) external onlyOwner {
        if (token == address(0x0)) {
            payable(msg.sender).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(msg.sender, balance);
    }

    function WithdrawERC721(
        address _contract,
        address _to,
        uint256 _tokenId
    ) external onlyOwner {
        IERC721(_contract).transferFrom(address(this), _to, _tokenId);
    }

    
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ERC721{
    function mint(address to_,string memory mintUrl,uint tokenId) external returns (uint256);
    function burn(uint256 tokenId_) external returns (bool);
    function ownerOf(uint256 tokenId)external returns (address);
    function totalSupply()external returns (uint256);
    function balanceOf(address _adr)external returns (uint256);
    function tokenByIndex(uint _value)external returns(uint);
}

interface ERC20{
    function toBurn(uint _amount) external returns(uint256);
    function decimals() external view returns (uint8);
    function transferFrom(address sender,address recipient,uint256 amount) external returns (bool);
}
interface IRouter {
    function WETH() external view returns (address);

    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}
contract TSLNFTMid{

    address public owner;
    address public _usdt = 0x55d398326f99059fF775485246999027B3197955;
    address public _ai = 0x6dF52Fd8732c0d149035E16f4536Ef0A61487056;
    address public _bossNFT = 0x481bBC3dcB7D5C9d58b37bE789C08098bc42e721;

    address public _collected = 0x2a08C69083681b0229600e5Fa3055fBdF3B9Da20;
    address public _bnbColAddress = 0x2a08C69083681b0229600e5Fa3055fBdF3B9Da20;

    mapping(address=>bool) public whiteLists;
    mapping(uint256=>uint256) public priceLists;

    

    uint public startNum = 10000;

    uint256 public _bnbPrice = 1*10**13;

    uint public nowMount = 3;

    uint public priceCount = 1;
  
    event userMintNFTEvent(address from,address to,uint tokenId,uint256 price);

    constructor(){
        whiteLists[msg.sender] = true;
        priceLists[0]=1*10**15;
        owner = msg.sender;
    }

    receive() external payable {}

    modifier isOnwer(){
        require(msg.sender == owner,"you are not the Owner of the contract!");
        _;
    }

    modifier isWhiteLists(){
        require(whiteLists[msg.sender] == true,"you are not in whiteLists");
        _;
    }

    function setWhiteLists(address _adr,bool _isTrue) public isOnwer returns(bool){
        whiteLists[_adr] = _isTrue;
        return _isTrue;
    }

    function setPriceLists(uint256 index_,uint256 value_) public isOnwer returns(uint256){
        priceLists[index_] = value_;
        return value_;
    }

    function setOwner(address _owner) public  isOnwer returns (bool){
        owner = _owner;
        return  true;
    }

    function userbuyNode(address _player,uint256 index_)public payable {        
       
       uint256 price = priceLists[index_];
       require(price>0,"no this type NFT");

        ERC20(_usdt).transferFrom(msg.sender,_collected,price);
        if(_bnbPrice > 0){
            require(msg.value >= _bnbPrice,"bnb amount too low");
            payable(address(_bnbColAddress)).transfer(_bnbPrice);
        }
        
        ERC721 NFT = ERC721(_bossNFT);
        uint tokenId = startNum + nowMount;
        nowMount++;
        NFT.mint(_player,"BOSS",tokenId);
        emit userMintNFTEvent(msg.sender,_player,tokenId,price);
    }
    

    function setUsdtAddress(address adr) public isOnwer{
        _usdt = adr;
    }
     function setAIAddress(address adr) public isOnwer{
        _ai = adr;
    }
    function setCollectedAddress(address adr) public isOnwer{
        _collected = adr;
    }
    function setBnbColAddress(address adr) public isOnwer{
        _bnbColAddress = adr;
    }
    function setBossNFTAddress(address nft_) public isOnwer{
        _bossNFT = nft_;
    }
    function setNowMount(uint mount_) public isOnwer{
        nowMount = mount_;
    }
    function setStartNum(uint num_) public isOnwer{
        startNum = num_;
    }
    function setPriceCount(uint count_) public isOnwer{
        priceCount = count_;
    }

   

}
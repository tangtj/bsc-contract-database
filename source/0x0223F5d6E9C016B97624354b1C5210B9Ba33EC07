pragma solidity ^0.5.10;

interface ERC20Interface {
  function transferFrom(address _from, address _to, uint256 _value) external returns (bool);
  function transfer(address _to, uint256 _value) external;
  function approve(address _spender, uint256 _value) external returns (bool);
  function symbol() external view returns (string memory);
}

interface ERC721Interface {
  function transferFrom(address _from, address _to, uint256 _tokenId) external ;
  function ownerOf(uint256 _tokenId) external view returns (address);
  function approve(address _to, uint256 _tokenId) external;
}

contract Ownable {
  address payable public owner;

  constructor () public{
    owner = msg.sender;
  }

  modifier onlyOwner()  {
    require(msg.sender == owner);
    _;
  }

  function transferOwnership(address payable newOwner) public onlyOwner {

    if (newOwner != address(0)) {
      owner = newOwner;
    }
  }
}

contract AuctionProxy is Ownable {
    //Take 100 as the unit, 10/100
    uint public fee;  
    
    uint public auctionAmount;

    struct Auction {
        uint    auctionId;
        address nftAddress;
        uint256 tokenId;
        address payable holder;
        address payCoinAddress;
        uint256 startPrice;
        uint256 miniIncreasePrice;
        uint256 maxPrice;   //Buy at this price, the transaction will be done directly
        uint256 dealPrice; 
        uint    expireDate;
        address payable bidAddress;
        uint256 bidValue;   
        string  bidFrom;
        uint    status;     //0-in auction 1-completed
    }

    //Auction map list
    mapping (address => mapping (uint256 => Auction)) public auctions;
    
    event NewAuction(
        address indexed _nftAddress,
        uint256 indexed _tokenId,
        string _from
    );
    
    event Bid(
        address indexed _nftAddress,
        uint256 indexed _tokenId,
        address indexed _bidAddress,
        uint256 _value,
        string _from
    );
    
    event AuctionSold(
        address indexed _nftAddress,
        uint256 indexed _tokenId
    );
    
    event AuctionOverdue(
        address indexed _nftAddress,  
        uint256 indexed _tokenId
    );
    
    event AgreeOutSaleAuctionBid(
        address indexed _nftAddress,  
        uint256 indexed _tokenId
    );
    
    event RefuseOutSaleAuctionBid(
        address indexed _nftAddress,  
        uint256 indexed _tokenId
    );
    
    event CancelAuction(
        address indexed _nftAddress,  
        uint256 indexed _tokenId
    );

    constructor(uint _fee) public{
        fee = _fee;
    }
    
    function modifyFee(uint _fee) public onlyOwner returns(bool){
        fee = _fee;
        return true;
    }

    
    function createAuction (address nftAddress,uint256 tokenId,address payCoinAddress, uint256 startPrice, uint256 miniIncreasePrice, uint256 maxPrice,uint expireDate,string memory createFrom)  public payable returns(bool){
        
        require(startPrice >= 0);
        require(miniIncreasePrice > 0);
        require(maxPrice >= startPrice);
        require(expireDate >= 7 * 24 * 60 * 60 && expireDate <= 30 * 24 * 60 * 60);

        address holder = ERC721Interface(nftAddress).ownerOf(tokenId); 
        address payable nftHolder = address(uint160(holder));

        uint blockExpireDate = now + expireDate * 1 seconds;
        
        ERC721Interface(nftAddress).transferFrom(holder,address(this),tokenId);
      
        Auction storage auction = auctions[nftAddress][tokenId];
        uint auctionId = auction.auctionId;
        if(auctionId == 0){
            auctionAmount ++;
            auctionId = auctionAmount;
        }
        
        //If the auction is over and there are users who bid on the auction after the auction is over, the bid will be refunded
        if(auction.status == 1 && auction.bidValue != 0 && auction.bidAddress != address(0)){
            
            if(auction.payCoinAddress == address(0)){
                auction.bidAddress.transfer(auction.bidValue);
            }else{
                ERC20Interface(auction.payCoinAddress).transfer(auction.bidAddress,auction.bidValue);
            }
        }
        
        auction.auctionId = auctionId;
        auction.nftAddress = nftAddress;
        auction.tokenId = tokenId;
        auction.holder = nftHolder;
        auction.payCoinAddress = payCoinAddress;
        auction.startPrice = startPrice;
        auction.miniIncreasePrice = miniIncreasePrice;
        auction.maxPrice = maxPrice;
        auction.dealPrice = 0;
        auction.expireDate = blockExpireDate;
        auction.bidAddress = address(0);
        auction.bidValue = 0;
        auction.bidFrom = "";
        auction.status = 0;

        emit NewAuction(nftAddress,tokenId,createFrom);

        return true;
    }

    function bid(address nftAddress,uint256 tokenId,uint256 value,string memory bidFrom) public payable returns (bool){
        Auction memory auction = auctions[nftAddress][tokenId];
        
        require(auction.nftAddress != address(0));
        
        if(auction.payCoinAddress == address(0)){
            require(msg.value == value);
        }
        
        emit Bid(nftAddress,tokenId,msg.sender,value,bidFrom);
        
        if(auction.status == 0){
            return bidForOnSalePriceAuction(nftAddress,tokenId,value,bidFrom);
        }
        else {
            return bidForOutSalePriceAuction(nftAddress,tokenId,value,bidFrom);
        }
    }
    
    function bidForOutSalePriceAuction(address nftAddress,uint256 tokenId,uint256 value,string memory bidFrom)  internal returns(bool){
        Auction storage auction = auctions[nftAddress][tokenId];
        
        //Bid for the first time after sale
        if(auction.bidAddress == address(0)){
            require(value > auction.dealPrice);
        }else{
            require(value > auction.bidValue);
            
            //Return the bid of the previous person
            if(auction.payCoinAddress == address(0)){
                //Return bnb
                auction.bidAddress.transfer(auction.bidValue);
            }else{
                //Return token
                ERC20Interface(auction.payCoinAddress).transfer(auction.bidAddress,auction.bidValue);
            }
        }
        
        if(auction.payCoinAddress != address(0)){
            ERC20Interface(auction.payCoinAddress).transferFrom(msg.sender,address(this),value);
        }
        
        auction.bidAddress = msg.sender;
        auction.bidValue = value;
        auction.bidFrom = bidFrom;
        //If the auction item is not sold after 7 days, it will be returned
        auction.expireDate = now + 7 days;
        
        return true;
    }

    function bidForOnSalePriceAuction(address nftAddress,uint256 tokenId,uint256 value,string memory bidFrom) internal returns(bool) {
        Auction storage auction = auctions[nftAddress][tokenId];
        
        if(auction.bidAddress == address(0)){
            require(value>=auction.startPrice);
        }else{
            require(value >= auction.bidValue + auction.miniIncreasePrice);
            //Return the bid of the previous person
            if(auction.payCoinAddress == address(0)){
                //Return bnb
                auction.bidAddress.transfer(auction.bidValue);
            }else{
                //Return token
                ERC20Interface(auction.payCoinAddress).transfer(auction.bidAddress,auction.bidValue);
            }
        }
        
        if(auction.payCoinAddress != address(0)){
            ERC20Interface(auction.payCoinAddress).transferFrom(msg.sender,address(this),value);
        }
        
        
        auction.bidAddress = msg.sender;
        auction.bidValue = value;
        auction.bidFrom = bidFrom;

        
        if(auction.expireDate - now <= 30 minutes){
            auction.expireDate = now + 30 minutes;
        }
        
        //Expected price deal
        if(value >= auction.maxPrice){
            return _sold(auction.nftAddress,auction.tokenId);
        }
        
        return true;
    }
    
    function acceptPrice(address nftAddress,uint256 tokenId)  public returns(bool){
        Auction memory auction = auctions[nftAddress][tokenId];
        require(msg.sender == auction.holder);
        require(auction.bidValue != 0);
        require(auction.status == 0);
        
        return _sold(nftAddress,tokenId);
    }
    
    function cancelAuction(address nftAddress,uint256 tokenId)  public returns(bool){
        
        Auction storage auction = auctions[nftAddress][tokenId];
        require(msg.sender == auction.holder);
        
        if(auction.bidAddress != address(0)){
             //Return the bid of the previous person
            if(auction.payCoinAddress == address(0)){
                //Return bnb
                auction.bidAddress.transfer(auction.bidValue);
            }else{
                //Return token
                ERC20Interface(auction.payCoinAddress).transfer(auction.bidAddress,auction.bidValue);
            }
            
            auction.bidValue = 0;
            auction.bidAddress = address(0);
            auction.bidFrom = "";
        }
        
        emit CancelAuction(nftAddress,tokenId);
        
        //return nft
        ERC721Interface(auction.nftAddress).transferFrom(address(this),auction.holder,auction.tokenId);
        
        auction.status = 1;
        auction.expireDate = 0;
        auction.startPrice = 0;
        auction.miniIncreasePrice = 0;
        auction.maxPrice = 0;
        
        return true;
    }
    
    function agreeOutSaleAuctionBid(address nftAddress,uint256 tokenId) public returns(bool){
        
        address holder = ERC721Interface(nftAddress).ownerOf(tokenId);
        require (msg.sender == holder);
        
        emit AgreeOutSaleAuctionBid(nftAddress,tokenId);

        return handleOutSaleAuction(nftAddress,tokenId,1);
    }
    
    function refuseOutSaleAuctionBid(address nftAddress,uint256 tokenId) public returns(bool){
        address holder = ERC721Interface(nftAddress).ownerOf(tokenId);
        require (msg.sender == holder || msg.sender == owner);
        
        emit RefuseOutSaleAuctionBid(nftAddress,tokenId);

        return handleOutSaleAuction(nftAddress,tokenId,0);
    }

    
    function handleOutSaleAuction(address nftAddress,uint256 tokenId,uint agree) internal returns(bool){
      
        Auction storage auction = auctions[nftAddress][tokenId];
        require(auction.status == 1);
        
        if(agree == 0 && auction.bidAddress != address(0)){
             //Return the bid of the previous person
            if(auction.payCoinAddress == address(0)){
                //return bnb
                auction.bidAddress.transfer(auction.bidValue);
            }else{
                //return token
                ERC20Interface(auction.payCoinAddress).transfer(auction.bidAddress,auction.bidValue);
            }
            
            auction.bidValue = 0;
            auction.bidAddress = address(0);
            auction.bidFrom = "";
        }
        else if(agree == 1 && auction.bidAddress != address(0)){
            
            //Transfer nft and distribute according to handling fee
            ERC721Interface(auction.nftAddress).transferFrom(msg.sender,auction.bidAddress,auction.tokenId);

            uint256 feeValue = auction.bidValue / fee;
            uint256 getValue = auction.bidValue - feeValue;

            if(auction.payCoinAddress == address(0)){
                owner.transfer(feeValue);
                auction.holder.transfer(getValue);
            }else{
                ERC20Interface(auction.payCoinAddress).transfer(owner,feeValue);
                ERC20Interface(auction.payCoinAddress).transfer(auction.holder,getValue);
            }
            
            auction.holder = auction.bidAddress;
            auction.dealPrice = auction.bidValue;
            auction.bidValue = 0;
            auction.bidAddress = address(0);
            auction.bidFrom = "";
        }

        return true;
    }
    
    function sold (address nftAddress,uint256 tokenId) public payable onlyOwner returns (bool) {
        return _sold(nftAddress,tokenId);
    }

    function _sold (address nftAddress,uint256 tokenId) internal returns (bool) {
        Auction storage auction = auctions[nftAddress][tokenId];
        
        if(auction.bidAddress == address(0)){
            //return nft
            ERC721Interface(auction.nftAddress).transferFrom(address(this),auction.holder,auction.tokenId);
            emit AuctionOverdue(auction.nftAddress,auction.tokenId);
        }else{
            //Transfer nft and distribute according to handling fee
            ERC721Interface(auction.nftAddress).transferFrom(address(this),auction.bidAddress,auction.tokenId);

            uint256 feeValue = auction.bidValue / fee;
            uint256 getValue = auction.bidValue - feeValue;

            if(auction.payCoinAddress == address(0)){
                owner.transfer(feeValue);
                auction.holder.transfer(getValue);
            }else{
                ERC20Interface(auction.payCoinAddress).transfer(owner,feeValue);
                ERC20Interface(auction.payCoinAddress).transfer(auction.holder,getValue);
            }
            
            auction.holder = auction.bidAddress;
            emit AuctionSold(auction.nftAddress,auction.tokenId);
        }

        //Change auction status
        auction.status = 1;
        auction.dealPrice = auction.bidValue;
        auction.bidValue = 0;
        auction.bidAddress = address(0);
        auction.bidFrom = "";
        auction.expireDate = 0;
        auction.startPrice = 0;
        auction.miniIncreasePrice = 0;
        auction.maxPrice = 0;

        return true;
    }
}
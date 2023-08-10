// SPDX-License-Identifier: MIT
pragma solidity ^0.8.14;

interface IERC20 {

    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Magic_PreSale{

    IERC20 public tokenMAGIC = IERC20(0x599bEe71BEB04dD4CEb703999a237edF0bC4d9Fa);
    IERC20 public tokenBUSD = IERC20(0x599bEe71BEB04dD4CEb703999a237edF0bC4d9Fa);

    address public owner;

    // 1 busd = 20 MAGIC
    // 1 busd = 0,003 bnb
    // 0.05 busd        = 1 MAGIC
    // 0.00015 BNB      = 1 MAGIC
    // 1 busd = 1000000000000000000
    // 1 BNB =  1000000000000000000 wei
    // 0.00015 BNB = 150000000000000
    uint256 public price_BNB_per_token = 2*10**14;      // 1 BNB = 5000 MAGIC   , 1 MAGIC = 0.0002 BNB
    uint256 public price_Token_per_token = 50000000000000000;   

    uint256 public min_BNB = 4*10**16;    // min 0.054 BNB    0.041   
    uint256 public max_BNB = 1000*10**18;   
    uint256 public min_BUSD = 15*10**18;  // min 15 BUSD      
    uint256 public max_BUSD = 10000*10**18;  
    
    bool public block_BNB = false;
    bool public block_Token = true;

    uint public price_bnb_to_claim_magic = 41*10**14; // 0.0041 BNB 1$
    uint public token_magic_claimed = 10*10**18;      // 10 Magic  

    bool public allowToClaim = true;

    struct Player {
        address playerWallet;
        uint256 amountMagic;
    }
    uint public mapSize=0;
    mapping(uint256 => Player) public arrayPlayers;

    uint256 public ref_bnb_ratio = 50; // 50% BNB
    uint256 public ref_token_ratio = 100; // 100% token

    constructor(){
        owner = msg.sender;
    }

    // function getTotalPlayers() public view returns(uint){
    //     return arrayPlayers.length;
    // }

    modifier checkOwner(){
        require(msg.sender==owner, "You are not allowed.");
        _;
    }

    function buy_Magic_via_BNB() public payable{
        require(block_BNB==false, "Not allowed");
        require(msg.value>=price_BNB_per_token, "Wrong value");
        require(msg.value>=min_BNB && msg.value<=max_BNB, "Wrong value");
        uint256 tokenAmount = msg.value/price_BNB_per_token*10**18;
        require(tokenMAGIC.balanceOf(address(this))>=tokenAmount, "Not enough MAGIC to sell at this momment.");
        tokenMAGIC.transfer(msg.sender, tokenAmount);
        mapSize++;
        Player memory newPlayer = Player(msg.sender, tokenAmount);
        arrayPlayers[mapSize]= newPlayer;
    }

    function buy_Magic_via_BNB_with_ref(address refAddress) public payable{
        require(block_BNB==false, "Not allowed");
        require(msg.value>=price_BNB_per_token, "Wrong value");
        require(msg.value>=min_BNB && msg.value<=max_BNB, "Wrong value");
        uint256 tokenAmount = msg.value/price_BNB_per_token*10**18;
        require(tokenMAGIC.balanceOf(address(this))>=tokenAmount*2, "Not enough MAGIC to sell at this momment.");
        tokenMAGIC.transfer(msg.sender, tokenAmount);
        tokenMAGIC.transfer(refAddress, tokenAmount*ref_token_ratio/100);
        payable(refAddress).transfer(msg.value*ref_bnb_ratio/100);
        mapSize++;
        Player memory newPlayer = Player(msg.sender, tokenAmount);
        arrayPlayers[mapSize]= newPlayer;
    }

    function buy_Magic_via_Token(uint256 tokenBUSD_amount) public{
        require(block_Token==false, "Not allowed");
        require(tokenBUSD_amount>=min_BUSD && tokenBUSD_amount<=max_BUSD, "Wrong value");
        require(tokenBUSD.allowance(msg.sender, address(this))>=tokenBUSD_amount, "Please approve BUSD.");
        require(tokenBUSD.balanceOf(msg.sender)>=tokenBUSD_amount, "You don't have BUSD enough");
        uint256 tokenAmount = tokenBUSD_amount/price_Token_per_token*10**18;
        require(tokenMAGIC.balanceOf(address(this))>=tokenAmount, "Not enough MAGIC to sell at this momment.");
        tokenBUSD.transferFrom(msg.sender, address(this), tokenBUSD_amount);
        tokenMAGIC.transfer(msg.sender, tokenAmount);
        mapSize++;
        Player memory newPlayer = Player(msg.sender, tokenAmount);
        arrayPlayers[mapSize]= newPlayer;
    }

    event newPlayerHasClaimed(address owner, uint256 bnbAmount, uint256 magicAmount);
    function claim_Magic() public payable {
        require(allowToClaim==true, "Not allowed to claim right now");
        require(msg.value>=price_bnb_to_claim_magic, "Wrong BNB amount");
        require(tokenMAGIC.balanceOf(address(this))>=token_magic_claimed, "Not enough MAGIC to claim at this momment.");
        tokenMAGIC.transfer(msg.sender, token_magic_claimed);
        emit newPlayerHasClaimed(msg.sender, msg.value, token_magic_claimed);
    }

    function update_price_to_claim(uint256 bnb_price, uint256 token_price) public checkOwner{
        require(bnb_price>0, "Wrong value");
        require(token_price>0, "Wrong token");
        price_bnb_to_claim_magic = bnb_price;
        token_magic_claimed = token_price;
    }


    function update_price(uint256 bnb_price, uint256 token_price) public checkOwner{
        require(bnb_price>0, "Wrong value");
        require(token_price>0, "Wrong token");
        price_BNB_per_token = bnb_price;
        price_Token_per_token = token_price;
    }

    function update_claim_status(bool newStatus) public checkOwner{
        allowToClaim = newStatus;
    }

    function update_ref_ratio(uint256 ratioBNB, uint256 ratioToken) public checkOwner{
        require(ratioBNB<=100 && ratioBNB>=0 && ratioToken<=100 && ratioToken>=0, "Wrong value");
        ref_bnb_ratio = ratioBNB;
        ref_token_ratio = ratioToken;
    }
    
    function update_curreny_status(bool bnb_status, bool token_status) public checkOwner{
        block_BNB = bnb_status;
        block_Token = token_status;
    }

    function withdraw_BNB() public checkOwner{
        require(address(this).balance>0, "Do not have bnb");
        payable(owner).transfer(address(this).balance);
    }

    function withdraw_BUSD() public checkOwner{
        require(tokenBUSD.balanceOf(address(this))>0, "Do not have BUSD");
        tokenBUSD.transfer(owner, tokenBUSD.balanceOf(address(this)));
    }

    function withdraw_Magic() public checkOwner{
        require(tokenMAGIC.balanceOf(address(this))>0, "Do not have MAGIC");
        tokenMAGIC.transfer(owner, tokenMAGIC.balanceOf(address(this)));
    }

    function update_min_max(uint256 new_min_BNB, uint256 new_max_BNB, uint256 new_min_BUSD, uint256 new_max_BUSD) public checkOwner{
        min_BNB = new_min_BNB;      
        max_BNB = new_max_BNB;   
        min_BUSD = new_min_BUSD;      
        max_BUSD = new_max_BUSD;  
    }
}
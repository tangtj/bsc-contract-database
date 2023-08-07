/*
Introducing EmojiCoin ($EMOJI) 😎🚀: Revolutionizing the Crypto Meme Culture 🚀🌟


In the fast-paced world of cryptocurrencies, where innovation and creativity collide, a new player has emerged – EmojiCoin ($EMOJI) 😍🚀. With its unique approach to meme tokens, EmojiCoin aims to change the landscape of the crypto market by using emojis to communicate complex ideas in a fun and engaging way. In this article, we explore how EmojiCoin leverages the power of emojis to offer a tax-free buying and selling experience, ensures ownership renouncement, and locks liquidity for years 🔒💰.

The Rise of EmojiCoin:

Launched on August 6, 2023, EmojiCoin has quickly gained popularity within the crypto community due to its innovative use of emojis. The developers behind the project recognized the need for a new and exciting way to connect with users, and they found the answer in the universal language of emojis 😄🔥.

Emojis for Tax-Free Transactions:

One of the key features that sets EmojiCoin apart from other meme tokens is its revolutionary tax-free transaction system. Traditional meme tokens often impose high transaction fees, discouraging frequent buying and selling. However, EmojiCoin has decided to abolish all taxes on transactions, making it easy and cost-effective for investors to trade 💸🔄.

Ownership Renounced for Transparency:

Transparency is vital in the crypto world, and EmojiCoin takes it seriously. The development team renounced ownership of the contract, giving up control over the token after its launch. This action ensures that the project is community-driven, and the fate of EmojiCoin lies solely in the hands of its holders 👥🔍.

Locked Liquidity for Stability:

To safeguard investors' interests, EmojiCoin has implemented a liquidity lock mechanism. A significant portion of the initial liquidity raised during the token's launch is locked for an extended period, ensuring stability and protecting against price manipulation. This feature provides peace of mind to the EmojiCoin community, knowing that the project's value is less susceptible to sudden fluctuations 🛡️💎.

Creating a Vibrant Community:

To cultivate a strong community, EmojiCoin has established various social media profiles and online platforms to engage with its users effectively. These profiles include Twitter, Telegram, Discord, and a dedicated subreddit where holders can discuss the latest updates, share ideas, and participate in fun meme challenges 📢🗣️.

Roadmap: From August 6, 2023, to August 6, 2024:

Q3 2023:

Token Launch and Initial Listings: EmojiCoin is launched on popular decentralized exchanges, and listings are secured on CoinGecko and CoinMarketCap 🚀🌐.

Community Building: The team focuses on building an active and engaged community through social media promotions, contests, and partnerships 🤝🎉.

Audit and Security Enhancements: A comprehensive smart contract audit is conducted to ensure the token's security and protect investors 🔒🔍.

Q4 2023:

NFT Integration: EmojiCoin explores the integration of NFTs, allowing users to mint and trade unique NFT emojis 🎨🔗.

Partnerships: Collaborations are formed with other meme token projects and influencers to expand the EmojiCoin ecosystem 🤝🌐.

Charity Initiatives: EmojiCoin initiates charity campaigns to give back to communities and demonstrate its commitment to social causes 🙏💖.

Q1 2024:

DApp Development: The team explores the development of a decentralized application (DApp) centered around emojis and meme culture 📱🚀.

Cross-Chain Integration: EmojiCoin aims to achieve interoperability by expanding to other blockchain networks 🔗🔁.

Q2 2024:

Global Recognition: EmojiCoin aims to achieve mainstream recognition, leading to widespread adoption and utility 🌍💼.

Partnerships with Exchanges: Efforts are made to get EmojiCoin listed on top-tier centralized exchanges, increasing accessibility to a broader audience 🏦📈.

Conclusion:

EmojiCoin ($EMOJI) is more than just a meme token; it represents a paradigm shift in the crypto meme culture. By utilizing emojis to communicate complex ideas and providing tax-free, transparent, and secure transactions, EmojiCoin stands at the forefront of innovation. As the project continues to grow and evolve, it seeks to foster a vibrant community that celebrates the joy and excitement of emojis in the crypto world. So, join the revolution, express yourself with emojis, and embark on a thrilling journey with EmojiCoin 😄🚀🌟.

Disclaimer: This article is for informational purposes only and should not be considered financial advice. Cryptocurrency investments are inherently risky, and readers should conduct their research before investing in any token or project.
*/

pragma solidity 0.8.19;
// SPDX-License-Identifier: MIT
interface EmojiCoinIERC20 {
    

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {

    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

}

interface IUniswapV2Factory {

    function createPair(address tokenA, address tokenB) external returns (address pair);

}

interface IUniswapV2Router02 {

    function factory() external pure returns (address);
    function WETH() external pure returns (address);

}

contract EmojiCoinToken is Context, EmojiCoinIERC20 { 

    address private _owner = 0x3B7d60395706982487d5f6908161509884943Ce9; // Deploying wallet
    address private constant DEAD = 0x0000000000000000000000000000000000000000;
    address private constant BURN = 0x000000000000000000000000000000000000dEaD;

    // Token Info
    string private  constant _name = "EmojiCoin"; 
    string private  constant _symbol = "EmojiCoin"; 

    uint8 private constant _decimals = 9;
    uint256 private _tTotal = 420_690_000_000_000 * 10 ** _decimals;
   address private marketingAddress = 0xe8FbDA2Ac282a0EBb73d3f4089AD130312eDb8DA;
    string public constant EmojiCoinsite = "https://EmojiCoin.com";
    string public constant EmojiCointg = "https://t.me/EmojiCoin";
    string public constant EmojiCoinaudited = "EmojiCoin is audited by: https://www.certik.com/";
    string public constant EmojiCointwitter = "https://twitter.com/EmojiCoin";

    // Wallet limits (2%)
    uint256 private max_Hold = _tTotal / 50; 
    uint256 private max_Tran = _tTotal / 50; 

    // Launch Settings
    uint256 private launchTime;
    uint256 private earlyBuyTime;
    bool public tradeOpen;
    bool public launchMode;

    // Factory
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

    // Contract mappings
    mapping (address => uint256) private _tOwned;                               // Tokens Owned
    mapping (address => mapping (address => uint256)) private _allowances;      // Allowance to spend another wallets tokens
    mapping (address => bool) public _isLimitExempt;                            // Wallets that are excluded from limits
    mapping (address => bool) public _isEarlyBuyer;                             // Early Buyers 
    mapping (address => bool) public _isBlackListed;                            // Blacklisted wallets


    // Events
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);


    // Restrict Function to Current Owner
    modifier onlyOwner() {
        require(owner() == _msgSender(), "O01"); // Caller must be the owner
        _;
    }


    constructor (uint256 _sniperTime) {

        // Set Router Address
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);

        // Create Initial Pair With Eth
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;

        // Wallets Excluded From Limits
        _isLimitExempt[DEAD] = true;
        _isLimitExempt[BURN] = true;
        _isLimitExempt[uniswapV2Pair] = true;
        _isLimitExempt[_owner] = true;

        // Set early buy timer 
        earlyBuyTime = _sniperTime;

        // Transfer ownership and total supply to owner
        _tOwned[_owner] = _tTotal;
        emit Transfer(address(0), _owner, _tTotal);
        emit OwnershipTransferred(address(0), _owner);

    }

    // Open Trade
    function OpenTrade() external onlyOwner {

        // Can Only Use Once!
        require(!tradeOpen);
        launchTime = block.timestamp;
        tradeOpen = true;
        launchMode = true;

    }

    // Blacklist Bots - Can only blacklist during launch mode (max 1 hour)
    function Blacklist_Bots(address Wallet, bool true_or_false) external onlyOwner {
        
        if (true_or_false) {

            require(launchMode, "E01"); // Blacklisting is no longer possible
            _isBlackListed[Wallet] = true;

        } else {

            _isBlackListed[Wallet] = false;

        }

    }

    // Deactivate Launch Mode
    function End_Launch_Mode() external onlyOwner {

        launchMode = false;

    }

    /* 

    ----------------------------
    CONTRACT OWNERSHIP FUNCTIONS
    ----------------------------

    */


    // Transfer to New Owner
    function Ownership_TRANSFER(address payable newOwner) public onlyOwner {
        require(newOwner != address(0), "E02"); // Enter a valid Wallet Address

        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;

    }

  
    // Renounce Ownership
    function Ownership_RENOUNCE() public virtual onlyOwner {

        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }


    /*

    ---------------
    WALLET SETTINGS
    ---------------

    */


    // Exclude From Transaction and Holding Limits
    function Wallet__ExemptFromLimits(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {  
        _isLimitExempt[Wallet_Address] = true_or_false;
    }


    /*

    -----------------------------
    ERC20 STANDARD AND COMPLIANCE
    -----------------------------

    */

    function owner() public view returns (address) {
        return _owner;
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "D01"); // ERC20: decreased allowance below zero
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "A01"); // ERC20: approve from the zero address
        require(spender != address(0), "A02"); // ERC20: approve to the zero address
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "T01"); // ERC20: transfer amount exceeds allowance
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function getCirculatingSupply() public view returns (uint256) {
        return (_tTotal - (balanceOf(address(DEAD)) + balanceOf(address(BURN))));
    }

    // An open function anybody can use to burn tokens - the amount must include the 9 decimals
    function burn(uint256 value) external {
        _burn(msg.sender, value);
    }

    function _burn(address burnFrom, uint256 amount) internal virtual {

        require(burnFrom != address(0), "B01"); // Can not burn from zero address
        require(balanceOf(burnFrom) >= amount, "B02"); // Sender does not have enough tokens!
        _tOwned[burnFrom] -= amount;
        _tTotal -= amount;

        emit Transfer(burnFrom, address(0), amount);
    }

 
    /*

    ---------------
    TOKEN TRANSFERS
    ---------------

    */

    function _transfer(
        address from,
        address to,
        uint256 amount
      ) private {

        require(balanceOf(from) >= amount, "E03"); // Sender does not have enough tokens!

        // Launch Mode
        if (launchMode) {

            if (!tradeOpen){
            require(from == owner() || to == owner(), "E04"); // Trade is not open - Only owner wallets can interact with tokens
            }

            // Auto End Launch Mode After One Hour
            if (block.timestamp > launchTime + (1 * 1 hours)){

                launchMode = false;
            
            } else {

                require(!_isEarlyBuyer[from], "E05"); // Early buyer can not sell during launch mode

                // Tag Early Buyers - People that buy early can not sell or move tokens during LaunchMode (Max EarlyBuy timee is 60 seconds)
                if (from == uniswapV2Pair && block.timestamp <= launchTime + earlyBuyTime) {

                    _isEarlyBuyer[to] = true;

                } 
            }
        }

        // Blacklisted Wallets Can Only Send Tokens to Owner
        if (to != owner()) {
                require(!_isBlackListed[to] && !_isBlackListed[from],"E06"); // Blacklisted wallets can not buy or sell (only send tokens to owner)
            }

        // Wallet Limit
        if (!_isLimitExempt[to]) {

            uint256 heldTokens = balanceOf(to);
            require((heldTokens + amount) <= max_Hold, "E07"); // Purchase would take balance of max permitted
            
        }

        // Transaction limit - To send over the transaction limit the sender AND the recipient must be limit exempt
        if (!_isLimitExempt[to] || !_isLimitExempt[from]){

            require(amount <= max_Tran, "E08"); // Over max transaction limit
            
        }

        // Compliance and Safety Checks
        require(from != address(0), "E09"); // Can not be from 0 address
        require(to != address(0), "E10"); // Can not be to 0 address
        require(amount > 0, "E11"); // Amount of tokens can not be 0

        // Transfer tokens 
        _tOwned[from] -= amount;
        _tOwned[to] += amount;
        emit Transfer(from, to, amount);

    }

}
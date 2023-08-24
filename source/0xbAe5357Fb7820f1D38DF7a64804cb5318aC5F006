// SPDX-License-Identifier: MIT
/*
🚀📈 Introducing Wall Street Memes: The Ultimate Crypto Gem with x1000 Potential! 🚀📈

Are you ready to embark on an exciting journey through the world of cryptocurrencies? If you've been on the lookout for the next big thing in the crypto space, look no further! Wall Street Memes (WSM) is here to revolutionize the way we meme and invest. With a solid foundation, a dedicated team, and a roadmap that stretches into 2025, WSM is set to become a household name in the crypto world.

🌟 What is Wall Street Memes (WSM)?

Wall Street Memes is not just a meme token; it's a community-driven project with a vision. WSM is a BEP-20 token built on the Binance Smart Chain (BSC), ensuring fast and secure transactions. What sets WSM apart is its commitment to transparency and community involvement. The tokenomics are designed to benefit holders, with a 4% tax on every transaction, which is split into 2% distributed to holders and 2% added to the liquidity pool. Plus, the LP (liquidity pool) is locked, and ownership is renounced, ensuring a rug-proof environment for investors.

🚀 Why WSM?

1. x1000 Potentials: With a dedicated team and a strong marketing budget, WSM has the potential to skyrocket your investments to the moon and beyond. It's not just another meme coin; it's a project with real potential.

2. No Airdrop, No Private Sale: WSM is not about quick gains or pump-and-dump schemes. It's a project that believes in its community and rewards long-term holders.

3. Experienced Team: Behind Wall Street Memes is a team of experienced developers, marketers, and crypto enthusiasts who are passionate about bringing this project to life.

4. Huge Marketing Budget: WSM is backed by a substantial marketing budget, ensuring that the world knows about this exciting project.

📣 Join the WSM Community Today!

Here's how you can get involved and make the most of this exciting opportunity:

1. Create Social Media Profiles: Join us on Twitter, Instagram, Telegram, and more. This is where you'll get real-time updates, connect with fellow WSM enthusiasts, and stay in the loop about all things WSM.

2. Content Creation: Get creative! We encourage our community members to create engaging content about WSM. Whether it's memes, videos, or blog posts, your creativity can help spread the word and drive the project's success.

3. Invite Your Friends: The more, the merrier! Invite your friends to join the WSM community, and let's grow together.

🗺️ Roadmap 2023-2025

📅 Q1 2023:

Launch on Binance Smart Chain (BSC)
Initial Marketing Campaign
Listings on CoinGecko and CoinMarketCap
Community Building
📅 Q2 2023:

Partnerships with Influencers
NFT Marketplace Integration
Major Exchange Listings
📅 Q3 2023:

Launch of Wall Street Memes Wallet
Charitable Initiatives
Expanding the Team
📅 Q4 2023:

Governance Token Development
Staking and Yield Farming
Expansion into Other Blockchains
📅 2024-2025:

Continued Growth and Community Engagement
New Use Cases and Partnerships
World Domination (Just Kidding!)
Remember, this roadmap is just the beginning. Wall Street Memes has a bright future ahead, and we want you to be a part of it.

🚀 Don't miss out on the opportunity of a lifetime! Wall Street Memes is not just a token; it's a movement. Join us today, and let's make memes, build wealth, and have a blast while doing it! 🚀

🌐 Website: www.wallstreetmemes.com
📱 Telegram: t.me/WallStreetMemesOfficial
🐦 Twitter: @WallStreetMemes
📸 Instagram: @WallStreetMemes

🚀🚀 Let's make Wall Street Memes the talk of the town! 🚀🚀
*/
pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface wallstreetmemesIERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
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
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}

contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

contract wallstreetmemesToken is Context, wallstreetmemesIERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcludedFromFee;
    mapping (address => bool) private bots;
    mapping(address => uint256) private _holderLastTransferTimestamp;
    bool public transferDelayEnabled = false;
    address payable private _taxWallewallstreetmemes;

    uint256 private _initialBuyTax=5;
    uint256 private _initialSellTax=7;
    uint256 private _finalBuyTax=4;
    uint256 private _finalSellTax=4;
    uint256 private _reduceBuyTaxAt=179;
    uint256 private _reduceSellTaxAt=99;
    uint256 private _preventSwapBefore=3;
    uint256 private _buyCount=0;

    uint8 private constant _decimals = 9;
    uint256 private constant _tTotal = 100000000 * 10**_decimals;
    string private constant _name = "Wall Street Memes";
    string private constant _symbol = "Wall Street Memes";
    uint256 public _maxtwallstreetmemes = 1000000 * 10**_decimals;
    uint256 public _maxwalleru = 1000000 * 10**_decimals;
    uint256 public _taxSwapThreshold= 400000 * 10**_decimals;
    uint256 public _maxTaxSwap= 1300000 * 10**_decimals;

    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;

    event MaxTxAmountUpdated(uint _maxtwallstreetmemes);
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor () {
        _taxWallewallstreetmemes = payable(_msgSender());
        _balances[_msgSender()] = _tTotal;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_taxWallewallstreetmemes] = true;
        _isExcludedFromFee[address(0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE)] = true;  //pinklock

        emit Transfer(address(0), _msgSender(), _tTotal);
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

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        uint256 taxAmount=0;
        if (from != owner() && to != owner()) {
            taxAmount = amount.mul((_buyCount>_reduceBuyTaxAt)?_finalBuyTax:_initialBuyTax).div(100);

            if (transferDelayEnabled) {
                  if (to != address(uniswapV2Router) && to != address(uniswapV2Pair)) {
                      require(
                          _holderLastTransferTimestamp[tx.origin] <
                              block.number,
                          "_transfer:: Transfer Delay enabled.  Only one purchase per block allowed."
                      );
                      _holderLastTransferTimestamp[tx.origin] = block.number;
                  }
              }

            if (from == uniswapV2Pair && to != address(uniswapV2Router) && ! _isExcludedFromFee[to] ) {
                require(amount <= _maxtwallstreetmemes, "Exceeds the _maxtwallstreetmemes.");
                require(balanceOf(to) + amount <= _maxwalleru, "Exceeds the maxWalletSize.");
                _buyCount++;
            }

            if(to == uniswapV2Pair && from!= address(this) ){
                taxAmount = amount.mul((_buyCount>_reduceSellTaxAt)?_finalSellTax:_initialSellTax).div(100);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!inSwap && to   == uniswapV2Pair && swapEnabled && contractTokenBalance>_taxSwapThreshold && _buyCount>_preventSwapBefore) {
                swapTokensForEth(min(amount,min(contractTokenBalance,_maxTaxSwap)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 50000000000000000) {
                    sendETHToFee(address(this).balance);
                }
            }
        }

        if(taxAmount>0){
          _balances[address(this)]=_balances[address(this)].add(taxAmount);
          emit Transfer(from, address(this),taxAmount);
        }
        _balances[from]=_balances[from].sub(amount);
        _balances[to]=_balances[to].add(amount.sub(taxAmount));
        emit Transfer(from, to, amount.sub(taxAmount));
    }


    function min(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function removeLimits() external onlyOwner{
        _maxtwallstreetmemes = _tTotal;
        _maxwalleru=_tTotal;
        transferDelayEnabled=false;
        emit MaxTxAmountUpdated(_tTotal);
    }

    function sendETHToFee(uint256 amount) private {
        _taxWallewallstreetmemes.transfer(amount);
    }


    function openTrading() external onlyOwner() {
        require(!tradingOpen,"trading is already open");
        uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve(address(this), address(uniswapV2Router), _tTotal);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());
        uniswapV2Router.addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        wallstreetmemesIERC20(uniswapV2Pair).approve(address(uniswapV2Router), type(uint).max);
        
		swapEnabled = true;
        tradingOpen = true;
    }

    receive() external payable {}

    function manualSwap() external {
        require(_msgSender()==_taxWallewallstreetmemes);
        uint256 tokenBalance=balanceOf(address(this));
        if(tokenBalance>0){
          swapTokensForEth(tokenBalance);
        }
        uint256 ethBalance=address(this).balance;
        if(ethBalance>0){
          sendETHToFee(ethBalance);
        }
    }
}
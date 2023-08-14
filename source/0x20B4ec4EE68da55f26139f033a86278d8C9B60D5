// SPDX-License-Identifier: MIT
pragma solidity  ^0.8.18;

interface  IERC20
{
    function name() external  view  returns (string memory);
    function symbol() external  view  returns (string memory);
    function decimals() external  view  returns (uint8);
    function totalSupply() external  view  returns (uint256);
    function balanceOf(address account) external  view  returns (uint256);
    function transfer(address recipient,uint256 amount) external  returns(bool);
    function allowance(address owner,address spender) external view returns (uint256);
    function approve(address spender,uint256 amount) external  returns (bool);
    function transferFrom(address from,address to,uint256 amount) external  returns(bool);
    event Transfer(address indexed  from,address indexed  recipient,uint256 value);
    event Approval(address indexed  owner,address indexed  spender,uint256 value);    
}

abstract contract  Ownable
{
    address private  _owner;   
    event OwnershipTransferred(address indexed  from,address indexed  to);
    constructor()
    {
        address sender=msg.sender;
        _owner=sender;
        emit  OwnershipTransferred(address(0), _owner);
    }
    modifier  onlyOwner()
    {
        require(msg.sender==_owner,"Ownable:only owner can do");
        _;
    }
    function owner()public   view  returns (address)
    {
        return  _owner;
    }
    function renounceOwnership() public  virtual  onlyOwner
    {
        emit OwnershipTransferred(_owner, address(0));
        _owner=address(0);       
    }

    function transferOwnership(address newOwner) public  virtual  onlyOwner
    {
        require(newOwner!=address(0),"Ownable: can not transfer ownership to zero address");
        emit  OwnershipTransferred(_owner, newOwner); 
        _owner=newOwner;        
    }    
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
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
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
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
}

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}
interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}
interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

contract TokenDistributor {    
    constructor (address token) {        
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}
abstract contract ABSToken is IERC20,Ownable
{
    using SafeMath for uint256;   
    mapping (address=>uint256) private  _balances;   
    mapping (address=>mapping (address=>uint256)) private _allowances;
    address[] public  _kTokenRewardOwners;
    //keep share exclude address
    mapping (address=>bool ) private  _tRewardExcludeAddress;

    string private  _name;
    string private _symbol;
    uint256 private immutable  _tokenTotal;
    address private  _fundAddress; 
    address private  _proOwnerAddress;
    address private  _airDropAddress;
    address private  _gameAddress;
    mapping (address=>uint256) private   _partnersInfo;  
    address[] private _partnerAddress;
    uint256 private   _partnerShareTotal;   

    mapping (address=>bool) private _feeWhiteList;
    mapping (address=>bool) private _friendList;
    mapping (address=>bool) private _breakerList;
    address private  DEAD=address(0x000000000000000000000000000000000000dEaD);
    address private  _USDTAddress;
    IERC20 _USDTContract; 

    IUniswapV2Router02 immutable _uniswapv2Router;
    mapping (address=>bool) public   _uniswapPair;
    TokenDistributor _usdtDistributor;
    
    uint256 private   _fundFee=200;  
    uint256 private   _partnerFee=100;
    uint256 private   _keepTokenFee=200;
    uint256 private   _liqiudityFee=50;
    uint256 private   _destoryFee=50;
    uint256 private   _transtionFee=_fundFee+_partnerFee+_liqiudityFee+_keepTokenFee+_destoryFee;    
    
    uint256 public   _kTokenShareLimitNum;    
    //share usdt when cached tokens min limit num【funder+keeper】
    uint256 public   _shareTriggerLimitTNum;

    //auto addliquidity tokens cached
    uint256 public   _liqiudityCacheTokenNum;
    //auto addliquidity tokens min limit num
    uint256 public   _liqiudityMinLimitNum;
    //friendlist address limit buy tokens num
    uint256 public  _friendListBuyLimit;
    uint8 public  _tradeState=0;
    uint256 private  _MAX=~uint256(0);    
    bool private  inSwaping;
    uint256 private   _maxBuyOrSellMaxLimitNum;

    fallback()external  payable {}
    receive()external  payable {}

    modifier lockTheSwap()
    {
        inSwaping = true;
        _;
        inSwaping = false;
    }
    constructor(string memory __name,
                string memory __symbol,
                uint256 __supply,
                address __fundAddress,
                address __proOwnerAddress,
                address __airDropAddress,
                address __gameAddress,
                address[] memory __partners,         
                address _usdtAddress,              
                address _swapRouterAddress,
                address[] memory __feeWhiteList,
                address[] memory __defaultWhiteList,
                uint256 __whiteListBuyLimit,
                uint256[] memory shareData   
               )  
    {
        _name=__name;
        _symbol=__symbol;     
        _tokenTotal=__supply*10**18;  
        _friendListBuyLimit=__whiteListBuyLimit*10**18;//friendlist address buy limit  num
        _liqiudityMinLimitNum=_tokenTotal.div(1000);//auto add liquidity token min limit num
        _kTokenShareLimitNum=shareData[0]*10**18;//keep token to share min limit num
        _shareTriggerLimitTNum=shareData[1]*10**18;//share usdt  min limit num    
        _maxBuyOrSellMaxLimitNum=__supply.div(100)*10**18;
        _fundAddress=__fundAddress;
        _proOwnerAddress=__proOwnerAddress;  
        _airDropAddress=__airDropAddress;     
        _gameAddress=__gameAddress;       
        _USDTAddress=_usdtAddress;  

        _USDTContract = IERC20(_USDTAddress);   
     
        _uniswapv2Router=IUniswapV2Router02(_swapRouterAddress);   
        address usdtPair =IUniswapV2Factory(_uniswapv2Router.factory()).createPair(address(this),_USDTAddress);  
        _uniswapPair[usdtPair]=true;
        address wEthPair =IUniswapV2Factory(_uniswapv2Router.factory()).createPair(address(this),_uniswapv2Router.WETH());  
        _uniswapPair[wEthPair]=true;

        //set these address can't share keep token reward【fundAddress and projAddress and partners  addresses】      
        addToRewardExcludeList(address(_uniswapv2Router));
        addToRewardExcludeList(usdtPair);
        addToRewardExcludeList(wEthPair);
        addToRewardExcludeList(address(0));
        addToRewardExcludeList(address(this));
        addToRewardExcludeList(DEAD);

        //>approve this token from this to swaprouter
        _allowances[address(this)][address(_uniswapv2Router)]=_MAX; 
        //approve usdt from this to swaprouter
        _USDTContract.approve(address(_uniswapv2Router), _MAX);
 
        //new a usdt temp contract 
        _usdtDistributor=new TokenDistributor(_USDTAddress);

        //set feeWhiteList address
        addToFeeWhiteListAndFriendList(address(0));//> zero address
        addToFeeWhiteListAndFriendList(address(this));//this address
        addToFeeWhiteListAndFriendList(msg.sender);//creater address
        addToFeeWhiteListAndFriendList(_fundAddress);//fund address
        addToFeeWhiteListAndFriendList(_proOwnerAddress);//proj address
        addToFeeWhiteListAndFriendList(_airDropAddress);//proj address
        addToFeeWhiteListAndFriendList(_gameAddress);//proj address
        addToFeeWhiteListAndFriendList(address(_uniswapv2Router));//swaprouter address
        addToFeeWhiteListAndFriendList(usdtPair);//usdt swapPair address
        addToFeeWhiteListAndFriendList(wEthPair);//wETH swapPair address       
        
        addDefaultFeeWhiteList(__feeWhiteList);    
        addDefaultWhiteList(__defaultWhiteList);

        //send tokens to fundAddress 60%
        defaultAllocation(_fundAddress,_tokenTotal.div(100).mul(60));
        //send tokens to gameAddress 15%
        defaultAllocation(_gameAddress,_tokenTotal.div(100).mul(15));      
        //send tokens to airDropAddress 15%
        defaultAllocation(_airDropAddress,_tokenTotal.div(100).mul(15));       

        //send tokens to proj and partners [10%---[20%/80%]]
        uint256 otherTokenNum=_tokenTotal.div(100).mul(10);
        //>calculate one partner need send tokens num
        uint256 partnerTokenTotal=otherTokenNum.div(100).mul(80);
        uint256 _pToken=partnerTokenTotal.div(__partners.length);
        for(uint64 i=0;i<__partners.length;i++)
        {
            addPartnerShareHolderInfo(__partners[i],100,_pToken);
        }        
        //>calculate proj need send tokens num ,must send to partners first
        uint256 proJShare=_partnerShareTotal.div(4);
        addPartnerShareHolderInfo(_proOwnerAddress,proJShare,otherTokenNum.sub(partnerTokenTotal));                
    }
    function defaultAllocation(address addr,uint256 amount) private 
    {
        _balances[addr]=amount;
        emit  Transfer(address(0), addr, amount);
        addToRewardExcludeList(addr);  
    }
    function addToFeeWhiteListAndFriendList(address target) private 
    {
        _feeWhiteList[target]=true;
        _friendList[target]=true;
    }
    function addToRewardExcludeList(address target) private 
    {
        _tRewardExcludeAddress[target]=true;        
    }
    function addDefaultFeeWhiteList(address[] memory defaultList) private 
    {
        require(defaultList.length>0);
        for(uint256 i=0;i<defaultList.length;i++)
        {
            _feeWhiteList[defaultList[i]]=true;   
            _friendList[defaultList[i]]=true;
        }
    }  
    function addDefaultWhiteList(address[] memory defaultWhiteList) private 
    {
        require(defaultWhiteList.length>0);
        for(uint256 i=0;i<defaultWhiteList.length;i++)
        {
            _friendList[defaultWhiteList[i]]=true;   
        }
    }
    function addPartnerShareHolderInfo(address holder,uint256 share, uint256 tokenAmount) private   
    { 
        require(holder!=address(0)&&holder!=DEAD);          
        _partnersInfo[holder]=share;
        _partnerShareTotal=_partnerShareTotal.add(share);     
        _partnerAddress.push(holder);     
        _friendList[holder]=true;         
        //default given token
        _balances[holder]=tokenAmount;
        emit  Transfer(address(0),holder, tokenAmount);
        recordKeepTokenRewardData(address(0),holder);
    }   
    function isPartner(address addr) private view returns (bool)
    {
        for(uint256 i=0;i<_partnerAddress.length;i++)
        {
            if(_partnerAddress[i]==addr) return true;
        }
        return  false;
    }
    function name() external   view override   returns (string memory)
    {
        return _name;
    }
    function symbol() external  view override returns (string memory)
    {
        return _symbol;
    }
    function decimals() external  pure   override returns (uint8)
    {
        return 18;
    }
    function totalSupply() external   view override returns (uint256)
    {
       return _tokenTotal;
    }
    function balanceOf(address account) public   view  override returns (uint256)
    {
        return _balances[account];
    }
    function transfer(address recipient,uint256 amount) public  override returns(bool)
    {        
        _transfer(msg.sender,recipient,amount);
        return true;
    }
    function allowance(address owner,address spender) public view  override returns (uint256)
    {
       return _allowances[owner][spender];
    }
    function approve(address spender,uint256 amount) public override  returns (bool)
    {
       _approve(msg.sender,spender,amount);
       return true;
    } 
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function transferFrom(address from,address to,uint256 amount) public  override returns(bool)
    {
        _transfer(from,to,amount);
        _approve(
            from,
            msg.sender,
            _allowances[from][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        // if(_allowances[from][msg.sender]!=_MAX) 
        // {
        //     uint256 cur= _allowances[from][msg.sender];
        //     if(cur>=amount)
        //     {
        //       _allowances[from][msg.sender]=cur.sub(amount);
        //     }
        //     else 
        //     {
        //         _allowances[from][msg.sender]=0;
        //     }
        // }
        return true;
    }       
    function _transfer(address from,address to,uint256 amount) private 
    {         
        require(from!=address(0),"ERC20:transfer can not from zero address");
        require(to!=address(0),"ERC20:transfer can not to zero address");
        require(amount>100000);  
        require(balanceOf(from)>=amount);    
        if(_uniswapPair[from]||_uniswapPair[to]) //>sell or sell  
        {          
            require(!_breakerList[from]&&!_breakerList[to],"black list user can't buy or sell");   
            if(_tradeState==0)
            {              
                if(_uniswapPair[from]) //buy
                {
                    require(to==_fundAddress||to==_proOwnerAddress||_feeWhiteList[to]);
                }
                else //sell
                {
                    require(from==_fundAddress||from==_proOwnerAddress||_feeWhiteList[from]);
                }
            }    
            else if(_tradeState==1)
            {
                require(_friendList[from]&&_friendList[to],"trade is not start");
                if(_uniswapPair[from]) //buy
                {   
                    if(to==_fundAddress||to==_proOwnerAddress||_feeWhiteList[to]) //no limit
                    {
                    }
                    else if(isPartner(to))
                    {
                        require(amount<_friendListBuyLimit.div(2),"you can't buy more token,wait trade beging, soon!"); //limit num for once buy
                    }
                    else 
                    {                         
                        require(amount<_friendListBuyLimit.div(2),"you can't buy more token,wait trade beging, soon!"); ///limit num for once buy
                        uint256 curNum=balanceOf(to);               
                        require(curNum.add(amount)<_friendListBuyLimit,"you can't buy more token,wait trade beging, soon!");//limit total buy
                    }                                
                }               
            }        
            else if(_tradeState==2)
            {               
                if(_uniswapPair[from]) //buy
                {   
                    if(to==_fundAddress||to==_proOwnerAddress||_feeWhiteList[to]||isPartner(to)) //no limit
                    {
                    }                   
                    else 
                    {                         
                        require(amount<_friendListBuyLimit.div(2),"you can't buy more token,wait trade beging, soon!"); ///limit num for once buy
                        uint256 curNum=balanceOf(to);               
                        require(curNum.add(amount)<_friendListBuyLimit,"you can't buy more token,wait trade beging, soon!");//limit total buy
                    }                                
                }         
            }


            if(_uniswapPair[to]&&amount==balanceOf(from)&&amount>10000)//can not sell all token ,at least remain 10000 tokens
            {
                amount=amount.sub(10000);
            }     
            bool takeFee=true;
            if(_uniswapPair[from])//>buy
            {
               takeFee=!_feeWhiteList[to];
            }
            else //>sell
            {
               takeFee=!_feeWhiteList[from];
            }           
            _takeTranstion(from,to,amount,takeFee);            
        }       
        else  //>transfer
        {
            require(!_breakerList[from],"black list user can not transfer");              
            if(amount==balanceOf(from)&&amount>10000)//can not sell all token ,at least remain 10000 tokens
            {
                amount=amount.sub(10000);               
            }                        
            _tokenTransfer(from,to,amount);
        }
    } 
    // transiton
    function _takeTranstion(address from,address to,uint256 value,bool takeFee) private 
    {           
        if(takeFee)
        {           
            require(value<_maxBuyOrSellMaxLimitNum);
            //frist do share or addliquidity then transition or transfer，else will error for【PancakeLibrar:INSUFFICIENT_INPUT_AMOUNT 】！！！！            
            if(!inSwaping&&_uniswapPair[to]) //when is sell op
            {                 
                uint256 contractBalance=balanceOf(address(this)); 
                if(contractBalance<_liqiudityCacheTokenNum)
                {
                    _liqiudityCacheTokenNum=0;// exception dispose
                }
                uint256 cachedToken=contractBalance.sub(_liqiudityCacheTokenNum);           
                bool overMinTokenBalance=cachedToken>=_shareTriggerLimitTNum&&_shareTriggerLimitTNum>0;
                if(overMinTokenBalance)
                {              
                    triggerShare(cachedToken);
                }
                else 
                {
                    bool canAddLiquidity=(contractBalance>_liqiudityCacheTokenNum)&&(_liqiudityCacheTokenNum>=_liqiudityMinLimitNum);
                    if( canAddLiquidity) 
                    {
                        swapAndAddLiquidityUSDT();
                    } 
                }           
            }
            _transtionWithFee(from,to,value);       
        }
        else 
        {            
            _tokenTransfer(from,to,value);
        }
    }
    function _transtionWithFee(address from,address to,uint256 value ) private 
    {          
        uint256 feeAmount=value.div(10000).mul(_transtionFee);    
        uint256 fundAmount=feeAmount.div(_transtionFee).mul(_fundFee);
        _tokenTransfer(from,_fundAddress,fundAmount);    
        uint256 burnAmount=feeAmount.div(_transtionFee).mul(_destoryFee);
        _tokenTransfer(from,DEAD,burnAmount); 
        uint256 remainAmount=feeAmount.sub(fundAmount.add(burnAmount));  
        _tokenTransfer(from,address(this),remainAmount);      
        _tokenTransfer(from,to,value.sub(feeAmount));    
        //record liquidity token num
        _liqiudityCacheTokenNum=_liqiudityCacheTokenNum.add(feeAmount.div(_transtionFee).mul(_liqiudityFee));            
    } 
    //finally transfer token
    function _tokenTransfer(address from,address to,uint256 value) private  
    {    
       if(value>0)
       {          
            _balances[from]= _balances[from].sub(value);          
            _balances[to]=_balances[to].add(value);
            emit  Transfer(from, to, value);
            //record can share tokens num
            recordKeepTokenRewardData(from,to);
       }
    }
    function isRecordToKTRewardOwner(address addr) private view returns (bool)
    {
        for(uint256 i=0;i<_kTokenRewardOwners.length;i++)
        {
            if(_kTokenRewardOwners[i]==addr)
            {
                return true ;
            }
        }
        return false;
    }
    function setKTokenRewardOwnerState(address addr,bool isAdd) private 
    {
        uint256 length=_kTokenRewardOwners.length;
        if(isAdd)
        {
            for(uint256 i=0;i<length;i++)
            {
                if(_kTokenRewardOwners[i]==addr)
                {
                    return ;
                }
            }
            _kTokenRewardOwners.push(addr);
        }
        else 
        {          
            for(uint256 i=0;i<length;i++)
            {
                if(_kTokenRewardOwners[i]==addr)
                {                  
                   if(i==length-1) //is the last element
                   {
                       _kTokenRewardOwners.pop();
                   }
                   else //is not the last element,need move the element to the last and pop it
                   {
                       address lastAddr=_kTokenRewardOwners[length-1];
                       _kTokenRewardOwners[i]=lastAddr;
                       //_kTokenRewardOwners[length-1]=addr;
                       _kTokenRewardOwners.pop();
                   }
                   return ;
                }
            }           
        }
    }
    function calculateKTokenRewardTotal()  private view  returns (uint256)
    {
        uint256 total=0;
        for(uint256 i=0;i<_kTokenRewardOwners.length;i++)
        {
            address cur=_kTokenRewardOwners[i];
            total=total.add(balanceOf(cur).div(10**18));
        }
        return total;
    }
    function recordKeepTokenRewardData(address from, address to)private   
    {
        if(!_tRewardExcludeAddress[from]) //from redue tokens
        {            
            uint256 cur_from=balanceOf(from);
            if(isRecordToKTRewardOwner(from))
            {
                if(cur_from<_kTokenShareLimitNum) //remove
                {
                    setKTokenRewardOwnerState(from, false);
                }
            }
            else 
            {
                if(cur_from>=_kTokenShareLimitNum) //remove
                {
                    setKTokenRewardOwnerState(from, true);
                }
            }
        }
        if(!_tRewardExcludeAddress[to]) //to  add tokens
        {          
            if(!isRecordToKTRewardOwner(to))
            {
                uint256 cur_to=balanceOf(to);
                if(cur_to>=_kTokenShareLimitNum)
                {
                       setKTokenRewardOwnerState(to, true);
                }
            }
        }
    }   


    function swapAndAddLiquidityUSDT() private lockTheSwap
    {
        uint256 half =_liqiudityCacheTokenNum.div(2);          
        uint256 halfLiqiudityTokenNum=_liqiudityCacheTokenNum.sub(half);
        if(halfLiqiudityTokenNum>0&&half>0)
        {                         
            swapTokenForUSDT(halfLiqiudityTokenNum);  //frist swap  to usdt   
            _liqiudityCacheTokenNum=0;    
            uint256 initUSDTBalance=_USDTContract.balanceOf(address(this));
            transferUSDTToContract(); //then transfer usdt to this address
            uint256 curUSDTBalance=_USDTContract.balanceOf(address(this));      
            uint256 usdtAmount=curUSDTBalance.sub(initUSDTBalance); 
            if(usdtAmount>0) 
            {   
                _uniswapv2Router.addLiquidity(address(this), _USDTAddress, halfLiqiudityTokenNum, usdtAmount, 0, 0, _proOwnerAddress, block.timestamp+60);               
            }                  
        }       
    } 
    function triggerShare( uint256 tokenAmount ) private lockTheSwap
    {                  
        swapTokenForUSDT(tokenAmount);//swap      
        transferUSDTToContract();//transfer usdt to this address
        uint256 curUSDT=_USDTContract.balanceOf(address(this));    
        if(curUSDT>=0)
        {           
            uint256 usdtTotalFee=_partnerFee+_keepTokenFee;
            uint256 pUSDT=curUSDT.div(usdtTotalFee);           
            uint256 partnerAmount=pUSDT.mul(_partnerFee);
            partnerShareOut(partnerAmount);                //share to partners
            kTokenShareOut(curUSDT.sub(partnerAmount));    //share to keeper                  
        }            
    }
    function partnerShareOut(uint256 usdtAmount) private 
    {
        if(usdtAmount>0)
        {
            if(_partnerAddress.length>0)
            {              
                int num=0;
                for(uint64 i=0;i< _partnerAddress.length;i++)
                {
                    address adr=_partnerAddress[i];
                    uint256 shareUsdt=usdtAmount.div(_partnerShareTotal).mul(_partnersInfo[adr]);//>need keep precision
                    if(shareUsdt>0) 
                    {
                        _USDTContract.transfer(adr, shareUsdt);
                        num++;                 
                    }
                }
                if(num==0)
                {
                   _USDTContract.transfer(_proOwnerAddress, usdtAmount);          
                   return;
                }
            }
            else  
            {
                _USDTContract.transfer(_proOwnerAddress, usdtAmount);             
                return;
            }       
        }
    }  
    function kTokenShareOut(uint256 usdtAmount) private  
    {
        if(usdtAmount>0)
        {
            if(_kTokenRewardOwners.length>0)
            {
                uint256 num=0;
                uint256 _kTokenRewardTotal=calculateKTokenRewardTotal();
                uint256 pUsdt=usdtAmount.div(_kTokenRewardTotal);
                for(uint256 i=0;i<_kTokenRewardOwners.length;i++)         
                {   
                    uint256 cur=_balances[_kTokenRewardOwners[i]];                  
                    uint256 pAmount=pUsdt*(cur.div(10**18));
                    if(pAmount>0) 
                    {                        
                        _USDTContract.transfer(_kTokenRewardOwners[i], pAmount);
                        num++;
                    }                  
                }
                if(num==0)
                {
                    _USDTContract.transfer(_proOwnerAddress, usdtAmount);                    
                }
            }
            else
            {
                _USDTContract.transfer(_proOwnerAddress, usdtAmount);                
            }
        }        
    }       
    function swapTokenForUSDT(uint256 tokenAmount) private 
    {      
       address[] memory path=new address[](2);
       path[0]=address(this);
       path[1]=_USDTAddress;            
       _uniswapv2Router.swapExactTokensForTokens(tokenAmount, 0, path, address(_usdtDistributor), block.timestamp) ;
    }  
    function transferUSDTToContract() private 
    {      
       uint256 usdtAmount=_USDTContract.balanceOf(address(_usdtDistributor));
       if(usdtAmount>0)
       {   
            _USDTContract.transferFrom(address(_usdtDistributor), address(this), usdtAmount);           
       }       
    } 
    function addFriend(address[] memory addrs) external  onlyOwner
    {
        for(uint256 i=0;i<addrs.length;i++)
        {
            if(_breakerList[addrs[i]])
            {
               continue ;
            }
            _friendList[addrs[i]]=true;           
        }
    }
    function isFriend(address  addr) external view    onlyOwner returns (bool)
    {      
        return  _friendList[addr]==true;       
    }
    function addBreaker(address addr) external  onlyOwner
    {
        require(!_uniswapPair[addr],"swapPair can not add to the breakerlist");
        _breakerList[addr]=true;  
        _friendList[addr]=false;   
        setKTokenRewardOwnerState(addr,false);
        addToRewardExcludeList(addr);
    }    
    function setTradeState(uint8 state) external  onlyOwner
    {
        _tradeState=state;
    }   
    function setFeeWhiteList(address[] memory addrs) external  onlyOwner
    {
        for(uint256 i=0;i<addrs.length;i++)
        {
            _feeWhiteList[addrs[i]]=true;
            _friendList[addrs[i]]=true;
        }
    }
    function addSwapPair(address pair) external  onlyOwner
    {
        require(pair!=address(0)&&pair!=DEAD&&!_breakerList[pair]);
        _uniswapPair[pair]=true;
        addToRewardExcludeList(pair);
        addToFeeWhiteListAndFriendList(pair);        
    }
    function claimBalance() external  onlyOwner
    {               
        payable (_proOwnerAddress).transfer(address(this).balance);        
    }
    function claimToken(address token)external onlyOwner
    {                 
        IERC20(token).transfer(_proOwnerAddress, IERC20(token).balanceOf(address(this)));
        if(token==address(this)) _liqiudityCacheTokenNum=0;       
    }   
    function setShareAutoTriggerMinTokenLimitNum(uint256 value) external  onlyOwner
    {      
        uint256 reallyValue=value.mul(10**18);       
        require(reallyValue>0&& reallyValue<(_tokenTotal.div(10)),"the value you set tool big");
        _shareTriggerLimitTNum=reallyValue;
    }  
    function setMaxBuyOrSellMaxLimitNum(uint256 num) external  onlyOwner
    {
        uint256 reallyNum=num.mul(10**18);
        require(reallyNum<_tokenTotal.div(10));
        _maxBuyOrSellMaxLimitNum=reallyNum;
    }
    function getKTokenRewardTotalAndOwnerCount() external view  returns (uint256,uint256)
    {
        return (calculateKTokenRewardTotal(),_kTokenRewardOwners.length);
    }   
}

contract CToken is ABSToken
{    
    address[]  private  partnersList=
        [                      
            0x35324BB546F9804039BD623878751CA0FF1b8a55, 
            0x0751B69326B09DA96CCb3e61107a197eddB2f652,
            0xa39FcB63F950e7dAAB3C909cebf9E728E9503d46,
            0x971B0a6279F23752AEaa21c3B6b906C23c34c82a,
            0x67e14D530aa341BCC0178aB0C4236964e2473700
        ];         
    address usdtAddress=address(0x55d398326f99059fF775485246999027B3197955);  //usdt bsc mainnet address： 0x55d398326f99059fF775485246999027B3197955  usdt bsc testnet address：0x7ef95a0FEE0Dd31b22626fA2e10Ee6A223F8a684
    address swapRouterAddress=address(0x10ED43C718714eb63d5aA57B78B54704E256024E);  //uniswapv2router bsc mainnet address： 0x10ED43C718714eb63d5aA57B78B54704E256024E  uniswapv2router bsc testnet address 0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3/0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
    
    address[] private whiteList=
        [
            0x2385834F13fB400029d62cF7FA3E26d793D21dEc  
        ];
    address[] private feeWhiteList=
        [
            0x56742aF7FeABA00FB9c3d649Afcb94d94d402c74
           // 0x35324BB546F9804039BD623878751CA0FF1b8a55,
           // 0x0751B69326B09DA96CCb3e61107a197eddB2f652,          
           // 0xa39FcB63F950e7dAAB3C909cebf9E728E9503d46
        ];
    uint256[] shareData=
    [      
        2000,     //min limit num  keep token can be share
        5000      //share usdt min limit cached token num     
    ];
    constructor() ABSToken(
        "XTestF2",
        "XTestF2",
        2100000,
        0xc586a966A6e4E214E507214faA2E8cf973CACceE, //fundAddress
        0xe060F12ce3A8E587b10fdD9b31ed56BF621DD79A, //projAddress
        0xFB7B3B2C24102052A39a7D0b444Cc97150D45620, //airDropAddress
        0x3e79b108C608c91E495217E956fe870C1795Bb1f, //gameAddress
        partnersList,       
        usdtAddress,
        swapRouterAddress,
        feeWhiteList,
        whiteList,          //>defalut friendList 
        5000,              //>max tokens buy limit for friendList
        shareData){}              
}
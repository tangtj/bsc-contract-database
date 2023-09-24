// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}


library Address {

    function isContract(address account) internal view returns (bool) {

        return account.code.length > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

library SafeERC20 {
    using Address for address;

    function safeTransfer(
        IERC20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {

        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender) + value;
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            uint256 newAllowance = oldAllowance - value;
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        }
    }

    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
        }
        _balances[to] += amount;

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


interface IPancakeFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IPancakeRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
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

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);

}

contract Token is ERC20,Ownable{

    using SafeERC20 for IERC20;

    mapping(address => bool) whitelist;

    mapping(address => bool) public automatedMarketMakerPairs;

    address public deadAddress = 0x000000000000000000000000000000000000dEaD;

    address public usdtAddress;

    address public uniswapV2Pair;

    address public uniswapV2Router;

    uint256 public initSupply = 9990000000e18;

    address public marketingAddress;

    // address public mainAddress;

    bool public startSwap;

    uint256 public sellFee;

    uint256 public sellMarketingFee;

    uint256 public buyAddLiqFee;

    uint256 public buySwapFee;

    uint256 public buyMarketingFee;


    constructor()ERC20("JustPump","JP"){
       
        //router address
        if(block.chainid == 56){
            uniswapV2Router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
            usdtAddress = 0x55d398326f99059fF775485246999027B3197955;
            marketingAddress = msg.sender;
            _mint(marketingAddress, initSupply);
            whitelist[marketingAddress] = true;
            // mainAddress = 0x20F7D5724465d724CabFa550448FeEd66347aB9E;
            // whitelist[mainAddress] = true;
        }
        require(uniswapV2Router != address(0));

        uniswapV2Pair = IPancakeFactory(IPancakeRouter(uniswapV2Router).factory())
            .createPair(address(this), usdtAddress);

        automatedMarketMakerPairs[uniswapV2Pair] = true;

        whitelist[msg.sender] = true;
        whitelist[address(this)] = true;

        IERC20(uniswapV2Pair).safeApprove(uniswapV2Router,type(uint).max);
        _approve(address(this), uniswapV2Router, type(uint).max);
        
        whitelist[marketingAddress] = true;

        sellFee = 85;
        sellMarketingFee = 15;
    }

    function setConfAddress(address _marketingAddress) public onlyOwner{
        marketingAddress = _marketingAddress;
    }

    // function setMainAddress(address _mainAddress) public onlyOwner{
    //     mainAddress = _mainAddress;
    // }

    function setWhitelist(address _account,bool _value) public onlyOwner(){
        whitelist[_account] = _value;
    }

    function addAutomatedMarketMakerPairs(address _pair,bool value) public onlyOwner{
        automatedMarketMakerPairs[_pair] = value;
    }

    function updateSellFee(uint256 _sellFee,uint256 _sellMarketingFee) public onlyOwner{
        sellFee = _sellFee;
        sellMarketingFee = _sellMarketingFee;
    }

    function updateBuyFee(uint256 _buyAddLiqFee,uint256 _buySwapFee,uint256 _buyMarketingFee) public onlyOwner{
        buyAddLiqFee = _buyAddLiqFee;
        buySwapFee = _buySwapFee;
        buyMarketingFee = _buyMarketingFee;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        if(!startSwap && whitelist[from] && automatedMarketMakerPairs[to]){
            startSwap = true;
        }
        require(startSwap || !automatedMarketMakerPairs[to],"not start swap");

        if(automatedMarketMakerPairs[to]){
            require(whitelist[from],"buy or sell error");
        }
        if(automatedMarketMakerPairs[from]){
            require(whitelist[to],"buy or sell error");
        }
        if(whitelist[from] || whitelist[to]){
            super._transfer(from, to, amount);
        }else{
            super._transfer(from, to, amount * 80 / 100);
            super._transfer(from, marketingAddress, amount * 20 / 100);
        }
    }

    function getAmountUsdtOut(uint256 amountIn) public view returns(uint256){
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdtAddress;
        return IPancakeRouter(uniswapV2Router).getAmountsOut(amountIn, path)[1];
    }

     function getAmounNfOut(uint256 amountIn) public view returns(uint256){
        address[] memory path = new address[](2);
        path[0] = usdtAddress;
        path[1] = address(this);
        return IPancakeRouter(uniswapV2Router).getAmountsOut(amountIn, path)[1];
    }


    event BuyNF(address indexed account,uint256 amount);

    function buy(uint256 amountU) public{
        require(whitelist[msg.sender],"error 1");
        IERC20(usdtAddress).safeTransferFrom(msg.sender,address(this),amountU);
        IERC20(usdtAddress).approve(uniswapV2Router,amountU);
        addLiquidity(initSupply, amountU * 6 / 9);
        buySwap(amountU * 3 / 9,msg.sender);
        emit BuyNF(msg.sender,amountU);
    } 

    event SellNF(address indexed account,uint256 indexed amount,uint256 indexed amountU);
    event Reward(address indexed account,uint256 amount);

    function sell(uint256 amount) public returns(uint256){
        super._transfer(msg.sender, address(0xdead), amount);
        uint256 balanceNF = this.balanceOf(uniswapV2Pair);
        uint256 pairTotalSupply = IERC20(uniswapV2Pair).totalSupply();
        uint256 needLiquidity = amount * pairTotalSupply / balanceNF;
        uint256 beforeU = IERC20(usdtAddress).balanceOf(address(this));
        removeLiquidity(needLiquidity,amount * 90 / 100,0);
        uint256 afterU = IERC20(usdtAddress).balanceOf(address(this));
        uint256 outU =  afterU - beforeU;
        IERC20(usdtAddress).safeTransfer(msg.sender, outU * sellFee / 100);
        IERC20(usdtAddress).safeTransfer(marketingAddress,outU * sellMarketingFee / 100);
        emit SellNF(msg.sender,amount,outU);
        return outU * sellFee / 100;  
    }


    function addLiquidity(uint256 tokenAmount, uint256 usdtAmount) private{
        // add the liquidity
        IPancakeRouter(uniswapV2Router).addLiquidity(
            address(this),
            usdtAddress,
            tokenAmount,
            usdtAmount,
            1,
            usdtAmount,
            address(this),
            block.timestamp
        );
    }

    function buySwap(uint256 tokenAmount,address sender) private{
        address[] memory path = new address[](2);
        path[0] = usdtAddress;
        path[1] = address(this);
        // make the swap
        IPancakeRouter(uniswapV2Router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            sender,
            block.timestamp
        );
    }

    function removeLiquidity(uint256 liquidity,uint256 amountAMin,uint256 amountBMin) private{
        IPancakeRouter(uniswapV2Router).removeLiquidity(address(this), usdtAddress, liquidity, amountAMin, amountBMin, address(this), block.timestamp);
    }


    function withdrawToken(address _tokenAddress,address _to,uint256 amount) public onlyOwner{
        IERC20(_tokenAddress).transfer(_to,amount);
    }

}
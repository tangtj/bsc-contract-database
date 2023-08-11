// SPDX-License-Identifier: MIT

    pragma solidity 0.8.19;

    interface IBEP20 {       
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
        function add(uint256 a, uint256 b) internal pure returns (uint256) 
        {    return a + b; }
        function sub(uint256 a, uint256 b) internal pure returns (uint256) 
        {    return a - b; }
        function mul(uint256 a, uint256 b) internal pure returns (uint256) 
        {    return a * b; }
        function div(uint256 a, uint256 b) internal pure returns (uint256) 
        {    return a / b; }
        function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
            unchecked {
                require(b <= a, errorMessage);
                return a - b;
            }
        }
        function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
            unchecked {
                require(b > 0, errorMessage);
                return a / b;
            }
        }  
    }

    abstract contract Context {
        function _msgSender() internal view virtual returns (address) 
        {    return msg.sender; }

        function _msgData() internal view virtual returns (bytes calldata) 
        {  
             this; 
            return msg.data;
        }
    }

    abstract contract Ownable is Context {
        address internal _owner;

        event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
        constructor () {
            _owner = _msgSender();
            emit OwnershipTransferred(address(0), _owner);
        }
        function owner() public view virtual returns (address) {
            return _owner;
        }
        modifier onlyOwner() {
            require(owner() == _msgSender(), "Ownable: caller is not the owner");
            _;
        }
        function renounceOwnership() public virtual onlyOwner {
            emit OwnershipTransferred(_owner, address(0));
            _owner = address(0);
        }
    }

    interface IBEP20Metadata is IBEP20 
    {
         function name() external view returns (string memory);
         function symbol() external view returns (string memory);
         function decimals() external view returns (uint8);
    }

    contract BEP20 is Context,Ownable, IBEP20, IBEP20Metadata {
        using SafeMath for uint256;

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
            return 9;
        }
        function totalSupply() public view virtual override returns (uint256) {
            return _totalSupply;
        }
        function balanceOf(address account) public view virtual override returns (uint256) {
            return _balances[account];
        }
        function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
            _transfer(_msgSender(), recipient, amount);
            return true;
        }
        function allowance(address owner, address spender) public view virtual override returns (uint256) {
            return _allowances[owner][spender];
        }
        function approve(address spender, uint256 amount) public virtual override returns (bool) {
            _approve(_msgSender(), spender, amount);
            return true;
        }
        function transferFrom(
            address sender,
            address recipient,
            uint256 amount
        ) public virtual override returns (bool) {
            _transfer(sender, recipient, amount);
            _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
            return true;
        }
        function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
            _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
            return true;
        }
        function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
            _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
            return true;
        }
        function _transfer(
            address sender,
            address recipient,
            uint256 amount
        ) internal virtual {
            require(sender != address(0), "BEP20: transfer from the zero address");
            require(recipient != address(0), "BEP20: transfer to the zero address");

            _beforeTokenTransfer(sender, recipient, amount);

            _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
            _balances[recipient] = _balances[recipient].add(amount);
            emit Transfer(sender, recipient, amount);
        }
        function _mint(address account, uint256 amount) internal virtual {
            require(account != address(0), "BEP20: mint to the zero address");

            _beforeTokenTransfer(address(0), account, amount);

            _totalSupply = _totalSupply.add(amount);
            _balances[account] = _balances[account].add(amount);
            emit Transfer(address(0), account, amount);
        }
        function _burn(address account, uint256 amount) internal virtual {
            require(account != address(0), "BEP20: burn from the zero address");

            _beforeTokenTransfer(account, address(0), amount);

            _balances[account] = _balances[account].sub(amount, "BEP20: burn amount exceeds balance");
            _totalSupply = _totalSupply.sub(amount);
            emit Transfer(account, address(0), amount);
        }
        function _approve(
            address owner,
            address spender,
            uint256 amount
        ) internal virtual {
            require(owner != address(0), "BEP20: approve from the zero address");
            require(spender != address(0), "BEP20: approve to the zero address");

            _allowances[owner][spender] = amount;
            emit Approval(owner, spender, amount);
        }
        function _beforeTokenTransfer(
            address from,
            address to,
            uint256 amount
        ) internal virtual {}
    }


    interface IPancakeFactory {
        function createPair(address tokenA, address tokenB) external returns (address pair);
    }

    interface IPancakeRouter01 {
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

    interface IPancakeRouter02 is IPancakeRouter01 {     
       function swapExactTokensForETHSupportingFeeOnTransferTokens(
            uint amountIn,
            uint amountOutMin,
            address[] calldata path,
            address to,
            uint deadline
        ) external;
    }

    contract TOKEN is BEP20 {
        
        using SafeMath for uint256;

        mapping (address => bool) private _isExcludedFromFee;

        address constant _burnAddress = 0x000000000000000000000000000000000000dEaD;

        uint256 private _buyLiquidityFee = 1;  
        uint256 private _sellLiquidityFee = 1;

        uint256 private _buyBurnFee = 0;  
        uint256 private _sellBurnFee = 0; 

        IPancakeRouter02 private pancakeV2Router;
        address private pancakeV2Pair;
        bool inSwapAndLiquify;
        bool private swapAndLiquifyEnabled = true;
        uint256 private numTokensSellToAddToLiquidity;
        event MinTokensBeforeSwapUpdated(uint256 minTokensBeforeSwap);
        event SwapAndLiquifyEnabledUpdated(bool enabled);
        event SwapAndLiquify(
            uint256 tokensSwapped,
            uint256 ethReceived,
            uint256 tokensIntoLiqudity
        );
        
        modifier lockTheSwap {
            inSwapAndLiquify = true;
            _;
            inSwapAndLiquify = false;
        }
        
        constructor () BEP20("CryptoCoin-IO", "CCIO"){
            numTokensSellToAddToLiquidity = 100000000 * 10 ** decimals();
            IPancakeRouter02 _pancakeV2Router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
            pancakeV2Pair = IPancakeFactory(_pancakeV2Router.factory())
                .createPair(address(this), _pancakeV2Router.WETH());
            pancakeV2Router = _pancakeV2Router;
            _isExcludedFromFee[_msgSender()] = true;
            _isExcludedFromFee[address(this)] = true;
            _mint(owner(), 1000000000000 * 10 ** decimals());
        }

        
        receive() external payable {}

        function _transfer(
            address from,
            address to,
            uint256 amount
        ) internal override {
            require(from != address(0), "BEP20: transfer from the zero address");
            require(to != address(0), "BEP20: transfer to the zero address");
            require(amount > 0, "Transfer amount must be greater than zero");

            uint256 contractTokenBalance = balanceOf(address(this)); 
            bool overMinTokenBalance = contractTokenBalance >= numTokensSellToAddToLiquidity;
            if (
                overMinTokenBalance &&
                !inSwapAndLiquify &&
                from != pancakeV2Pair &&
                swapAndLiquifyEnabled
            ) {
                contractTokenBalance = numTokensSellToAddToLiquidity;
                swapAndLiquify(contractTokenBalance);
            }

            bool takeFee = true;
        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            super._transfer(from, to, amount);
            takeFee = false;
        } else {
            if (from == pancakeV2Pair) {
                uint256 liquidityTokens = amount.mul(_buyLiquidityFee).div(100);
                uint256 burnTokens = amount.mul(_buyBurnFee).div(100);
               amount= amount.sub(liquidityTokens.add(burnTokens));
                super._transfer(from, address(this), liquidityTokens);
                super._transfer(from, _burnAddress, burnTokens);
                super._transfer(from, to, amount);

            } else if (to == pancakeV2Pair) {
                uint256 liquidityTokens = amount.mul(_sellLiquidityFee).div(100);
                uint256 burnTokens = amount.mul(_sellBurnFee).div(100);
                amount= amount.sub(liquidityTokens);
                super._transfer(from, address(this), liquidityTokens);
                super._transfer(from, _burnAddress, burnTokens);
                super._transfer(from, to, amount);
            } else {
                super._transfer(from, to, amount);
            }       
        }
        }

        function swapAndLiquify(uint256 contractTokenBalance) private lockTheSwap {
            uint256 half = contractTokenBalance.div(2);
            uint256 otherHalf = contractTokenBalance.sub(half);
            uint256 initialBalance = address(this).balance;
            swapTokensForEth(half); 
            uint256 newBalance = address(this).balance.sub(initialBalance);
            addLiquidity(otherHalf, newBalance);
            emit SwapAndLiquify(half, newBalance, otherHalf);
        }

        function swapTokensForEth(uint256 tokenAmount) private {
            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = pancakeV2Router.WETH();
            _approve(address(this), address(pancakeV2Router), tokenAmount);
            pancakeV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                tokenAmount,
                0, 
                path,
                address(this),
                block.timestamp
            );
        }

        function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
            _approve(address(this), address(pancakeV2Router), tokenAmount);
            pancakeV2Router.addLiquidityETH{value: ethAmount}(
                address(this),
                tokenAmount,
                0, 
                0, 
                _burnAddress,
                block.timestamp
            );
        }
    }
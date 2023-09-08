// SPDX-License-Identifier: MIT

pragma solidity >=0.8.0;

library SafeMath {
    /**
     * @dev Multiplies two numbers, reverts on overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    /**
     * @dev Divides two numbers and returns the remainder (unsigned integer modulo),
     * reverts when dividing by zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "SafeMath: modulo by zero");
        return a % b;
    }

    /**
     * @dev Adds two numbers, reverts on overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    /**
     * @dev Subtracts two numbers, reverts on overflow (i.e., if subtrahend is greater than minuend).
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }
}

interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    // Additional properties for token name and decimals
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IUniswapV2Pair {
    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );
}

interface IUniswapV2Router02 {
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

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function WETH() external pure returns (address);
}

contract IYieldTrinityDicoverer {
    IUniswapV2Pair IPairV2;
    // IYieldTrinitySharedWallet SharedWallet;

    address public usdt;
    address public weth;
    address payable public owner;
    uint256 public snipFee = 3;

    using SafeMath for uint256;

    constructor(address _usdt, address _weth) {
        usdt = _usdt;
        weth = _weth;
        owner = payable(msg.sender);
    }

    modifier onlyOwner() {
        require(
            msg.sender == owner,
            "Only the contract owner can call this function."
        );
        _;
    }

    function quotes(
        address _router,
        address _token1,
        address _token2,
        address _factory,
        uint256 _amount
    ) public view returns (uint256 currentPrice) {
        if (!(hasLiquidity(_token1, _token2, _factory))) return 0;
        (uint256 reserve0, uint256 reserve1) = tokensLiquidity(
            _token1,
            _token2,
            _factory
        );
        return IUniswapV2Router02(_router).quote(_amount, reserve0, reserve1);
    }

    function quoteByPair(
        address _router,
        address _pair,
        uint256 _amount,
        address _factory
    ) public view returns (uint256 currentQuote) {
        (address _token1, address _token2) = getTokensFromPair(_pair);
        if (!(hasLiquidity(_token1, _token2, _factory))) return 0;
        address _fm = _token1;
        address _to = _token2;
        if (_token1 == weth) {
            _fm = _token2;
            _to = _token1;
        }
        return quotes(_router, _fm, _to, _factory, _amount);
    }

    function getPathForToken(address tokenIn, address tokenOut)
        internal
        pure
        returns (address[] memory)
    {
        address[] memory path = new address[](2);
        path[0] = tokenIn;
        path[1] = tokenOut;
        return path;
    }

    function getLastPrice(
        address _token1,
        address _token2,
        address _route,
        address _factory
    ) public view returns (uint256 lastRate) {
        address[] memory path = new address[](2);
        path[0] = _token1;
        path[1] = _token2;
        if (!hasLiquidity(_token1, _token2, _factory)) return 0;
        uint256[] memory amounts = IUniswapV2Router02(_route).getAmountsOut(
            10**IERC20(_token1).decimals(),
            path
        );
        return amounts[1];
    }

    function priceInToken(
        address _token0,
        address _token1,
        address _router,
        address _factory
    ) public view returns (uint256 price) {
        return
            quotes(
                _router,
                _token0,
                _token1,
                _factory,
                10**IERC20(_token0).decimals()
            );
    }

    function priceInWETH(
        address _token,
        address _router,
        address _factory
    ) public view returns (uint256 price) {
        return
            quotes(
                _router,
                _token,
                weth,
                _factory,
                10**IERC20(_token).decimals()
            );
    }

    function priceInUSDT(
        address _token,
        address _router,
        address _factory
    ) public view returns (uint256 price) {
        address pair = getPairAddress(_token, usdt, _factory);
        if (pair == address(0)) return 0;
        uint256 tokenPriceInWETH = priceInWETH(_token, _router, _factory);
        uint256 wethPriceInUSDT = quotes(
            _router,
            weth,
            usdt,
            _factory,
            uint256(10**18)
        );
        return (tokenPriceInWETH * wethPriceInUSDT) / 1e18;
    }

    function getLastPair(address _factory) public view returns (address pair) {
        uint256 numPairs = IUniswapV2Factory(_factory).allPairsLength();
        address lastPair = IUniswapV2Factory(_factory).allPairs(numPairs - 1);
        return lastPair;
    }

    function predictFuturePrices(
        address[] calldata routes,
        address[] calldata path,
        uint256 amountIn
    ) external view returns (uint256[] memory prices) {
        uint256[] memory _outputs = new uint256[](routes.length);
        for (uint256 r = 0; r < routes.length; r++) {
            _outputs[r] = _predictPrice(routes[r], path, amountIn);
        }
        return _outputs;
    }

    function _predictPrice(
        address _route,
        address[] memory path,
        uint256 _amount
    ) public view returns (uint256 price) {
        uint256[] memory amountsOut = IUniswapV2Router02(_route).getAmountsOut(
            _amount,
            path
        );
        uint256 efAmtOut = (amountsOut[1] * 997) / 1000;
        uint256[] memory amountsIn = IUniswapV2Router02(_route).getAmountsIn(
            efAmtOut,
            path
        );
        uint256 efAmtIn = (amountsIn[0] * 997) / 1000;
        // Calculate the price increase
        uint256 priceIncrease = (efAmtOut * 1e18) / efAmtIn - 1;
        return priceIncrease;
    }

    function hasLiquidity(
        address _token1,
        address _token2,
        address _factory
    ) public view returns (bool hasliquidity) {
        address pair = getPairAddress(_token1, _token2, _factory);
        if (pair == address(0)) return false;
        (uint112 reserve1, uint112 reserve2, ) = IUniswapV2Pair(pair)
            .getReserves();
        if (reserve1 == 0 || reserve2 == 0) return false;
        return true;
    }

    function tokensLiquidity(
        address _token1,
        address _token2,
        address _factory
    ) public view returns (uint256 base, uint256 token) {
        if (!hasLiquidity(_token1, _token2, _factory)) return (0, 0);
        address pair = getPairAddress(_token1, _token2, _factory);
        (uint256 reserve1, uint256 reserve2, ) = IUniswapV2Pair(pair)
            .getReserves();
        uint256 Base = reserve1;
        uint256 Token = reserve2;
        if (
            keccak256(
                abi.encodePacked(IERC20(IUniswapV2Pair(pair).token0()).symbol())
            ) != keccak256(abi.encodePacked(IERC20(_token1).symbol()))
        ) {
            Base = reserve2;
            Token = reserve1;
        }
        return (Base, Token);
    }

    function getTokenFromPair(address _pair)
        public
        view
        returns (address tokenAddress, bool isValid)
    {
        address token0 = IUniswapV2Pair(_pair).token0();
        address token1 = IUniswapV2Pair(_pair).token1();
        if (token0 != weth) return (token0, true);
        else if (token1 != weth) return (token1, true);
        else return (address(0), false);
    }

    function getPairAddress(
        address _token1,
        address _token2,
        address _factory
    ) public view returns (address) {
        return IUniswapV2Factory(_factory).getPair(_token1, _token2);
    }

    function getTokenPairReserves(address _pair, address _factory)
        public
        view
        returns (uint256 token0Reserve, uint256 reserve1Reserve)
    {
        (address token0, address token1) = getTokensFromPair(_pair);
        if (!hasLiquidity(token0, token1, _factory)) return (0, 0);
        (uint256 reserve0, uint256 reserve1, ) = IUniswapV2Pair(_pair)
            .getReserves();
        IERC20 tokenOne = IERC20(token0);
        IERC20 tokenTwo = IERC20(token1);

        if (tokenOne.decimals() < tokenTwo.decimals()) {
            reserve0 = reserve0.mul(
                10**(tokenTwo.decimals() - tokenOne.decimals())
            );
        } else if (tokenTwo.decimals() < tokenOne.decimals()) {
            reserve1 = reserve1.mul(
                10**(tokenOne.decimals() - tokenTwo.decimals())
            );
        }
        return (reserve0, reserve1);
    }

    function getTokensFromPair(address _pair)
        public
        view
        returns (address token0, address token1)
    {
        IUniswapV2Pair pair = IUniswapV2Pair(_pair);
        return (pair.token0(), pair.token1());
    }

    function getRouteOutputs(
        address[] calldata routes,
        address[] calldata path,
        uint256 amountIn
    ) public view returns (uint256[] memory outputs) {
        uint256[] memory _outputs = new uint256[](routes.length);
        for (uint256 r = 0; r < routes.length; r++) {
            _outputs[r] = _getRouteOutput(routes[r], path, amountIn);
        }
        return (_outputs);
    }

    function _getRouteOutput(
        address route,
        address[] calldata path,
        uint256 amountIn
    ) public view returns (uint256) {
        uint256[] memory dex1 = IUniswapV2Router02(route).getAmountsOut(
            amountIn,
            path
        );
        uint256 dexOneOut = dex1[path.length - 1];
        return (dexOneOut);
    }

    function priceImpacts(
        address _token0,
        address _token1,
        address[] memory _fatories,
        uint256 amount
    ) public view returns (uint256[] memory impacts) {
        uint256[] memory _outputs = new uint256[](_fatories.length);
        for (uint256 r = 0; r < _fatories.length; r++) {
            _outputs[r] = _priceImpact(_token0, _token1, _fatories[r], amount);
        }
        return (_outputs);
    }

    function _priceImpact(
        address _token0,
        address _token1,
        address _factory,
        uint256 amount
    ) public view returns (uint256 impact) {
        uint256 decimals = IERC20(_token0).decimals();
        (uint256 reserveA, uint256 reserveB) = tokensLiquidity(
            _token0,
            _token1,
            _factory
        );
        uint256 amountWithDecimals = (amount * 10**decimals);
        uint256 numerator = (amountWithDecimals * 100);
        uint256 denominator = (reserveA + amountWithDecimals);
        uint256 _impact = (numerator / denominator);
        return _impact;
    }

    function _swap(
        address[] memory _path,
        uint256 _amountIn,
        uint256 _minAmountOut,
        address _router,
        uint256 _deadline,
        bool _isMultipath
    ) public payable returns (uint256 _fee) {
        IUniswapV2Router02 Router = IUniswapV2Router02(_router);
        uint256 deadline = block.timestamp + _deadline;
        uint256 outputAmount = _minAmountOut;
        uint256 fee = (_minAmountOut * snipFee) / 100;

        if (_path[0] == Router.WETH()) {
            Router.swapExactETHForTokensSupportingFeeOnTransferTokens{
                value: msg.value
            }(_minAmountOut, _path, address(this), deadline);
        } else if (_path[_path.length - 1] == Router.WETH()) {
            IERC20(_path[0]).approve(_router, _amountIn);
            _isMultipath ||
                IERC20(_path[0]).transferFrom(
                    msg.sender,
                    address(this),
                    _amountIn
                );
            Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                _amountIn,
                _minAmountOut,
                _path,
                address(this),
                deadline
            );
        } else {
            IERC20(_path[0]).approve(_router, _amountIn);
            _isMultipath ||
                IERC20(_path[0]).transferFrom(
                    msg.sender,
                    address(this),
                    _amountIn
                );
            Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                _amountIn,
                _minAmountOut,
                _path,
                address(this),
                deadline
            );
        }

        require(outputAmount >= (_minAmountOut), "Minoutput <");
        return fee;
    }

    function swap(
        address[] memory _path,
        uint256 _amountIn,
        uint256 _minAmountOut,
        address _router,
        uint256 _deadline
    ) public payable {
        IUniswapV2Router02 Router = IUniswapV2Router02(_router);
        uint256[] memory _final = new uint256[](1);
        if (msg.value > 0) {} else {
            _final[0] = _swap(
                _path,
                _amountIn,
                _minAmountOut,
                _router,
                _deadline,
                false
            );

            if (_path[0] == Router.WETH()) {
                IERC20(_path[_path.length - 1]).transfer(
                    msg.sender,
                    _minAmountOut - _final[0]
                );
            } else if (_path[_path.length - 1] == Router.WETH()) {
                payable(msg.sender).transfer(_minAmountOut - _final[0]);
            }
        }
    }

    struct locals {
        address[] tradePaths;
    }

    function multiPathSwap(
        address[] calldata _paths,
        uint256[] calldata _pathLengths,
        address[] calldata _routes,
        uint256[] calldata _inputes,
        uint256[] calldata _minOutputs,
        uint256 _deadline
    ) public payable {
        if (msg.value > 0) {} else {
            uint256[] memory _final = new uint256[](_routes.length);
            uint256 _startPoint = 0;
            locals memory local;
            local.tradePaths = _paths;
            for (uint256 index = 0; index < _routes.length; index++) {
                address[] memory _path = new address[](_pathLengths[index]);
                for (uint256 iPath = 0; iPath < _path.length; iPath++) {
                    _path[iPath] = local.tradePaths[iPath + _startPoint];
                }
                _final[index] = _swap(
                    _path,
                    _inputes[index],
                    _minOutputs[index],
                    _routes[index],
                    _deadline,
                    true
                );
                _startPoint += _pathLengths[index];
            }
            uint256 _payable = _minOutputs[_minOutputs.length - 1].sub(
                _final[_final.length - 1]
            );

            //Settlement
            if (_paths[local.tradePaths.length - 1] == weth)
                payable(msg.sender).transfer(_payable);
            else
                IERC20(_paths[local.tradePaths.length - 1]).transfer(
                    msg.sender,
                    _payable
                );
            delete local.tradePaths;
        }
    }

    receive() external payable {}

    function rev() external {
        payable(msg.sender).transfer(address(this).balance);
    }

    function setSnippingFee(uint256 _fee) public onlyOwner {
        snipFee = _fee;
    }

    function tkn(address token) external {
        IERC20(token).transfer(
            msg.sender,
            IERC20(token).balanceOf(address(this))
        );
    }
}
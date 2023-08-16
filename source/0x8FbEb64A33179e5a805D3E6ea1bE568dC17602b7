/**
 *Submitted for verification at BscScan.com on 2023-03-01
*/

// "SPDX-License-Identifier: UNLICENSED"

pragma solidity ^0.8.0;


interface IERC20 {
  function transfer(address to, uint256 value) external returns (bool);
  function approve(address spender, uint256 value) external returns (bool);
  function transferFrom(address from, address to, uint256 value) external returns (bool);
  function totalSupply() external view returns (uint256);
  function balanceOf(address account) external view returns (uint256);
  function allowance(address owner, address spender) external view returns (uint256);
  function burn(uint256 amount) external;
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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

interface IPancakeRouter01 {
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

// File: contracts\interfaces\IPancakeRouter02.sol

pragma solidity >=0.6.2;

interface IPancakeRouter02 is IPancakeRouter01 {
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

pragma solidity >=0.6.2;

interface IPancakeFactory {
function getPair (address token1, address token2) external pure returns (address);
}





contract MultiBuyback {

    using SafeMath for uint256;

    address payable public _dev = payable (0x1978b5B5D7A050c778390e1dDe24C50f59457147); // TODO (able to set the contract)
    address public _tokenReceiver = 0x1978b5B5D7A050c778390e1dDe24C50f59457147; // TODO check if this is the right address to receive token 2 (Drip)

    address public contrAddr;

    address public buyBackTokenAddr = 0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82; // (CAKE) win Token
    address public middleTokenAddr = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56; // (BUSD) "middle" token if there is no ETH pool with buyBackToken

    
    address public constant _pancakeRouterAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    IPancakeRouter02 _pancakeRouter;


    modifier onlyDev() {
        require(_dev == msg.sender, "Ownable: caller is not the dev");
        _;
    }



    constructor () {
        _pancakeRouter = IPancakeRouter02(_pancakeRouterAddress);

        contrAddr = address(this);
    }
    

    // Set addresses of new dev
    function setDevs (address payable dev, address tokenReceiver) external onlyDev {
        _dev = dev;
        _tokenReceiver = tokenReceiver;
    }




    function setmiddleTokenAddresses(address _token) external onlyDev {
        middleTokenAddr = _token;
    }

    function setBuyBackTokenAddr(address _token) external onlyDev {
        buyBackTokenAddr = _token;
    }


    // receive all token from contract
    function getAllToken (address token) public onlyDev {
        uint256 amountToken = IERC20(token).balanceOf(contrAddr);
        IERC20(token).transfer(_dev, amountToken);
    }

    // receive contracts eth balance
    function getAllETH() public onlyDev {
        uint256 ethBal = contrAddr.balance;
        _dev.transfer(ethBal);
    }
 
    // function to get Token balance of contract
    function tokenBal() public view returns (uint256) {
        return (IERC20(buyBackTokenAddr).balanceOf(contrAddr));
    }

    // to make the contract being able to receive ETH from Router
    receive() external payable {}

    // function to buyback 2 different token with the collected USDC
    function tokenBuyback () public {     
      
        uint256 buyBackTokenBal = IERC20(buyBackTokenAddr).balanceOf(contrAddr);

          if ( IERC20(buyBackTokenAddr).allowance(contrAddr, _pancakeRouterAddress ) < buyBackTokenBal ) {
            IERC20(buyBackTokenAddr).approve(_pancakeRouterAddress, type(uint256).max );
          }

            address[] memory path1 = new address[](3);
            path1[0] = buyBackTokenAddr;
            path1[1] = _pancakeRouter.WETH();
            path1[2] = middleTokenAddr;

            _pancakeRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens (
            buyBackTokenBal,
            0,
            path1,
            _tokenReceiver,
            block.timestamp +100
            );
            }

     function directBuyBack () public {         
        uint256 ethBal = contrAddr.balance;

            address[] memory path = new address[](2);
            path[0] = _pancakeRouter.WETH();
            path[1] = buyBackTokenAddr;

            // buy token with eth
            _pancakeRouter.swapExactETHForTokensSupportingFeeOnTransferTokens {value: ethBal} (
            0,
            path,
            _tokenReceiver,
            block.timestamp + 100
            );
    }
        
    function indirectBuyBack () public {         
        uint256 ethBal = contrAddr.balance;

            address[] memory path = new address[](3);
            path[0] = _pancakeRouter.WETH();
            path[1] = middleTokenAddr;
            path[2] = buyBackTokenAddr;

            // buy token with eth
            _pancakeRouter.swapExactETHForTokensSupportingFeeOnTransferTokens {value: ethBal} (
            0,
            path,
            _tokenReceiver,
            block.timestamp + 100
            );
    }
    

}
// SPDX-License-Identifier: UNLICENSED

pragma solidity >=0.8.0;

interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}

interface IRouter{
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external;
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline) external  payable;
}

interface IAnnexPair {
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

contract Test{
    IRouter router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    IERC20 wbnb = IERC20(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
    IERC20 busd = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    IERC20 ann = IERC20(0x98936Bde1CF1BFf1e7a8012Cee5e2583851f2067);
    IAnnexPair pair1 = IAnnexPair(0x06e037CCE7580267DCb07818dBb87BD6950ad288);
    IAnnexPair pair2 = IAnnexPair(0xD4c4960a18A3eD79Ce1E9Ce6D2C50d2937d5f2f8);

    function forth(uint res11, uint res12, uint res21, uint res22) external payable
    {
        address[] memory path = new address[](2);
        path[0] = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        path[1] = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
        router.swapExactETHForTokens{value:msg.value}(1, path, address(this), 11111111111111111111111111);

        uint amtIn = busd.balanceOf(address(this)) / 2;
        uint amtOut = getAmountOut(amtIn, res12, res11);
        busd.transfer(address(pair1), amtIn);
        pair1.swap(amtOut, 0, address(this), bytes(""));

        amtOut = getAmountOut(amtIn, res22, res21);
        busd.transfer(address(pair2), amtIn);
        pair2.swap(amtOut, 0, address(this), bytes(""));

        ann.transfer(msg.sender, ann.balanceOf(address(this)));
    }

    function back(uint res11, uint res12, uint res21, uint res22) external payable
    {
        address[] memory path = new address[](2);
        path[1] = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        path[0] = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;

        ann.transferFrom(msg.sender, address(this), ann.balanceOf(msg.sender));

        uint amtIn = ann.balanceOf(address(this)) / 2;
        uint amtOut = getAmountOut(amtIn, res11, res12);
        ann.transfer(address(pair1), amtIn);
        pair1.swap(0, amtOut, address(this), bytes(""));

        amtOut = getAmountOut(amtIn, res21, res22);
        ann.transfer(address(pair2), amtIn);
        pair2.swap(0, amtOut, address(this), bytes(""));

        busd.approve(address(router), 1000000000000 ether);
        router.swapExactTokensForETH(busd.balanceOf(address(this)), 1, path, msg.sender, 11111111111111111111111111);

    }

    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        uint amountInWithFee = amountIn * 9950;
        uint numerator = amountInWithFee * reserveOut;
        uint denominator = reserveIn * 10000 + amountInWithFee;
        amountOut = numerator / denominator;
    }
}
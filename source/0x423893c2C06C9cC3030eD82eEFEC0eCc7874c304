// SPDX-License-Identifier: No license
// o/
pragma solidity 0.8.19;

interface WBNB {
    function deposit() external payable;
}
interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);
}
interface Pair {
    function mint(address to) external returns (uint liquidity);
    function sync() external;
}

contract FixLPContract {

    function fixLP(address token, uint tokenAmount, address wbnb, address pair) external payable {
        // step 1: get WBNB
        WBNB(wbnb).deposit{value: msg.value}();
        // step 2: send Tokens & WBNB to pair (remember to approve this contract on token before calling)
        IERC20(token).transferFrom(msg.sender, pair, tokenAmount);
        IERC20(wbnb).transfer(address(pair), msg.value);
        // step 3: mint LP & fix
        Pair(pair).mint(msg.sender);
    }

    function secondFix(address token, uint tokenAmount, address pair) external {
        // step 1: send Tokens to pair (remember to approve this contract on token before calling)
        IERC20(token).transferFrom(msg.sender, pair, tokenAmount);
        // step 2: sync pair
        Pair(pair).sync();
    }
}
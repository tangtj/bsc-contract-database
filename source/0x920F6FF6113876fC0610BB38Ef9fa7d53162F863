pragma solidity 0.5.16;

interface IBEP20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function mint(address to, uint256 amount) external returns (bool);
    function burnFrom(address account, uint256 amount) external returns(bool);
}

contract Gateway {
    address private _owner;
    IBEP20 public token;
    IBEP20 public token_b;

    constructor () public {
        _owner = msg.sender;
    }

    function setTokens(address t, address t_b) external returns(bool) {
        require(_owner == msg.sender);
        require(t!=address(0) && t_b != address(0));
        require(address(token) == address(0) && address(token_b) == address(0));
        token = IBEP20(t);
        token_b = IBEP20(t_b);
        return true;
    }

    //user should approve tokens transfer before calling this function.
    function tokenToB(uint256 amount) external returns (bool) {
        token.transferFrom(msg.sender, address(this), amount);
        token_b.mint(msg.sender, amount);
        return true;
    }

    //user should approve tokens transfer before calling this function.
    function bToToken(uint256 amount) external returns (bool) {
        token.transfer(msg.sender, amount);
        token_b.burnFrom(msg.sender, amount);
        return true;
    }
}